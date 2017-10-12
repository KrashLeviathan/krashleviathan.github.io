---
layout:    page
permalink: "/project/smartsync"
author:    nkarasch
keywords:  Software Engineering portfolio project ISU iastate SE339 Angular2 Spring boot Java microservices
title:     SmartSync
menutitle: SmartSync
visible:   false
excerpt:   An online dashboard application to sync a user's accounts, IoT devices, and other tech.
--- 

<h3 class="author-info">An online dashboard application to sync a user's accounts, IoT devices, and other tech.</h3>

<div class="album">
    <img src="{{site.baseurl}}/assets/projects/smartsync_screenshot_01.png">
    <img src="{{site.baseurl}}/assets/projects/smartsync_screenshot_02.png">
    <img src="{{site.baseurl}}/assets/projects/smartsync_screenshot_03.png">
    <img src="{{site.baseurl}}/assets/projects/smartsync_screenshot_04.png">
    <img src="{{site.baseurl}}/assets/projects/smartsync_screenshot_05.png">
    <img src="{{site.baseurl}}/assets/projects/smartsync_screenshot_06.png">
    <img src="{{site.baseurl}}/assets/projects/smartsync_screenshot_07.png">
    <img src="{{site.baseurl}}/assets/projects/smartsync_screenshot_08.png">
    <img src="{{site.baseurl}}/assets/projects/smartsync_screenshot_09.png">
</div>


## Overview

SmartSync is a home management system for synchronizing Internet of Things (IoT) devices, family members’ online accounts, and other user applications. It runs as a web application, interfacing with various services to provide a more efficient home for the modern family.

Our Minimal Viable Product for Smart Sync consisted of the following:

- Provide a platform that allows the user to synchronize and manage Internet of Things devices and services.
- Integrate at least one device or service from each of the following categories: Internet of Things Devices, Online Account Services, and Internet Services
- Provide a “Dashboard” view that displays the status of multiple devices/services simultaneously
- Provide household account registration with a household admin user and one or more standard users
- Implement basic security measures for the system (password encryption, input validation, etc.)


<div class="md-card shadow education">
    <div class="title icon-link">
        <h2>Links and Other Info</h2>
    </div>
    <dl class="coursework">
        <dt>Authors</dt>
        <dd>Nathan Karasch, Jack Meyer, Gregory Steenhagen, and Trevor Hendersen</dd>
        <dt>Front-end Repository</dt>
        <dd><a href="https://github.com/KrashLeviathan/SmartSync-Dashboard" target="_blank">
            github.com/KrashLeviathan/SmartSync-Dashboard
        </a></dd>
        <dt>Server-side Repository</dt>
        <dd><a href="https://github.com/jackcmeyer/SmartSync" target="_blank">
            github.com/jackcmeyer/SmartSync
        </a></dd>
        <dt>Demo Video</dt>
        <dd><a href="https://youtu.be/OpqfT2fiN9k" target="_blank">
            youtu.be/OpqfT2fiN9k
        </a></dd>
        <dt>Architecture Evolution</dt>
        <dd><a href="{{site.baseurl}}/assets/pdfs/SmartSync_Architecture_Evolution.pdf" target="_blank">
            SmartSync_Architecture_Evolution.pdf
        </a></dd>
        <dt>Started</dt>
        <dd>February 2017</dd>
        <dt>Completed</dt>
        <dd>May 2017</dd>
    </dl>
</div>


## Software Features

### Login Authentication with Google

We chose Google’s OAuth2 API to authenticate our application because it is almost never a good idea to tackle authentication on your own. Google’s API is well-documented, but we ran into problems due to the fact that we were using Angular2 for a single-page application. On successful authentication, Google’s API reloads the page or redirects the page. This didn’t work completely well with our system since the entire application is supposed to be navigated without leaving the page. In retrospect, the architecture would have benefitted from having the login and registration pages as static HTML instead of being part of the main Angular2 application. We could have avoided the page redirect issues, and the architecture would have been simplified.

### User Management

If a user creates a new household, he is automatically the admin user of that household. Admin users have the ability to invite other users to the household, remove them from the household, and give or revoke admin privileges to other users.

The admin user view looks slightly different from the regular user view. The “Manage Users” and “Household Settings” pages become available, and in the “Services” page there are additional actions that can be performed by the admin, such as adding a new service to the household, configuring services, etc.

The difference between admin user privileges and regular user privileges was something we had to address in our architecture, and it took most of the semester to figure out. The authentication service is talked about later, so it won’t be elaborated on here.

### User and Household Settings

Every user is allowed to make limited customizations to their profile. Their picture, email, name, etc are tied to their Google account, so any changes pertaining to that should be done at their Google account screen. The “User Settings” page features a nice color-selector component for changing the theme of an individual user’s dashboard. The choice of Angular2 made the addition of this component to the architecture fairly simple, since the entire front end is using npm for dependency management. It was a matter of installing the component, inserting it into the UI, and hooking up the form fields to the component. However, there wasn’t enough time to implement the theme colors in the application. They’re simply stored with the user’s preferences on the server.

With the security requirements came the need for input validation. Input validation is done on the front and back end, with Angular2 and Bootstrap elements helping notify the user of invalid form data before sending it to the server.

### Dashboard View

The main user interface is the Dashboard View, which lets the user view and interact with the different services in the household. For customization, the user can add more services and rearrange the tiles to fit his or her own preferences. The dashboard settings are linked to the user’s account, so that each time they open SmartSync, they see the same dashboard that they customized before.

The JavaScript library packery was used for tile packing, and the draggabilly library was used for dragging/dropping the tiles to rearrange them.

### Service Management

Every user has the ability to view the services in the household, but the admin user has the privilege to add, remove, and configure services. The services presented the biggest architectural problem that we had to solve, since a system like this needs to be able to handle a multitude of service types, each with their own unique qualities and attributes.

On the front end, Angular2 uses TypeScript, which allows classes and class extension for your components. There is a component for the generic service, which handles UI features common to all services (the bordered tile box, the functionality to move around the dashboard, etc.). The concrete service instances then have their own view and controller code specific to the service type. So, for example, a lightbulb service has an On/Off switch, a to-do list has an editable list of to-do items, and so on down the line for each new service.

The back end then handles each service by spinning up a new server within the microservice architecture. This will be discussed later in the report.

### Other Features

Since SmartSync operates as a single-page application using lots of AJAX calls, it was important to have a method of informing the user when operations succeeded or failed. A small alert API was written to make it easy for any part of the application to post an alert to the top of the page. The alerts disappear after a certain timeout, or the user can click the X in the corner to close them. When more than one alert is displayed, they are stacked vertically, with the older alerts pushed down to make room for newer alerts at the top.

We had originally planned to let each user create and manage multiple dashboard views. The UI was written to handle the view management; however, we didn’t have time to fully implement and integrate the backend aspects of this feature.
