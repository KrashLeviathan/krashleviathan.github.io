---
layout:            post
title:             "Application Promotion Process for Oracle APEX Apps (Part 1)"
menutitle:         "Application Promotion Process for Oracle APEX Apps (Part 1)"
category:          Tech Musings
author:            nkarasch
tags:              Application Development, DevOps
---

One thing I’ve noticed while developing different projects over the years is that all projects are unique, yet all
projects are the same. They’re the ***same*** in that they all go through similar stages of the application development
lifecycle, such as requirements gathering, design, implementation, etc. They have similar types of components or
subsystems, such as an application server, a database, and a user interface. And they all face similar challenges, such
as authentication, authorization, and quality assurance. On the other hand, all projects are ***unique*** in that each
project has different use cases, constraints, and requirements. There are different technologies to choose from,
different services to integrate with, and different personalities and skill-sets among your team members.

Some of my most recent project work has required looking for a “***unique***” solution to the “***same***” kind of
problem. Specifically, we’ve spent some time discussing and hammering out the best approach for the application
promotion process (a common requirement for many projects) in the context of the Oracle Application Express (APEX)
development platform (a unique implementation approach for a project). The general process is well understood by
developers, but the use of Oracle APEX presents unique challenges that must be overcome to achieve repeatable, reliable
results.

For those of you who are unfamiliar with APEX, “Oracle Application Express
(APEX) is a low-code development platform that enables you to build scalable, secure enterprise apps, with world-class
features, that can be deployed anywhere.” [^1] The platform comes as part of the Oracle Database and can be used to
rapidly develop and deploy data-driven applications. We’ve used it to great effect for our clients to create web apps
that allow them to collect, manipulate, search, and report on data that is vital to their businesses. If you’d like to
learn more about APEX, please check out [this Zirous blog post][otherBlogPost]{:target="_blank"} that discusses it in
more detail.

[otherBlogPost]: https://www.zirous.com/2020/07/14/oracle-application-express-your-option-for-application-customization/

This blog post will be presented in two parts. In this first part, we will discuss the broader topics of application
environments, application promotion, and the challenges that come with performing app promotion (for apps in general and
for APEX applications specifically). In Part 2 we will discuss the three main items of concern for APEX app promotion:
promoting the APEX Application from one workspace to the next; promoting database objects, such as packages, views, and
table schemas; and promoting data used by the application, such as static lookup table records.

## Application Environments

As mentioned above, the APEX platform comes as part of the Oracle Database. That is, all the source code for your
application actually lives *in the database...* yes, in database tables, packages, and other objects [^2]. As you can
imagine, it would be too risky to actively develop an application within the database that stores all your precious
data. You need a place where you can safely develop your application without accidentally breaking anything important
that would impact your business. Then, once the app has been deemed “safe” and does what you intend it to do, only
*then* can you move it into “production” where real users interact with it to perform regular business tasks.

What I briefly described above is the concept of “application environments”, and it’s a normal part of most app
development, regardless of whether APEX is used or not. [IBM][ibmLink]{:target="_blank"} defines an environment as
“a user-defined collection of resources that hosts an application.” Not including your local environment (running the
app on your machine), a project typically has three environments:

[ibmLink]: https://www.ibm.com/support/knowledgecenter/SS4GSP_7.0.4/com.ibm.udeploy.doc/topics/app_environment.html

1. The *Dev* environment is used by developers to test features currently under development.
2. The *QA* [^3] environment is used to test stable versions of the application before they are released to production.
3. The *Prod* environment contains the live production instance of the application.

How does an application get from one of these environments to another? This is accomplished by a process called
application promotion.

## What is Application Promotion?

In loose terms, “application promotion” is the process of moving some version of your application across different
environments. The goal of app promotion is to successfully deliver changes to your production application with little to
no negative impact on its users. You’ll also see this process referred to as “code promotion” or “server promotion”, but
I like the term “app promotion” because it encompasses all aspects of the app that may need to flow through quality
assurance: code changes, dependency updates, server or subsystem changes, network configuration changes, database data
changes, etc.

The word *promotion* captures the idea of moving from a lower level to a higher level. Most often you promote the app
from Dev to QA or from QA to Prod [^4]. For example, when developing a feature or fixing a bug in an application, developers
may deploy the app to the Dev environment several times so that they can make sure the changes they made have the
desired effect. Once they’re confident in the solution, it can then be *promoted* to the QA environment, where QA
testers, product owners, and/or a handful of other stakeholders can verify that the changes are effective and that no
other functionality in the app regressed [^5]. If testing is successful in the QA environment, the changes will then be
promoted into the Prod environment during the next scheduled release. In any case, having a systematic promotion process
will help to ensure a smooth, successful transition from “feature in development” to “feature in production.”.

## Challenges with Promoting APEX Applications

