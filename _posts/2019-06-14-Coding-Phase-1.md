---
layout: post
title: Coding Phase-1 Starts!!
subtitle: Here comes the cool stuff you have been waiting for
bigimg: /img/path.jpg
---

Before I plan to **Improve Diff Handling** I had to first find the libraries and add some features to them so that I could use them in coala.
I already had an idea what I wanted my diffs to look like. Next step was to find the right libraries to get the job done.
The following week began my hunt to find the right libraries to use to generate **Binary and XML Diffs**. 

I had to weigh a variety of factors alongside each other and see the code and diff output I would be most comfortable with to come to a decision.

I narrowed the libraries down and ultimately decided upon the winners. They were easy to use and had all the right functionality 
I needed from them. 

## multidiff - A binary diffing library

This library was easy to install and use. I became familiar with the working of the library and brainstormed some new features I would like to be added to make the library better.

When you ran this library on some big files the output looked almost unreadable. 

I began with adding a patch to set up travis for the github repo. Then I added some additional arguments to make the output
more readable and structured.

First I had added a **width** argument using which the user could specify the width the bytes could be printed to per line.
The user had an option to set the width to **max** which would allow to print the bytes throughout the width of the console.

The output can further be enhanced by allowing the user to print how many bytes he wanted per line.
I added a **bytes** argument which allowed this. The output looks like below - 

**bytes = 10**
~~~
000000: 30 31 32 33 34 35 36 37 38 39 |0123456789|
00000a: 61 62 63 64 65 66<span class='delete'> </span>  |abcdef|
~~~

**bytes = 6**
~~~
000000: 30 31 32 33 34 35 |012345|
000006: 36 37 38 39 61 62 |6789ab|
00000c: 63 64 65 66<span class='delete'> </span>  |cdef|
~~~

Now when you are only changing 2 bytes it doesn't make sense to print a hexdump for the whole diff of the two binary files. 
So I added a **diff** argument for printing only the address where the bytes were changed.I have a branch ready for a PR, which will soon make the dream real. 

By adding the diff argument you get a much smaller output with the required info where the bytes have changed. I also showed the
deleted bytes when the diff argument is used which was not shown in the library originally.

**multidiff bin_file1 bin_file2**
~~~
000000:<span class='delete'> </span>02 00 02 00 00 00 03 00 03 00 00 00 04 00 04 00 
000010: 00 00 05 00 05 00 00 00 06 00 06 00 00 00 07 00 
000020: 07 00 00 00 08 00 08 00 00 00 09 00 09 00 00 00 
000030: 0a 00 0a 00 00 00 0b 00 0b 00 00 00 0c 00 0c 00 
000040: 00 00 0d 00 0d 00 00 00 0e 00 0e 00 00 00 0f 00 
000050: 0f 00 00 00 10 00 10 00 00 00 11 00 11 00 00 00 
000060: 12 00 12 00 00 00 13 00 13 00 00 00 14 00 14 00 
000070: 00 00 15 00 15 00 00 00 16 00 16 00 00 00 17 00 
000080: 17 00 00 00 18 00 18 00 00 00 19 00 19 00 00 00 
000090: 1a 00 1a 00 00 00 1b 00 1b 00 00 00 1c 00 1c 00 
0000a0: 00 00 1d 00 1d 00 00 00 1e 00 1e 00 00 00 1f 00 
0000b0: 1f 00 00 00 20 00 20 00 00 00 21 00 21 00 00 00 
0000c0: 22 00 22 00 00 00 23 00 23 00 00 00 24 00 24 00 
0000d0: 00 00 25 00 25 00 00 00 26 00 26 00 00 00 27 00 
0000e0: 27 00 00 00 28 00 28 00 00 00 29 00 29 00 00 00 
0000f0: 2a 00 2a 00 00 00 2b 00 2b 00 00 00 2c 00 2c 00 
000100: 00 00 2d 00 2d 00 00 00 2e 00 2e 00 00 00 2f 00 
000110: 2f 00 00 00 30 00 30 00 00 00 31 00 31 00 00 00 
000120: 32 00 32 00 00 00 <span class='insert'>33 00 33 00 00 00</span>
~~~

**multidiff bin_file1 bin_file2 --diff**
~~~
000000:<span class='delete'>01 00 01 00 00 00</span> 02 00 02 00 00 00 03 00 03 00 00 00 04 00 04 00
000120: 32 00 32 00 00 00 <span class='insert'>33 00 33 00 00 00</span>
~~~

It was good news that the miantainer for that library was active ;-]

## xmldiff - The ultimate python library for xml diffs

The guys who made this did not fool around. Some serious work was put into this library. The diff algorithm was based on a 
[published paper](http://ilpubs.stanford.edu:8090/115/1/1995-46.pdf) which I haven't read fully till now. 

I thought of some changes to improve here. This library already had most of the things which I was looking for.
I researched around while looking for a proper framewrok for XML Diffs and luckily I found a useful [document](https://tools.ietf.org/html/rfc5261). I focused my work on getting xml diffs using the same approach as was given in the doc.

I looked into the code and searched around for some issues the library might have which can be fixed but couldn't find any(Assuming typos don't count ;))

The xmldiff library already had a lot of features. The library didn't mention unused namespaces as that is considered useless behavior. 
I thought I would add an issue and send a patch to mention this but decided against it in the end.

During this time I also brushed up on my XML knowledge and learnt quite a bit about XML Schemas, Namespaces and XLST Transformations.


## Preparing for the Journey Ahead
After deciding these libraries the first half of my Phase-1 comes to an end. 
Now that I've got the libraries to work with, Time to get some work done on the coala side.
