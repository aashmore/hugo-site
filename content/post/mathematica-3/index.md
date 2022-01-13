---
title: Mathematica tips for numerical linear algebra (3)
subtitle: ""
date: 2021-04-01T00:00:00.000Z
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
## Mathematica is smart

The take-away from this is that many clever people have worked for a very long time on optimising the Mathematica’s built-in functions, such as `Dot`. In particular, when `Dot` is called, it turns the calculation over to a set of C++ libraries for numerical linear algebra known as the Intel MKL. Of course, we could just do all of this in C++ and call MKL directly. Usually, however, the time saved in the calculation itself is not worth the extra headaches of coding in C++ and having to recompile just to try out a new idea. Mathematica allows us to play with ideas and have performance close to (within a factor of three or so) doing everything in C++. And as we’ll see in a future post, Mathematica can take advantage of multiple CPU cores without us really having to think about it.

We can see the same behaviour play out with higher-rank tensors. Imagine we want to contract a matrix with another matrix: $m_{ab}n_{bc} = (mn)_ac$. We could implement this as

```mathematica
m = RandomComplex[{}, {500, 1000}];
n = RandomComplex[{}, {1000, 750}];

m.n; // AbsoluteTiming
(* 0.0735985 s *)
```

Not bad! Just let Mathematica do the dirty work! Note that Mathematica thinks of these matrices as arrays (or more accurately `Lists`) of dimensions `{500,1000}` and `{1000,750}` respectively:

```mathematica
m // Dimensions
(* {500,1000} *)
n // Dimensions
(* {1000,750} *)
m.n // Dimensions
(* {500,750} *)
```

This also gives us an idea of what Mathematica is doing with `Dot` -- the length of the last dimension of $m$  must match the length of the first dimension of $n$ (since it is contracting these indices). The resulting array is labelled by the remaining indices of $m$ and $n$ in the order they appear in according to $m_{ab} n_{bc} = (m n)_{ac}$.
