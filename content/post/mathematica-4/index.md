---
title: Mathematica tips for numerical linear algebra (4)
subtitle: ""
date: 2021-06-15T00:00:00.000Z
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
## Transpose and Flatten

So now we know how to multiply matrices using Mathematica’s `Dot` function. This also works for higher-rank tensors (more indices) such as $m_{abc} n_{cde} = (mn)_{abde}$, which we can implement as

```mathematica
m = RandomComplex[{}, {10, 20, 30}];
n = RandomComplex[{}, {30, 40, 50}];

m // Dimensions
(* {10,20,30} *)
n // Dimensions
(* {30,40,50} *)

m.n // Dimensions
(* {10,20,40,50} *)
```

Here we used `Dimensions` to see that check that $m_{abc}$ is indeed a rank-three tensor of size `{10,20,30}`, and that `Dot` contracts the adjacent indices in `m.n`, leaving the remaining indices in the same order.

What if the indices we want to contract aren’t adjacent? The trick is to shuffle the indices of our tensors using either `Transpose` or `Flatten`.

## Transpose

Let’s start with `Transpose`. Given a tensor $m$, the two simplest uses of this function are `Transpose[m]` and `Transpose[m, k<->l]`. The first of these transposes the first two indices of $m$:

```mathematica
m = RandomComplex[{}, {10, 20, 30}];

m // Dimensions
(* {10,20,30} *)
Transpose[m] // Dimensions
(* {20,10,30} *)
```

The second form of the function allows us to specify which indices are transposed. For example, taking `k=1` and `l=3` will transpose the 1st and 3rd indices of $m$:

```mathematica
m = RandomComplex[{}, {10, 20, 30}];

m // Dimensions
(* {10,20,30} *)
Transpose[m, 1 <-> 3] // Dimensions
(* {30,20,10} *)
```

Using this second form, we can contract tensors such as $m_{abc} p_{dec}$:

```mathematica
m = RandomComplex[{}, {10, 20, 30}];
p = RandomComplex[{}, {40, 50, 30}];

m.Transpose[p, 1 <-> 3] // Dimensions
(* {10,20,50,40} *)
```

What if we want the indices of the resulting tensor to be ordered as `{10,20,40,50}` instead of `{10,20,50,40}`? Obviously, we could do the calculation in two steps, first constructing the product as above, and then taking a further transpose on the 3rd and 4th indices. Fortunately, `Transpose` can do this in one step. The result of `Transpose[m, {k1, k2, k3}]` is a tensor such that the 1st index of $m$ becomes the `k1`-th index, the 2nd index of $m$ becomes the `k2`-th index, and so on. For example, swapping the first two indices of $m$ is implemented as

```mathematica
m = RandomComplex[{}, {10, 20, 30}];

Transpose[m, {2, 1, 3}] // Dimensions
(* {20,10,30} *)
```

which we interpret as sending the 1st index to position 2, the 2nd index to position 1, and leaving the 3rd index unchanged.

We can then perform the contraction of $m$ and $p$ as above but with the 3rd and 4th indices swapped as

```mathematica
m = RandomComplex[{}, {10, 20, 30}];
p = RandomComplex[{}, {40, 50, 30}];

m.Transpose[p, {3, 2, 1}] // Dimensions
(* {10,20,50,40} *)
```
