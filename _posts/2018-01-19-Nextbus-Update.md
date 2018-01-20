---
layout:            post
title:             "Nextbus Project Update"
menutitle:         "Nextbus Project Update"
category:          Tech Musings
author:            nkarasch
tags:              Projects
---

<figure>
   <img src="{{site.baseurl}}/assets/projects/nextbus_update_screenshot_20180119.png"/>
</figure>

## Oldie but a Goodie

Today I revisited an old project I wrote a couple years
ago in C++ named
[nextbus](/project/nextbus){:target="_blank"}. It's
surprising to me that the smallest, most insignificant
things I've written tend to be the most useful and have
the longest lifespan. I guess it aligns with the Unix
philosophy of
["Do one thing, and do it well"](https://en.wikipedia.org/wiki/Unix_philosophy#Do_One_Thing_and_Do_It_Well){:target="_blank"}, right?
The nextbus app isn't anything spectacular. Heck, it
isn't even written that well! (What do you expect after
only one semester's worth of C/C++ training?) However,
it accomplishes its mission: giving me timely bus route
information with a few quick keystrokes without having
to leave the shell.

The functionality I wanted to add was to be able to list
the bus stop numbers for any given route. I had some adventures
on unfamiliar bus routes today, and (as usual) I found
using the [Cyride website](http://www.cyride.com/){:target="_blank"}
to be frustrating. I knew what
route I needed to take, but without knowing the stop number,
I was unable to use my nextbus app to get time predictions.
Time to update my app!

Luckily it wasn't too big of a change. Despite being written
by a novice C/C++ developer, the code design was good, so
adding the new feature was a clean and pain-free process.
I made the necessary updates, but I only made a minimal
effort in cleaning up the code [^1]. I added an `.editorconfig`
file, reformatted all the files, and cleaned up some imports,
but I didn't do much else. Not really worth the effort at
this point. Any cruft, despite being a bit unsightly in
places, is harmless. On top of that, I'm the only one using
the app, so there's nobody else to care about it being perfect!

## Future Plans

<aside>
   <figure class="right">
      <img src="{{ "/assets/amazon-echo.jpg#right" | absolute_url }}" />
	  <figcaption>The Amazon Echo (2nd Gen)</figcaption>
   </figure>
</aside>

I know, per the
[nextbus project page](/project/nextbus){:target="_blank"},
I previously had
"future plans" to implement a Raspberry Pi LCD display to
show nextbus predictions. However, over Christmas we got a
new Amazon Echo, and I've been itching to try to write something
for it. I think it could be a cool project to get nextbus
working over the Echo. Just imagine it...

> "Alexa, when's my next bus to school?"

<blockquote style="border-left: #146eb4 0.4em solid;">
  <p>"Your next bus will arrive in seven minutes."</p>
</blockquote>

> "Gee Alexa, you're a lifesaver!"

<blockquote style="border-left: #146eb4 0.4em solid;">
  <p>"Go get 'em, tiger. *wink wink*"</p>
</blockquote>

That *would* be pretty sweet, right? `;-)`

<div style="float:none;clear:both;"></div>

- - -

[^1]: *NOTE*: Previously I had only ever worked on C/C++ projects in emacs, so opening it in CLion made me cringe at the tab/space combinations of indentation in places! Yay for modern IDEs!
