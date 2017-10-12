---
layout:    project
permalink: "/project/browser_cli"
author:    nkarasch
keywords:  Software Engineering portfolio project ISU iastate ComS319 Dart shell
title:     browser_cli
menutitle: browser_cli
visible:   false
excerpt:   A command line interface for the browser.
imageurls:
  - "/assets/projects/browser_cli_screenshot_1.png"
  - "/assets/projects/browser_cli_screenshot_2.png"
  - "/assets/projects/browser_cli_block_diagram.png"
--- 

This project explored creating a command line interface that runs in the browser,
combining two types of interfaces into a unique user experience. The application
was written in Dart, a language developed by Google that transpiles to JavaScript
to run in the browser.

The hardest part of the project was, of course, trying to replicate a bash command
line with process management. For it to be useful to users at all, it needed to
provide core functionality that users expect from a command line. Scope creep
started to slow down development momentum, so eventually I had to nail down what
features I wanted in release v1.0.0 and settle for additional features in later
releases. The desired functionality for the first release was as follows:

- Interactive IO
- Process management
- Environment variable storage, recall, and persistence
- Command completion (via the tab key)
- Input history (accessible with the up and down keys)
- A set of standard library processes

The set of standard library processes in v1.0.0 includes:

<dl>
    <dt>authentication</dt>
    <dd>loads user/session information from the document cookies and (in later
        versions) will authenticate the user’s credentials.</dd>
    <dt>echo</dt>
    <dd>prints the supplied input back to the shell.</dd>
    <dt>export</dt>
    <dd>creates a variable that persists across browser sessions via cookies.</dd>
    <dt>help</dt>
    <dd>offers help information for the command line interface.</dd>
    <dt>jobs</dt>
    <dd>lists all processes that are currently running.</dd>
    <dt>printenv</dt>
    <dd>prints all environment variables.</dd>
    <dt>testinput</dt>
    <dd>demonstrates how to get user input from within a process and how to call
        other commands from within a process.</dd>
    <dt>unset</dt>
    <dd>erases an environment variable.</dd>
</dl>

<div class="md-card shadow education">
    <div class="title icon-link">
        <h2>Links and Other Info</h2>
    </div>
    <dl class="coursework">
        <dt>Code Repository</dt>
        <dd><a href="https://github.com/KrashLeviathan/browser_cli" target="_blank">
            github.com/KrashLeviathan/browser_cli
        </a></dd>
        <dt>Website</dt>
        <dd><a href="https://pub.dartlang.org/packages/browser_cli" target="_blank">
            pub.dartlang.org/packages/browser_cli
        </a></dd>
        <dt>Started</dt>
        <dd>18 August 2016</dd>
        <dt>Completed</dt>
        <dd>27 September 2016</dd>
    </dl>
</div>
