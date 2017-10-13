---
layout:    project
permalink: "/project/beatnik_encryption"
author:    nkarasch
keywords:  Software Engineering portfolio project ISU iastate ComS319 encryption esoteric language Cliffle
title:     Beatnik Encryption
menutitle: Beatnik Encryption
visible:   false
excerpt:   Get hip to the scene by encrypting any string as an avant-garde poem.
imageurls:
  - "/assets/projects/beatnik_tux.png"
  - "/assets/projects/beatnik_screenshot_1.png"
  - "/assets/projects/beatnik_screenshot_2.png"
--- 

Beatnik Encryption is a JavaScript program that encrypts any string as an avant-garde poem.
The “poem,” in reality, is a Beatnik program that prints out the given string. In theory,
then, to encrypt any file, you just need to base64 encode it before passing it through the
encrypter. To decrypt the file, you need to run the program in a Beatnik interpreter, base64
decode the resulting string, and presto! -- you're back to the original message! We used a
JavaScript interpreter written by Arjeim Icallun and hosted on the
[Carnegie Mellon Computer Club website](http://www.club.cc.cmu.edu/~rjmccall/beatnik.js){:target="_blank"}.

## What is a Beatnik program?

The Beatnik programming language, written by Cliff L. Biffle (aka Cliffle) in 2001, is a
syntax-free stack language that uses the Scrabble score of the tokens as op codes and
parameter values. According to [Cliffle’s website](http://cliffle.com/esoterica/beatnik.html){:target="_blank"}:

>A Beatnik program consists of any sequence of English words, separated by any sort of
>punctuation from spaces to hyphens to blank pages. Thus, “Hello, aunts! Swim around
>brains!” is a valid Beatnik program, despite not making much sense.


## But... why?

According to Merriam-Webster, the definition of
[encrypt](http://www.merriam-webster.com/dictionary/encrypt){:target="_blank"} is “to
change (information) from one form to another especially to hide its meaning.” Encryption
plays a big role in computer science, and it is ubiquitous to technology today. While we
make no claims about the overall security of hiding a message in the code of an esoteric
programming language, we certainly feel it qualifies as a form of encryption (albeit weak).
It also satisfied our desire to make something both practical and humorous.


## Programs writing programs

One of the things that initially intrigued us about the project was the prospect of writing
a program that writes a program. This, we felt, was the main “new and complex” element that
was being explored. Solving the problem required us to get an understanding of how stack
languages work, and that was also new territory, though not terribly complex. We also
wanted to make the poems feel like poems instead of just looking like long strings of
random words chained together. To that effect, we put measures in place to sprinkle
whitespace and punctuation throughout based on distribution percentages.


## What words are we using to generate the poems?

When the page is loaded, we make a call to
[RandomText.me’s gibberish API](http://www.randomtext.me/api/gibberish/p-5/100){:target="_blank"},
which gives us back a string full of nonsense. We parse the words out of the nonsense, give
them Scrabble scores, and attempt to add them to our own database of word/score pairs. If
the word is not already in the database, it gets added. You can actually see in the browser
console how many words are added each time the page is loaded. They’re added in four batches,
so there are four messages in the console that read, “Learned # new words!” If this project
is ever extended in the future, it would be a good idea to explore some other word-generation
APIs, since RandomText.me’s API has a limited selection. The “learning” growth starts to level
out between 3000 and 4000 words.

<div class="md-card shadow">
    <div class="title icon-briefcase">
        <h2>View the full write-up</h2>
    </div>
    <div class="content">
        <iframe src='{{site.baseurl}}/assets/pdfs/Portfolio2-BeatnikEncryption.pdf' frameborder='0' style="width:100%;"></iframe>
    </div>
</div>

<div class="md-card shadow education">
    <div class="title icon-link">
        <h2>Links and Other Info</h2>
    </div>
    <dl class="coursework">
        <dt>Authors</dt>
        <dd>Nathan Karasch and Gregory Steenhagen</dd>
        <dt>Code Repository</dt>
        <dd><a href="https://github.com/KrashLeviathan/beatnik_encryption" target="_blank">
            github.com/KrashLeviathan/beatnik_encryption
        </a></dd>
        <dt>Live Website</dt>
        <dd><a href="http://beatnikencryption.krashdev.com/" target="_blank">
            beatnikencryption.krashdev.com/
        </a></dd>
        <dt>Started</dt>
        <dd>12 October 2016</dd>
        <dt>Completed</dt>
        <dd>13 November 2016</dd>
    </dl>
</div>
