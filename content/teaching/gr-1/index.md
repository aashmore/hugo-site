---
title: General Relativity I (Autumn 2018)
subtitle: ""
date: 2018-10-01
summary: ""
draft: false
featured: false
authors:
  - admin
lastmod: 2020-12-13T00:00:00.000Z
tags: []
categories:
projects: []
image:
  caption: ""
  focal_point: ""
  placement: 2
  preview_only: false
---
## Overview

Calabi-Yau manifolds are special as they are Kähler manifolds which admit Ricci-flat metrics. As of the end of 2020, there are still no complete analytic expressions for such metrics on compact manifolds, other than in the somewhat trivial cases of tori. Instead, the best we can do are approximate numerical metrics. I became interested in this a couple of years ago when trying to learn some of the machine-learning techniques that are all the rage at the moment. But before I could I apply all this fancy new technology, I first needed some data to work with. And so I found myself needing to understand how to generate these numerical metrics.

Previous work on these numerical metrics had used C++. Unfortunately, my experience with C++ is limited to a “Teach Yourself C++ in 24 Hours” book and some bad memories of pointers in undergraduate computing labs. Since I didn’t want to reinvent the wheel, I decided I’d try to implement the numerical metrics using Mathematica instead. I figured this would make the code much easier to handle and it would be closer to the kind of language (tensors and so on) that I would use if I were doing a calculation with pen and paper.

And so here we are almost two years later. I did manage to do what I wanted using only Mathematica. It took a few late nights of scouring the Wolfram documentation and copious amounts of help from StackExchange, but I think it really was easier than keeping track of hundreds of .cc and .h files (and trying to decipher completely cryptic compiler errors). Along the way, I learned a few neat tricks and tips for working with numerical calculations in Mathematica that I figure might be of use to someone else, and so I’m going to lay them out in the next few posts for the interested reader.
