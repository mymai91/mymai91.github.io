---
layout: post
title:  "[WEB DESIGN] Getting Started With Scalable Vector Graphics (SVG)"
comments: true
date:   2017-05-15 15:07:33 +0700
categories: web_design
---

Hi guys! <big><b> >__< </b></big>

After longtime I back with my blog. Today let start with SVG

### What is SVG?

SVG stand for Scalable Vector Graphics it is used to define graphic for web site.
Let me show for you some example. I definitely that you will like it and it easy to learn. Let's go yeah!!!
And we will use `<svg>` tag to start draw

#### 1) Draw Line

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

<br/>

#### 2) Draw Rectangles

<br/>


To draw the Rectangles we need to use the `<rect>` tag

**Code**

```
<svg>
  <rect width="200" height="100" style="fill: #e7e7e7"/>
</svg>
```

**Result**
<div style="text-align: center">
  <svg>
    <rect width="200" height="100" style="fill: #000"/>
  </svg>
</div>

<p><i>The result similar way draw rectangles by CSS</i></p>

<br/>

#### 3) Draw Polygons

<br/>


To draw the Polygons we need to use the `<polygon>` tag.

I will make 2 example draw polygon are draw triangle or rectangle

#### Ex 1: Draw triangle

**Code**

```
<svg>
  <polygon points="10 20, 300 200, 200 20" style="stroke:#000; stroke-width:5"/>
</svg>
```

**Result**
<div style="text-align: center">
  <svg style="width: 500; height: 300; background: #e7e7e7;">
    <polygon points="20 20, 300 200, 200 20" style="stroke:#000; stroke-width:5"/>
  </svg>

</div>

**points What are those?**

Let take a look this photo I draw.

`points="X1 Y1, X2 Y2, X3 Y3"`

With (X1, Y1) => We will get the point **A**

With (X2, Y2) => We will get the point **B**

With (X3, Y3) => We will get the point **C**

**Connect 3 points A,B and C** We will get the triangle **ABC**


<div style="text-align: center">
  <img src="https://cloud.githubusercontent.com/assets/6791942/26058326/a1c8f49a-39a7-11e7-80b2-5979569dbba5.jpg" alt="toado">
</div>

<br/>

#### Draw Path

<br/>


To draw the Path we need to use the `<path>` tag.

**Code**

```
<svg width=400 height=400>
  <path d="M100 50, C150 100, 200 120, 300 50, H300 50, 100 50" stroke="red" fill="none"/>
</svg>
```

**Result**


<div style="text-align: center">
  <svg style="width: 400; height: 200; background: #e7e7e7;">
    <path d="M100 50, C150 100, 200 120, 300 50, H300 50, 100 50" stroke="red" fill="none"/>
  </svg>

</div>

If you want to have Sweet lips let make the fill with red color hahaaaa ^___^

**Code**

```
<svg width=400 height=400>
  <path d="M100 50, C150 100, 200 120, 300 50, H300 50, 100 50" stroke="red" fill="red"/>
</svg>
```

**Result**


<div style="text-align: center">
  <svg style="width: 400; height: 200; background: #e7e7e7;">
    <path d="M100 50, C150 100, 200 120, 300 50, H300 50, 100 50" stroke="red" fill="red"/>
  </svg>

</div>

You see I write `d="M100 50, C150 100, 200 120, 300 50, H300 50, 100 50"`

**What is it ?**

M: move to

L: line to

H: horizontal line to

V: vertical line to

C: curve to

S: smooth curve to

Q: quadratic bezier curve

T: smooth quadratic bezier curve to

A: elliptical Arc

Z: close path



>
Haha it make me mind about the Math I learn in high school ```>__<```