The general challenge with app promotion is that the application logic (code) should transfer from one environment to
the next without changing, but any environment-specific configuration details used in the code should remain... well...
environment-specific. It is best practice to make the environments as similar as possible so that the app behavior is
consistent across environments; however, it’s unavoidable that there will be configuration differences between them.
They will likely be hosted on different servers, use different credentials and connection information at integration
points, and have different network configurations. For example, one of my current projects integrates with a payment
processing service, but it’s configured differently in each environment. The Dev and QA environments use a test biller
account so that payments don’t actually charge real money to anyone’s credit card.

<figure>
   <img src="{{site.baseurl}}/assets/apex-promotion/apex-promotion-1.png"
        alt="Diagram showing app promotion from Dev to QA"/>
   <figcaption>
       The application logic is promoted from Dev to QA without changing, but the configuration details are
       environment-specific.
   </figcaption>
</figure>

One of the challenges with APEX app promotion is that it can’t leverage the same technologies, tools, and services
commonly used by more traditional [^6], file-based development projects. Traditional projects often use popular
third-party CI/CD [^7] tools like Jenkins, Travis, or TeamCity for their DevOps [^8] pipelines. An example of this
approach would be one of our projects (a Java/Struts application) that uses BitBucket Pipelines to automatically build
and run unit tests whenever code is pushed or merged in the repository. Once the pipeline’s “build” is complete, it’s
then possible to automatically deploy the main build artifact to a given environment depending on certain conditions. At
that point, the promotion process becomes very hands-off, which helps remove the potential for human error.
Unfortunately, because the APEX development platform is inseparable from the APEX application, you’re not able to
(easily [^9]) leverage these types of third-party tools to build, test, report on, and deploy your application.

Is this, then, an indictment on APEX as a web application development platform? Not necessarily. As with most
architecture decisions, the decision to use APEX has its tradeoffs. The biggest benefit to using APEX is that, depending
on the use case, you can develop a robust, secure application in a fraction of the time that it would take with another
technology stack. A tradeoff for this rapid development benefit is that the promotion process is a bit less automated
than you might see elsewhere; however, the platform does a pretty good job of making the manual promotion steps
relatively pain-free.

### Overcoming these challenges

As I mentioned at the start, despite all projects being similar in many respects, they are also all unique. Each project
has different use cases, constraints, and other requirements. As such, implementing the application promotion process
can and will look different from one project to the next. Given the complexity of modern application development and the
unique challenges that face every project, it is never a bad idea to bring a strategic partner on board to help overcome
those challenges.

With 35+ years of experience navigating the ever-changing technological landscape, Zirous can help. Our experts can
offer technical guidance for your unique project requirements, help your team implement best-practice development
processes, provide development services as needed, and ensure the successful delivery of a high-quality enterprise
solution. Working with Zirous means having a partner that cares not only about the immediate needs of your project but
also about the long-term success of your organization. For more information or to find out if Zirous would be a good
partner for your next project, please [contact us][zirousContactUs]{:target="_blank"}!

[zirousContactUs]: http://www.zirous.com/contact

## Stay tuned for Part 2!

In this first part we’ve gotten an overview of several key concepts and laid the groundwork for understanding the APEX
application promotion process. Now that we know what application environments are, how applications get moved from one
environment to another, and what the general challenges are, we can move onto the specifics of how to accomplish this
within the context of the APEX development platform.

[Click here to continue on to Part 2](/blog/tech%20musings/Application-Promotion-Process-for-Oracle-APEX-Apps-pt2)

- - -

This post was originally published on the Zirous blog and can be found at
[https://www.zirous.com/2021/03/05/application-promotion-process-for-oracle-apex-apps/](https://www.zirous.com/2021/03/05/application-promotion-process-for-oracle-apex-apps/){:target="_blank"}

- - -

[^1]: [https://apex.oracle.com/en/](https://apex.oracle.com/en/){:target="_blank"}
[^2]: These special APEX tables and objects aren’t accessed by the developer directly — rather, they’re manipulated by the APEX platform.
[^3]: The Quality Assurance (QA) environment is also commonly referred to as the Test environment
[^4]: In certain “hotfix” situations you may also need to promote the app from Dev to Prod.
[^5]: A *regression* is when something that used to be working in the application is now broken.
[^6]: The “traditional” custom-developed web apps I’m referring to are those that combine a general purpose language with a web framework in that language to form the core application logic. Examples of popular language/framework combinations: Java/Spring, TypeScript/Angular, Python/Django, etc.
[^7]: From Wikipedia: In software engineering, CI/CD or CICD generally refers to the combined practices of continuous integration and either continuous delivery or continuous deployment.
[^8]: [Wikipedia](https://en.wikipedia.org/wiki/DevOps){:target="_blank"} defines DevOps as a set of practices that combines software development (Dev) and IT operations (Ops). It aims to shorten the systems development life cycle and provide continuous delivery with high software quality.
[^9]: I won’t go as far as to say it’s impossible, but for most projects I don’t know that it would be practical.
