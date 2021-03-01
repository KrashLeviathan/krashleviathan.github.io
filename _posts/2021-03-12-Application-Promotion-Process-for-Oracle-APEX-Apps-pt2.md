---
layout:            post
title:             "Application Promotion Process for Oracle APEX Apps (Part 2)"
menutitle:         "Application Promotion Process for Oracle APEX Apps (Part 2)"
category:          Tech Musings
author:            nkarasch
tags:              Application Development, DevOps
---

Promoting web applications across environments (e.g. from Dev to QA or from QA to Production) is a necessary step in
most application development lifecycles. Performing this promotion process within the context of the Oracle Application
Express (APEX) development platform offers unique challenges that must be addressed to establish a systematic,
repeatable process. This is the second part of a two-part blog. In [Part 1][part1Link] we discussed the broader topics
of application environments, application promotion, and the challenges that come with performing app promotion. Having
laid the groundwork, we will now discuss the three main items of concern for APEX app promotion: promoting the APEX
Application from one workspace to the next; promoting database objects, such as packages, views, and table schemas; and
promoting data used by the application, such as static lookup table records.

[part1Link]: /blog/tech%20musings/Application-Promotion-Process-for-Oracle-APEX-Apps-pt1

## Promoting APEX Applications

In general, promotion is accomplished by exporting the App from one environment and importing it into another. Details
about the process can be found in [the Oracle documentation][oracleExportDoc]{:target="_blank"}, but it’s fairly
straightforward once you get the hang of it. Most of the time it only takes about 5 minutes and a dozen clicks,
starting with selecting the “Export” item from the “App Builder” menu.

[oracleExportDoc]: https://docs.oracle.com/database/apex-18.1/HTMDB/exporting-an-application-and-application-components.htm

<figure>
   <img src="{{site.baseurl}}/assets/apex-promotion/apex-promotion-2.png"
        alt="Screenshot of the APEX development interface showing the Export action within the App Builder menu."/>
   <figcaption>
       The first step in App promotion is to export the Database Application.
   </figcaption>
</figure>

When planning your promotion process for an APEX app, a really great place to start is the following Oracle whitepaper:
[Life Cycle Management with Oracle Application Express][oracleLifecycleDoc]{:target="_blank"}. It’s a few years
old (2016), but it still captures the essential steps and concepts needed for success. Rather than regurgitate the
paper here, I’ll offer a quick summary of some of the highlights and then offer some additional tips and advice based
on our experience conducting APEX app promotion. *Please note that I’m using “app” or “application” to refer to the
system in a broad sense, and I’m using “App” or “Application” to refer to the specific APEX Application entity within
an APEX workspace.*

[oracleLifecycleDoc]: https://www.oracle.com/technetwork/developer-tools/apex/learnmore/apex-life-cycle-management-wp-3030229.pdf

### Write your PL/SQL logic in packages

It is best practice to develop the majority of your business logic inside database objects (instead of within
Application attributes and components) to facilitate code reuse and to help separate the presentation layer from the
business layer. Most of the code you write will be contained in packages, though it can also be helpful to create
standalone functions, procedures, or views when appropriate. All of these database objects will have to be promoted
separately from the Application, but I’ll discuss more about promoting database objects in a little bit.

### Export the entire Application(s)

When you initially setup your QA and Prod environments, export the entire APEX workspace. For every promotion
thereafter, you should do Application exports. Try to avoid promoting individual pages or components unless absolutely
necessary. During the import step, make sure to check the option to use the same App ID. This will ensure internal
variable names and external links that reference the application ID will not break.

### Keep all exports

It’s a good idea to save the application exports in an artifact repository or object store with relevant date/time/build
information so that A) you can promote the same version to Prod that you tested with in QA, and B) so you can re-import
that version if a rollback is needed.

### Avoid conflicting code changes with other developers

When you’re working with other developers on the same application, use good development practices and communicate when
you’re working on certain pages or database objects. Unlike file-based development, most APEX development happens on the
same running instance of the application and on the same database objects. If you and another developer are working on
the same database package without coordinating your work, you may unintentionally overwrite the other developer’s
changes. If you’re working on a page in APEX, utilize the “page lock” feature to prevent others from making simultaneous
changes to that page.

### Environment-specific configuration using Substitution Strings

While most of your environment-specific details should be placed in a database object (more on this later), if you need
environment-specific variables within the App proper, you can define these as Substitutions on the “Edit Application
Definition” page and then update the Supporting Objects so that APEX prompts a user for environment-specific
substitution values when the App is imported into a workspace (i.e. when it gets promoted).

<figure>
   <img src="{{site.baseurl}}/assets/apex-promotion/apex-promotion-3.png"
        alt="Screenshot of the APEX development interface showing Substitution Strings."/>
   <figcaption>
       Substitution Strings can be used to store environment-specific values.
   </figcaption>
</figure>

<figure>
   <img src="{{site.baseurl}}/assets/apex-promotion/apex-promotion-4.png"
        alt="Screenshot of the APEX development interface showing Supporting Object substitutions."/>
   <figcaption>
       By editing the Supporting Objects, you can define which Substitution Strings the user should be prompted to
       change when performing an App installation.
   </figcaption>
</figure>

## Promoting Database Objects

