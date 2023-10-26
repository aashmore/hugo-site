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
## Dot products

The bulk of the numerical calculations that I need are basically linear algebra -- matrix-matrix, vector-matrix and more exotic multiplications. All of the entries in these tensors are “machine precision”, which roughly translates to a C++ double. Mathematica can store these numbers as “packed arrays” -- working with machine precision numbers rather than “exact” quantities greatly speeds up calculations (if they are written in the right way).

Let’s start with a simple example where we want to take the dot product of two vectors $u$ and $v$. In index notation this is simply $u \cdot v =u_a v_a$, so we can think of the dot product as simply multiplying and then summing the individual entries of the vectors. Mathematica has many ways to carry out such a simple calculation, but picking the right way now will make sure more complicated calculations don’t take all weekend!

```mathematica
l = 10; (* size of vectors *)
u = RandomComplex[{}, {l}];
v = RandomComplex[{}, {l}];

Sum[u[[i]] v[[i]], {i, 1, l}]
Total[u*v]
u.v
```

Here we’ve defined our vectors to be of length $l$. `RandomComplex` then gives a list whose entries are machine-precision complex numbers. We have given three ways to calculate the dot product.

- The first is closest to what one might do in C++ where we simply loop over the entries and sum them together. Though this might be the most obvious thing to do, this is nearly always the slowest (as we’ll see below).

- The next way uses the Mathematica operator `*` which in this instance takes in two vectors and outputs a third vector whose entries are given by $(u*v)_a = u_a v_a$  with no sum over $a$. We can then sum the entries of this new vector using `Total` to get the dot product.

- Our third way is simply using the built-in Mathematica function `Dot[u,v]`, which can also be called as `u.v`.

For relatively small vectors, any of these will do. However, when one is dealing with numerical calculations with millions of points, there are big differences between them. For example, let’s increase the length of the vectors to ten million and check how long each method takes.

```mathematica
l = 10^7; (* size of vectors *)
u = RandomComplex[{}, {l}];
v = RandomComplex[{}, {l}];

Sum[u[[i]] v[[i]], {i, 1, l}] // AbsoluteTiming
(* 29.6766 s *)
Total[u*v] // AbsoluteTiming
(* 0.155223 s *)
u.v // AbsoluteTiming
(* 0.036661 s *)
```

Uh oh! Explicitly summing over the elements of the vectors as one might do in C++ is crazy slow, taking over 800 times longer than using `Dot`. Notice that using `Total` is slower than `Dot` -- you might have expected this since `u*v` constructs a new vector which `Total` then sums, whereas `Dot` just computes the sum directly. So what’s up?
