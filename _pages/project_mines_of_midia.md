---
layout:    project
permalink: "/project/mines_of_midia"
author:    nkarasch
keywords:  Software Engineering portfolio project ISU iastate SE329 MIDI audio sheet music
title:     Mines of MIDIa
menutitle: Mines of MIDIa
visible:   false
excerpt:   Learn to play drums with graphical MIDI playback and sheet music.
imageurls:
  - "/assets/projects/mines_of_midia_screenshot_1.png"
  - "/assets/projects/mines_of_midia_screenshot_2.png"
  - "/assets/projects/mines_of_midia_screenshot_3.png"
  - "/assets/projects/mines_of_midia_screenshot_4.png"
  - "/assets/projects/mines_of_midia_screenshot_5.png"
--- 

The goal of this project was to create a web application that helps individuals
learn to play the drumset. The web app features the ability to play MIDI files
while seeing a graphical representation of the drums being played. The main GUI
consists of two parts: 1) a drum set image that highlights the different drums
as the music plays, and 2) a music staff that shows the notes in traditional
music notation.

The original name, "The Mines of MIDIa," is a play on words alluding to a
fictional location in the Middle-earth realm of J.R.R. Tolkien literature. It
was in the Mines of Moria that Gandalf read aloud from the last few lines in
the Book of Mazarbul, "We hear drums, drums in the deep." A bit nerdy, yes,
but such was our sense of humor at times! As of November 2016, Charlie (Gregory)
was planning on continuing work on the site under the less nerdy name,
[http://drumitdown.com/](DrumItDown.com).

The site makes use of a JavaScript library called MIDI.js for processing the
MIDI files. It wasn't a very well-documented library, so it took a bit of
experimentation to get everything up and running. I helped get things up and
running, but my main contribution was parsing the MIDI drum tracks into staff
music. I learned a lot about the MIDI file format and SVG markup in the process.

The main problem in converting MIDI into staff music is that everything in MIDI
is event based, and everything in music notation is context-dependent. For
example, every event in MIDI is defined in terms of the "ticks" since the last
event, with ticks referring to a specified length of time. For audible notes
themselves, there are "NoteOn" events and "NoteOff" events. These had to be
converted to music notes on the staff by quantizing the summed "ticks" value
for a NoteOn event and adding it to a custom tree structure for each measure.
The tree structure helps determine where to place rests in the measure, since
rests (the absence of notes) is completely context-dependent and isn't
represented in MIDI at all.

<div class="md-card shadow education">
    <div class="title icon-link">
        <h2>Links and Other Info</h2>
    </div>
    <dl class="coursework">
        <dt>Code Repository</dt>
        <dd><a href="https://github.com/csteamengine/drum_it_down" target="_blank">
            github.com/csteamengine/drum_it_down
        </a></dd>
        <dt>Live Website</dt>
        <dd><a href="http://www.drumitdown.com/" target="_blank">DrumItDown.com</a></dd>
        <dt>Started</dt>
        <dd>18 October 2016</dd>
        <dt>Completed</dt>
        <dd>3 November 2016</dd>
    </dl>
</div>
