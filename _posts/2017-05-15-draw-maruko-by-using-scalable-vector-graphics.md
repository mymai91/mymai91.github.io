---
layout: post
title:  "[WEB DESIGN] Draw Maruko by using scale vector graphic (SVG)"
comments: true
date:   2017-05-25 15:07:33 +0700
categories: web_design
---


I drawn Maruko only using SVG object. She appear so cute. Her face like my partner in current company.

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:space="preserve" viewBox="0 0 500 300">
  <path id="hair" d="M53.8 177.3, C57 150, 51.1 119.5, 76 66, 101.6 37.3, 160.5 16, 212.2 42.3,
            246 65.3, 265 106.8, 275 149.8, L275 149.8, 53.8 177.3" stroke="#231815" fill="#132335"></path>

  <path id="face" d="M72 126.8, 72 110.9, L82.9 123.4, 86 97, 99 116.2, 108.3 76.7, 127.2 97.6,
  138.3 63.2, 159 95, 175.6 60.5, 190.4 91.5, 205 66.1, 216.3 98.4, 229 81.2, 238.4 109.5, 246.3 99.3, 250 105.7,
  M250 105.7, 246 123.8, C242 151, 238.8 174.1, 210 187.7, 184 195.5, 154 212.5, 100.9 178,
  83.1 165, 78.3 142.6, 71 122, " stroke="#231815" fill="#fad9c7"></path>

  <g id="eyebrow">
    <path d="M101 113, C123 101, 134 92, 144.4 80.8" stroke="#231815" fill="#231815"/>
    <path d="M177 79, C186 87.7, 200.6 95, 222 98" stroke="#231815" fill="#231815"/>
  </g>

  <g id="eyes">
    <circle cx="126.4" cy="125" r="9" fill="#231815"/>
    <circle cx="183.8" cy="117.7" r="9" fill="#231815"/>
  </g>

  <path id="mouth" stroke="#231815" fill="#cb6866"
    d="M150 170, C150 161, 172 159, 179 163, 183.9 165.4, 182 170, 182 171, 177 183, 156 181, 150 170"
  ></path>
</svg>

```
<svg viewBox="0 0 500 300">
  <path id="hair" d="M53.8 177.3, C57 150, 51.1 119.5, 76 66, 101.6 37.3, 160.5 16, 212.2 42.3,
            246 65.3, 265 106.8, 275 149.8, L275 149.8, 53.8 177.3" stroke="#231815" fill="#132335"></path>

  <path id="face" d="M72 126.8, 72 110.9, L82.9 123.4, 86 97, 99 116.2, 108.3 76.7, 127.2 97.6,
  138.3 63.2, 159 95, 175.6 60.5, 190.4 91.5, 205 66.1, 216.3 98.4, 229 81.2, 238.4 109.5, 246.3 99.3, 250 105.7,
  M250 105.7, 246 123.8, C242 151, 238.8 174.1, 210 187.7, 184 195.5, 154 212.5, 100.9 178,
  83.1 165, 78.3 142.6, 71 122, " stroke="#231815" fill="#fad9c7"></path>

  <g id="eyebrow">
    <path d="M101 113, C123 101, 134 92, 144.4 80.8" stroke="#231815" fill="#231815"/>
    <path d="M177 79, C186 87.7, 200.6 95, 222 98" stroke="#231815" fill="#231815"/>
  </g>

  <g id="eyes">
    <circle cx="126.4" cy="125" r="9" fill="#231815"/>
    <circle cx="183.8" cy="117.7" r="9" fill="#231815"/>
  </g>

  <path id="mouth" stroke="#231815" fill="#cb6866"
    d="M150 170, C150 161, 172 159, 179 163, 183.9 165.4, 182 170, 182 171, 177 183, 156 181, 150 170"
  ></path>
</svg>
```

This is the image I capture from my phone:

<div style="text-align: center">
  <img src="https://cloud.githubusercontent.com/assets/6791942/26454488/743f564e-4191-11e7-985b-b5b6f46b9cf4.jpg" alt="maruko">
</div>
