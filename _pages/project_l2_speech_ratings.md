---
layout:    project
permalink: "/project/l2_speech_ratings"
author:    nkarasch
keywords:  Software Engineering portfolio project ISU iastate WLC second language audio rating speech
title:     L2 Speech Ratings
menutitle: L2 Speech Ratings
visible:   false
excerpt:   A web application for rating audio files.
imageurls:
  - "/assets/projects/l2speechratings_01.png"
  - "/assets/projects/l2speechratings_02.png"
  - "/assets/projects/l2speechratings_03.png"
  - "/assets/projects/l2speechratings_04.png"
  - "/assets/projects/l2speechratings_05.png"
  - "/assets/projects/l2speechratings_06.png"
  - "/assets/projects/l2speechratings_07.png"
  - "/assets/projects/amt_l2_speech_ratings_screenshot_1.png"
  - "/assets/projects/amt_l2_speech_ratings_screenshot_2.png"
  - "/assets/projects/amt_l2_speech_ratings_screenshot_3.png"
--- 

## Overview

L2 Speech Ratings was a project I was contracted to develop for
[Dr. Charles Nagle](https://language.iastate.edu/portfolio_page/nagle/){:target="_blank"},
a professor at the Iowa State University's college of
[World Languages and Cultures](https://language.iastate.edu/){:target="_blank"}.
His field of research was in second-language learning. For his research, he recorded
second-language learners (students) as they read different prompts in their second language
(mostly Spanish). The recordings, then, were played for native or near-native speakers,
who would then rate the recorded voices on fluency, comprehension, and accent.

Previously, this had all been accomplished via manual processes. The goal of the project
was to automate some of that process through a web platform. The raters, rather than having
to come into the office to rate audio clips, should be able to login on their own computer
from home and have the clips and rating form presented to them through a web interface.
The data collected from raters would then be stored in a database and made downloadable
to Dr. Nagle. All of this and more was accomplished by the end of the project.


## Features

The features of the application are mostly captured in the screenshots above, so I invite
you to look through the images to see some of the various functionality that was built.
The features include:

- User authentication and login via Google OAuth2. There were two classes of users: Raters and
  Administrators.
- A demographics form (not shown) to collect information about the raters. These demographics
  could then be downloaded by the administrators as a CSV file.
- An audio file management system, with the ability to upload and delete audio files. The audio
  files would automatically sort themselves into appropriate surveys based on language and task.
- Survey management capabilities, so that administrators could create, close, or administer
  different batches of audio files to the raters.
- Survey instructions and "Consent to participate" displayed to the raters before taking a survey.
- When a survey starts, the audio files in the given survey are auto-played to the rater, who
  then selects a number for each metric collected. These ratings are collected by the platform
  and made downloadable to the administrators.
- Quality assurance measures were put into place to try to enforce quality responses from
  participants. For example, ratings for a file could not be submitted before the entire clip
  was played from start to finish.


## Project Lifespan

The project took about a month to develop, with another half-month spent debugging issues
that arose during deployment to Iowa State's servers. The platform was used to collect ratings
over the summer of 2017, but in the fall Dr. Nagle decided to move the project in a different
direction to collect ratings through
[Amazon Mechanical Turk (AMT)](https://www.mturk.com/mturk/welcome){:target="_blank"}.
This would allow him to collect ratings through a much bigger platform from a much wider
audience (worldwide). It also allowed him to integrate participant payment into the system,
which my platform did not do. I developed the AMT code for Dr. Nagle, and it has been
deployed and used to collect ratings from all over the world. The time spent developing
the original platform, however, was not wasted! I learned many valuable lessons over the
course of the project, and I am still very proud of the final result.


<div class="md-card shadow education">
    <div class="title icon-link">
        <h2>Links and Other Info</h2>
    </div>
    <dl class="coursework">
        <dt>Code Repository</dt>
        <dd><a href="https://github.com/KrashLeviathan/L2_speech_ratings" target="_blank">
            github.com/KrashLeviathan/L2_speech_ratings
        </a></dd>
        <dt>Started</dt>
        <dd>31 March 2017</dd>
        <dt>Completed</dt>
        <dd>24 May 2017</dd>
    </dl>
</div>