Promoting the APEX Application is only one piece of the puzzle. If done correctly, most of your application logic should
be written in database objects (packages, procedures, functions, views, etc.) rather than in the APEX development
interface. This makes the code more reusable, since you can reference the database objects within multiple APEX
components. It can lead to better, less error-prone code, since you can use other tools to run static analysis on the
code. It also makes the code more maintainable, since you can easily check in
[DDL (Data Definition Language)][ddlLink]{:target="_blank"} scripts to your source control and use the scripts to move
code across environments.

[ddlLink]: https://www.w3schools.in/mysql/ddl-dml-dcl/#DDL

One useful tool for promoting database objects is the SQL Developer “Database Diff” tool, which can be used to compare
object definitions between two databases. In addition to offering line-by-line differences for objects, it also
generates the SQL command needed to update the destination database. For example, if a discrepancy is found with one of
the columns in a table, the Database Diff tool produces the `ALTER TABLE` statement needed to update the column
definition for the destination database table. If you’re using scripts from source control to promote database objects
to the next environment, the database diff tool can be a good follow-up step to ensure the scripts didn’t miss any
important code changes.

<figure>
   <img src="{{site.baseurl}}/assets/apex-promotion/apex-promotion-5.png"
        alt="Screenshot of the Database Diff tool in SQL Developer."/>
   <figcaption>
       The “Database Diff” tool in SQL Developer.
   </figcaption>
</figure>

### Environment-specific configuration using database packages

Rather than using Substitution Strings to store configuration details, it is usually better to create a package, table,
or some other database object (or combination of objects) to store and retrieve this information. For
environment-specific configuration within database packages, there are various approaches you can take, each with its
own tradeoffs. I won’t dive into the specifics of each approach, but here are some things to consider when designing
your solution:

- In general, you want to be able to move code cleanly between environments without having to make any
  environment-specific modifications to the code during promotion.
- It can be helpful to put all environment-specific data references in a single package rather than have them spread
  across your code base. For example, you can create an `ENVIRONMENT_PROPERTIES_PKG` package that contains a bunch of
  getters for various pieces of information. Calling the same getter in different environments should return the
  appropriate value for that environment.
- Avoid putting sensitive data (e.g. credentials) in plain text in your code. Securing sensitive data within an Oracle
  database is beyond the scope of this article, so make sure to talk to your DBA about safe storage options if this
  comes up in your project.

Now that we’ve talked about promoting database ***schema***, let’s talk about promoting database ***data***.

## Promoting Data

Sometimes you will need to promote the data within one or more tables in the database. Usually this is for lookup tables
or other types of static information that doesn’t (or shouldn’t) change from environment to environment. A simple
example would be a lookup table that populates values in a dropdown menu. I remember in one project we defined a dynamic
navigation menu where the menu item details (including which roles could access which menus or submenus) were stored
within a table underlying a view that could be queried by all the Apps in the workspace. In cases like these, the
application logic heavily depends on the state of the data to achieve the desired functionality, so it’s important to
include “data promotion” in the systematic promotion process rather than arbitrarily applying SQL changes as the need
arises. This especially becomes important when you have multiple developers working on a project. If Developer 1 makes
inserts/updates to Table A in the Dev environment, Developer 2 needs to know what changes to make when promoting the
application to the QA environment, and the changes need to be made consistently.

In general, it’s best practice to put all data changes within
[DML (Data Manipulation Language)][dmlLink]{:target="_blank"} scripts that are then checked into version control.
These scripts can then be executed in the QA and Prod environments during promotion. Additionally, there are various
[tools][comparisonTools]{:target="_blank"} that you can use to compare and sync data between two environments. Since
there are many options out there and the best option may vary depending on the needs of the project, I won’t go into
too much detail here, other than to say that this should be a step in your promotion checklist.

[dmlLink]: https://www.w3schools.in/mysql/ddl-dml-dcl/#DML

[comparisonTools]: https://dbmstools.com/categories/data-compare-tools/oracle

One final piece of advice regarding the promotion of static lookup table data: You would be wise to add a “lookup code”
column to these types of tables and refer to the code whenever specific values are used in the application logic. This
is important because neither a generated id column nor a display name column is guaranteed to remain consistent over
time or across environments. A three-character string (defined as `CODE VARCHAR2(3 BYTE) UNIQUE NOT NULL`)
makes a good lookup code column in most instances.

## Conclusion

Regardless of the exact details of your promotion plan, the important thing is to ***have a plan***. Document the
process and create a checklist for developers to use as they promote the application from one environment to the next.
Remember, the overall goal of app promotion is to successfully deliver changes to your production application with
little to no negative impact to its users. By having an established systematic approach, you will contribute to the
overall quality of your application and avoid costly mistakes.

With 35+ years of experience navigating the ever-changing technological landscape, Zirous can help. Our experts can
offer technical guidance for your unique project requirements, help your team implement best-practice development
processes, provide development services as needed, and ensure the successful delivery of a high-quality enterprise
solution. Working with Zirous means having a partner that cares not only about the immediate needs of your project but
also about the long-term success of your organization. For more information or to find out if Zirous would be a good
partner for your next project, please [contact us][zirousContactUs]{:target="_blank"}!

[zirousContactUs]: http://www.zirous.com/contact
