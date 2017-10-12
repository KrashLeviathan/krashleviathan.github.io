---
layout:            post
title:             "That Which Cannot Be Crawled"
menutitle:         "That Which Cannot Be Crawled"
category:          Tech Musings
author:            nkarasch
tags:              Web Crawler Sinkhole
---

In the spring of 2017, one of the
[programming assignments](https://github.com/KrashLeviathan/coms311_pa2){:target="_blank"}
for my Algorithms class included the implementation of a web crawler. We had been covering
graph traversal algorithms, so naturally this led to the use of web crawlers to
discover the vertices in a web graph. This semester we are once again re-visiting
web crawlers in my Algorithms for Large Data Sets class.

As I sat through lecture today taking notes, it occurred to me that you could create
a small website to trap an unsuspecting web crawler. The mechanics are fairly
simple. Web crawlers all use some form of graph traversal&mdash;either
Breadth-first Search (BFS) or Depth-first Search (DFS)&mdash;to discover pages
in a web graph. These algorithms use a queue to store vertices (i.e. web pages) not
yet visited, and while that queue is not empty, they will continue to extract elements
from the front for processing.

Here's a brief review of the BFS algorithm (as applied to a web crawler) in pseudocode:

```
Input: a seed url
Output: all vertices (urls) reachable directly or indirectly through links from the seed

D = {seed}    // D is the set of all urls that have been discovered
Q = {seed}    // Q is a queue of all urls that have been discovered but not yet visited

while (Q is not empty):
    let e = the first url element popped from Q
    output e
    parse out all the links contained in e
    for each link url u in e:
        if D does not contain u;
            add u to D
            add u to Q
```

What happens, then, if you endlessly encounter new links on each new page you visit?
If that were the case, the queue would continue to fill, essentially creating an
infinite while loop.

Creating such a site only really takes two files. An `.htaccess` file (shown below)
uses the RewriteEngine to make all urls point to `index.php`.

```
RewriteEngine on
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(.*)$ index.php?q=$1 [L,QSA]
```

The `index.php` file generates HTML content containing random URI's within the domain.

```php
<?php
/*
 * Create a random string
 * @author	XEWeb <http://www.xeweb.net/2011/02/11/generate-a-random-string-a-z-0-9-in-php/>
 * @param $length the length of the string to create
 * @return $str the string
 */
function randomString($length = 20)
{
    $str = "";
    $characters = array_merge(range('A', 'Z'), range('a', 'z'), range('0', '9'));
    $max = count($characters) - 1;
    for ($i = 0; $i < $length; $i++) {
        $rand = mt_rand(0, $max);
        $str .= $characters[$rand];
    }
    return $str;
}

/*
 * The quotes and narrative are taken from the movie "Labyrinth" (1986) and
 * were found on IMDB.com. I thought they were fitting...
 */
$quotes = array('Don\'t go on...',
    'Go back, while you still can...',
    'This is not the way...',
    'Take heed, and go no further...',
    'Beware, beware...',
    'Soon it will be too late...');
?>

<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>That Which Cannot Be Crawled</title>
</head>
<body>
    <?php
    for ($i = 0; $i < 6; $i++) {
        echo '<p><a href="/' . randomString() . '">' . $quotes[$i] . '</a></p>';
    }
    ?>
</body>
</html>
```

You can view the code [on GitHub](https://github.com/KrashLeviathan/thatwhichcannotbecrawled){:target="_blank"}
or check out the [live demo here](http://thatwhichcannotbecrawled.krashdev.com/){:target="_blank"}. I've added
some extra flair, but the premise remains the same.

So what is the probability you see any given URL? Since the characters are randomly
chosen from among `[a-zA-Z0-9]`, there are 62 possibilities for each character. If
the URI portion of the URL is 20 characters long, that's 62<sup>20</sup> unique
combinations! Or, approximately 7&times;10<sup>35</sup> pages. By comparison, as
of last year, Google only knew of about
[130 trillion pages](https://www.seroundtable.com/google-130-trillion-pages-22985.html)
(1.3&times;10<sup>14</sup>).

Why would anyone want to do this? Well, my initial use case (childish, I know) would be to
punish web crawlers that disregard the `robots.txt` file on the website. One of purposes
of a [robots file](http://www.robotstxt.org/){:target="_blank"} is to tell web crawlers
which pages NOT to crawl. However, it's up to the crawler to actually abide by that policy.
In my demo code, the robots file tells web crawlers not to visit ANY of the pages on the
site. If they choose to ignore it, the fault is theirs!

Obviously, there are ways to avoid getting stuck in such a trap. If the crawler processes
each page and stores it's document signature/fingerprint, it can refuse to add links to the
queue if the page is similar to another page it's already visited. This could potentially be
defeated by generating the page as randomly as possible to avoid any two pages being
similar. I didn't dive down that rabbit hole, though, as I was just interested in
exploring the concept. I'd love to hear if anyone else has seen or experimented with this though!
