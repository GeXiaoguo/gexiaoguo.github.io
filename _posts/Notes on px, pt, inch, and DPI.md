---
layout: post
title: Notes on px, pt, inch, and DPI
---

The fact that there are so many seach results on this topic tells how confusing the documents are. 

Here are the points that I find misleading when reading the documents.

1. px, pt, in and cm are all length unit.
Defining px as "device independent pixel" is really misleading. Both px and pt are length units. 1 px = 1/96 inch and 1 pt = 1/72 inch. They have nothing to do with device Independence and pixels. 

2. DPI (Dots per inch) is a system SW setting
To convert length(expressed in px, pt, in, or cm) to physical pixels, we need DPI. It tells how many physical pixels should 1 inch be converted to. It is not a physical property of the monitor but a SW setting. On a system with a DPI setting of 96, a 1 inch line (or a 96px, 72pt, 2.54cm line), converts to a line of 96 physical pixels.

3. DPI is also a monitor physics property
This is another point that confuses people. DPI is often used indistinguishably describing both the system SW setting and monitor physics. 
DPI as monitor physics tells how many physical pixels occupie one physical inch on a monitor. On a monitor of 96 DPI, 96 physical pixels occupy 1 physical inch. 
