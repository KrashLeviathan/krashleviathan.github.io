---
layout:    project
permalink: "/project/codable_media_mashup"
author:    nkarasch
keywords:  Software Engineering portfolio project ISU iastate ComS319 ANTLR Java Programming Language
title:     Codable Media Mashup
menutitle: Codable Media Mashup
visible:   false
excerpt:   A language. A command line tool. A quick and dirty video editor for the power user.
imageurls:
  - "/assets/projects/comm_banner.png"
  - "/assets/projects/comm_screenshot_1.png"
  - "/assets/projects/comm_screenshot_2.png"
--- 

## Overview

Codable Media Mashup (CoMM) is both a language and a command line tool used for
slicing and splicing online videos together. The purpose is to allow people to
quickly and efficiently make "highlights reel" or “compilation” types of movies
using a collection of online videos. The traditional video editing process might
look something like this:

- Individually download the videos, and **wait** for them to complete.
- Load them into a bulky video editing application, and **wait** for it to process
  each video for thumbnails.
- Select clips from the various source videos, adjust the time sliders, **wait**
  for the CPU to catch up as the graphics card gets bogged down.
- **Waiting, waiting, waiting...** you get the point.

By contrast, with a “codable” video editing experience, users can avoid the wait!
Or at least most of it. For many people, video editing shouldn’t need a
feature-heavy application. It just needs to put pieces of videos together. The
less time spent doing it, the better! In addition to it being faster, all the
downloading and processing is done in the same contiguous block of time. This
allows the user to “set it and forget it” so they can work on something else
until their video is complete.

<div class="md-card shadow education">
    <div class="title icon-link">
        <h2>Links and Other Info</h2>
    </div>
    <dl class="coursework">
        <dt>Code Repository</dt>
        <dd><a href="https://github.com/KrashLeviathan/codable_media_mashup" target="_blank">
            github.com/KrashLeviathan/codable_media_mashup
        </a></dd>
        <dt>Started</dt>
        <dd>1 December 2017</dd>
        <dt>Completed</dt>
        <dd>11 December 2017</dd>
    </dl>
</div>


## Potential use cases:

- Game footage highlights from the season
- Top ten funniest Seinfeld moments
- Cat fail compilations
- Caruso one-liners montage
- Compilation of every time Neo says "No", "Why", or "I don't understand". Wake up Neo…

## Project Complexity

This project involved creating a fully functioning interpreter that performs
lexical analysis, grammar parsing, translation, and execution. Up until this
point, we have only created lexers and parsers with ANTLR. The interpreter
had to be able to provide helpful error messages and output useful log files
as well, which involved understanding StandardError/StandardOutput
redirection in a Java program running a bash script.

## Creation, Analysis, and Evaluation

Creation began by defining what we wanted the language to do and writing the
grammar from that definition. We focused on getting end-to-end integration
and a working prototype before drilling down on the finer details of the
program. Translation and execution were implemented by using a bash script
as an intermediate language, and then running that script from within the
Java application. The basic features of the program allow the user to define
the output video file name and cache, download numerous videos, slice them
into shorter clips, and then join those clips into a single video.

Once the initial prototype was functional, it was analyzed and improved
upon in additional iterations. The repository was completely restructured
and refactored to comply to better coding standards and to promote
readability and maintainability. The process involved decoupling distinct
modules, restructuring the file directory, updating build scripts, removing
cruft, refactoring variables and functions, and adding comments and
documentation.

The application and project goals were evaluated intermittently during
development, and plans had to be adjusted to arrive at a satisfactory end
result within the allotted time. We determined that it was more important
to produce a stable application with a few core features and robust error
handling than it was to make a buggy, unstable application with lots of
features. Original plans for a client-server web application were shifted
to target a command line application, and extra features were tabled to
focus on the more essential pieces of functionality.

The result, Codable Media Mashup, is a fun new way to edit videos with a
few easy commands!
