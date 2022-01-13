---
title: Mathematica tips for numerical linear algebra (2)
subtitle: ""
date: 2021-02-27T00:00:00.000Z
summary: ""
draft: false
featured: false
authors:
  - admin
lastmod: null
tags: []
categories:
  - Mathematica
projects: []
image:
  caption: ""
  focal_point: ""
  placement: 2
  preview_only: false
---
## Overview

The bulk of the numerical calculations that I need are basically linear algebra -- matrix-matrix, vector-matrix and more exotic multiplications. All of the entries in these tensors are “machine precision”, which roughly translates to a C++ double. Mathematica can store these numbers as “packed arrays” -- working with machine precision numbers rather than “exact” quantities greatly speeds up calculations (if they are written in the right way).

Let’s start with a simple example where we want to take the dot product of two vectors $u$ and $v$. In index notation this is simply $u \cdot v =u_a v_a$, so we can think of the dot product as simply multiplying and then summing the individual entries of the vectors. Mathematica has many ways to carry out such a simple calculation, but picking the right way now will make sure more complicated calculations don’t take all weekend!

```Mathematica
l = 10; (* size of vectors *)
u = RandomComplex[{}, {l}];
v = RandomComplex[{}, {l}];

Sum[u[[i]] v[[i]], {i, 1, l}]
Total[u*v] 
u.v
```