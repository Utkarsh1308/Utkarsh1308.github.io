---
layout: post
title: Coding Phase-1 Starts!!
subtitle: Here comes the cool stuff you have been waiting for
bigimg: /img/path.jpg
---

Before I plan to **Improve Diff Handling** I had to first find the libraries and fix them so that I could use them in coala.
I already had a fair idea what I wanted my diffs to look like. Next step was to find the right libraries to get the job done.
The following weel began my hunt to find the right library to use to generate **Binary and XML Diffs**. 
I narrowed the libraries down and ultimately decided upon the winners. They were easy to use and had all the right functionality 
I needed from them. 

## Multidiff - A binary diffing library

I had to do a lot of improvements to this library to get my desired behaviour. This library was the best to work with once I 
added some cool features to it. When you ran this library on some big files the output looked almost unreadable. So first I had 
add a width argument which would make the output at least readable by printing a specified number of bytes per line. 

Now when you are only changing 2 bytes it doesn't make sense to print 1000 other irrelevant bytes. So how about a diff attribute
which would only print the required bytes. I have a branch ready for a PR, which will soon make the dream real. It was good news
that the miantainer was active ;-]

## xmldiff - The ultimate python library for xml diffs

The guys who made this did not fool around. Some serious work was put into this library. The diff algorithm was based on a 
[published paper](http://ilpubs.stanford.edu:8090/115/1/1995-46.pdf) which I haven't read fully till now. There was nothing
to improve here. They had everything I was looking for. But understanding the code and how everything was connected
took me a day or two anyway. Better safe than sorry.

So with these 2 libraries decided the First half of my Phase-1 comes to an end. 
Now that I've got the libraries to work with, Time to get some work done on the coala side. Maybe I'll add some code snippets the next
time ;)
