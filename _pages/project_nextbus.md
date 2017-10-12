---
layout:    project
permalink: "/project/nextbus"
author:    nkarasch
keywords:  Software Engineering portfolio project ISU iastate ComS327 C++ CyRide NextBus
title:     nextbus
menutitle: nextbus
visible:   false
excerpt:   Get NextBus predictions through a command line interface.
imageurls:
  - "/assets/projects/nextbus_screenshot.png"
  - "/assets/projects/nextbus_raspberry_pi.jpg"
--- 

This program interfaces with the NextBus REST API to display NextBus information for CyRide in Ames. It can return bus route information in a variety of formats, based on user input of route, stop, etc. Users are also able to configure the program so that they can quickly find out the time of the next bus heading home or the time of the next bus heading to school/work without having to manually type all the optional parameters. The program is meant to be usable with other bus agencies (not just CyRide), but my target for release was CyRide, so no other agencies were tested extensively during development.

The program, which was written in C++, curls the NextBus information with an http request, which returns the data in XML format. A custom XML parser was written to pull out relevant information to display to the user. XML data that can be reasonably cached and referenced later is saved in the directory “$HOME/.nextbus/” along with a configuration file with user-specific settings.

The most practical usage of the program is the saved route and stop feature. When you're working late into the night on a project and you're ready to go home, just type `nextbus home` and get bus arrival estimations without ever having to open the browser or pull out a phone!

Another application of nextbus I am currently exploring is the use of the program with an LED seven-segment display integrated with a Raspberry Pi. I added the `-m | --minutes` command line parameter to allow the program to only output the minutes remaining until the next bus leaves for a given stop. The program is then executed every 30 seconds from a python script running on the Pi, and the output is fed into the LED display. The proof of concept is operational — my next step is to move the logic to a Raspberry Pi Zero (a model the size of a credit card), design an enclosure in Solidworks, 3D print the enclosure, and mount it on my wall for daily use!

<div class="md-card shadow education">
    <div class="title icon-link">
        <h2>Links and Other Info</h2>
    </div>
    <dl class="coursework">
        <dt>Code Repository</dt>
        <dd><a href="https://github.com/KrashLeviathan/nextbus" target="_blank">
            github.com/KrashLeviathan/nextbus
        </a></dd>
        <dt>Started</dt>
        <dd>13 April 2016</dd>
        <dt>Completed</dt>
        <dd>20 April 2016</dd>
    </dl>
</div>
