---
layout: post
title: Notes on px, pt, inch, and DPI
---

The fact that there are so many seach results on this topic tells how confusing the documents are. 

Here are the points that I find misleading when reading the documents.

###1. px, pt, in and cm are all length units.
Defining px as "device independent pixel" is really misleading. Both px and pt are length units. 1 px = 1/96 inch and 1 pt = 1/72 inch. They have nothing to do with device Independence and pixels. 

###2. DPI (Dots per inch) is a system SW setting
To convert length(expressed in px, pt, in, or cm) to physical pixels, we need DPI. It tells how many physical pixels should 1 inch be converted to. It is not a physical property of the monitor but a SW setting. On a system with a DPI setting of 96, a 1 inch line (or a 96px, 72pt, 2.54cm line), converts to a line of 96 physical pixels.

###3. DPI is also a monitor physics property
This is another point that confuses people. DPI is often used indistinguishably describing both the system SW setting and monitor physics. 
DPI as monitor physics tells how many physical pixels occupie one physical inch on a monitor. On a monitor of 96 DPI, 96 physical pixels occupy 1 physical inch.

###For example:
If a line is specified as 1 in ( or 96px, 72pt, 2.54cm) in WPF, this line is said to be logically 1 inch long. When drawing the line, WPF needs to calculate how many physical pixels to draw on the monitor. To do this, it uses the system DPI setting. If the DPI is set to 96, then WPF will paint 96 physcial pixels onto the monitor. How long this line measures physically on the monitor depends on the monitor DPI property. On a monitor of 96 DPI, this line will apear to be just 1 inch long.

###Conclusions:
1 in, 2.54cm, 96px, 72pt all define a 1 logical inch line. What unit to use is entirely a personal preference and convenience. To insist that there is any meaningful difference between all the different units only makes it confusing.

Increasing the system DPI setting makes the line longer

Buying a larger DPI display without changing the system DPI setting makes this line shorter.
