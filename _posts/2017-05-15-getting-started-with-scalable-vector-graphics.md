---
layout: post
title:  "[WEB DESIGN] Getting Started With Scalable Vector Graphics (SVG)"
comments: true
date:   2017-05-15 15:07:33 +0700
categories: web_design
---

Hi guys!

After longtime I back with my blog. Today let start with SVG

### What is SVG?

SVG stand for Scalable Vector Graphics it is used to define graphic for web site.
Let me show for you some example. I definitely that you will like it and it easy to learn. Let's go yeah!!!
And we will use `<svg>` tag to start draw

#### Draw Line

To draw the line we need to use the `<line>` tag

**Code**

```
<svg>
  <line x1="10" y1="20" x2="300" y2="200" style="stroke:#000; stroke-width:5"/>
</svg>
```

**Result**
<div style="text-align: center">
  <svg style="background: #e7e7e7;">
    <line x1="10" y1="20" x2="300" y2="200" style="stroke:#000; stroke-width:5"/>
  </svg>
</div>

**x1 x2 y1 y2  What are those?**

Let take a look this photo I draw.

**X1:** The start position of the line on the x-axis
**Y1:** The start position of the line on the y-axis
**X2:** The end position of the line on the x-axis
**Y2:** The end position of the line on the y-axis

With (X1, Y1) => We will get the point **A**

With (X2, Y2) => We will get the point **B**

**Connect 2 points A & B** We will get the line **AB**


<div style="text-align: center">
  <img src="https://cloud.githubusercontent.com/assets/6791942/26051292/f27b2d66-398b-11e7-8edf-9dc02f0e0b6e.jpg" alt="toado">
</div>


Now I sure that you can get a little bit meaning of SVG

#### Draw Rectangles

#### Draw Polygons

#### Draw Circle



#### Draw Path


Haha it make me mind about the Math I learn in high school ```>__<```
