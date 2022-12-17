---
layout: post
read_time: true
show_date: true
title:  Robot Motion
date:   2022-12-16 11:48:22 -0600
description: Algorithms for Robot Motion
img: posts/cover/CG-Robot-Motion.png
tags: [Computational Geometry]
author: Thomas Men
toc: yes
mathjax: yes
---

Given a robot and set of obstacles, find the shortest path from $s$ to $t$ without collision.

<img src="./assets/img/posts/typora-user-images/image-20210318143348856.png" alt="image-20210318143348856" style="zoom:50%;" />

## Robot without collision volume

Starting from a point $s$ and finds a possible way to the end point $t$.

First, build a rectangle boundary and do the planar subdivision, pick the mid point for vertical planar edge and pick the mid point for ever face.  

Find the shortest path from the face $f_s$ containing $s$ to that  $f_t$ containing $t$. Add an edge from $s$ to the nearest face middle point and add an edge from that to $t$. 

<img src="./assets/img/posts/typora-user-images/image-20210318144526854.png" alt="image-20210318144526854" style="zoom:50%;" />

Analysis:

Creating trapezoidal map: $O(n\log n)$ time.

Locating trapezoids containing $s$ and $t$: $O(\log n)$ time.

Computing path using BFS: $O(V+E)=O(n)$ time.

## Robot with polygon shape

Considering the robot has certain polygon shape in vertical direction, which means the robot polygon are not allowed to rotate in inclined angle, below repeated polygon is the robot:

<img src="./assets/img/posts/typora-user-images/image-20210318144840952.png" alt="image-20210318144840952" style="zoom:67%;" />

The dark grey area is the open obstacle (boundary can be touched) and the light grey area is the collision area for the robot gravity, which means, if the robot gravity is located inside the light grey area, then the robot would collide the obstacle. The white area is the free space.

**How we represent such structure?**

Define:

- $\mathcal{R}:$ polygon robot. Reference point is $(0,0)$.
- $\mathcal R(x,y):=\mathcal R+(x,y)$, translation by vector $(x,y)$.
- $\mathcal P$: polygon obstacle.
- $\mathcal{CP}=\mathcal P\oplus(-\mathcal R)$. Here $A\oplus =\\{a+b\mid a\in A,b\in B\\}$ is the *Minkowsky sum*. 

**How do we compute the free space? How complicated can the space be? i.e., how many corners can it have?** 

### Naïve algorithm

![image-20210318162907416](./assets/img/posts/typora-user-images/image-20210318162907416.png)

Each edge of $A\oplus B$ stems from an edge of $A$ or $B$ (or both). Therefore, $A\oplus B$ has $\leq n+m$ edges, where $n$ edges of $A$ and $m$ edges of $B$. 

Compute $C_A\oplus C_B$ and take the convex hull, $C_A$ and $C_B$ are the corners of $A$ and $B$. 

Time: $O(mn\log (mn))$.  

For every corners $b\in B$, it will match all corners $a\in A$, thus the total number of nodes on boundary of the polygon equals to $mn$ and determining a convex hull for a polygon is $CH(P)$. 

### Smarter algorithm

<img src="./assets/img/posts/typora-user-images/image-20210318164524585.png" alt="image-20210318164524585" style="zoom:50%;" /> <img src="./assets/img/posts/typora-user-images/image-20210318164859731.png" alt="image-20210318164859731" style="zoom:45%;" />

Pick two extreme points on the polygon along the arrow direction. Move the edge $e_A$ parallel to make it ends in $B$. Then rotate the arrow and find next perpendicular edge in two polygons and remove the corresponding edge next to the boundary of forbidden area.

Time: $O(m+n)$.

`Theorem` $A$ and $B$ polygons with $m$ and $n$ corners,

- $A$ and $B$ are convex: $A\oplus B$ has $O(m+n)$ corners,
- only one is convex: $A\oplus B$ has $O(mn)$ corners,
- none is convex: $A\oplus B$ has $O(m^2n^2)$ corners.

These bounds are tight.

---

`Defintiion`  Polygon $o_1$ and $o_2$ are pseudodisks iff $\partial o_1\cap int (o_2)$ and $int(o_1)\cap \partial o_2$ are connected, ($\partial o_1$ is the boundary of $o_1$ and $int(o_2)$ represents the intersection area of $o_1$ with $o_2$). 

If two polygons have no intersection, they are pseudodisks as well, because the union is empty represents there is no element violates the rule.

<img src="./assets/img/posts/typora-user-images/image-20210318170340179.png" alt="image-20210318170340179" style="zoom:67%;" /> 

`Theorem` A set of pairwise pseudodisks with $n$ corners in total $\Longrightarrow$ union has $2n$ corners.

`Proof` 

Pay 1 means 1 corner has been introduced.

<img src="./assets/img/posts/typora-user-images/image-20210318170558274.png" alt="image-20210318170558274" style="zoom:60%;" />

`Theorem` $\mathcal R$ robot with $O(1)$ corners, obstacles with $n$ corners $\Longrightarrow$ free space has $O(n)$ corners. 

`Proof` Since there are $O(n-2)=O(n)$ triangles for a polygon with $n$ corners, then for each triangle $T_i$, the intersection $T_i\oplus(-\mathcal R)$ has complexity $O(3+O(1))=O(1)$ corners. 

Since $T_i$ and $T_j$ are interior-disjoint triangles, then $T_1\oplus (-\mathcal R)$ and $T_2\oplus(-\mathcal R)$ are pseudodisks. 

Finally, $\bigcup_T(T\oplus(-\mathcal R))$ has $O(n)$ corners.

### Algorithm

$k$ obstacles with $n_1,\cdots,n_k$ corners, in total $n$.

Triangulating: 
<center>$$
\sum_{i=1}^kO(n_i\log n_i)=O(\sum_{i=1}^kn_i\log n_i)\leq O(\log n\sum_{i=1}^kn_i)=O(n \log n)
$$</center>
Computing Minkowsky sum between each obstacles and robot, $\mathcal{CP}_j=\mathcal P_j\oplus(-\mathcal R):O(1)$ time (three corners per $\triangle$ and one corner per robot, constant number in total), in total $O(n)$. 

Below algorithm computing the forbidden space:

<img src="./assets/img/posts/typora-user-images/image-20210318172942912.png" alt="image-20210318172942912" style="zoom:67%;" />

Compute the union of two polygons in step 5 takes $O(n\log n)$ time using overlay algorithm from chap. 2.

The total Running time: $T(n)=2T(n/2)+O(n\log n)$, solves to $T(n)=O(n\log^2n)$.

## Shortest path

<img src="./assets/img/posts/typora-user-images/image-20210318174052202.png" alt="image-20210318174052202" style="zoom:67%;" />

The complexity of computing visibility graph (for every node $i$, rotate a ray starting from $i$ and catch the intersections):

- Initialization: sort corners by angle, $O(n\log n)$ time.
- Compute intersections with horizontal sweep line and sort them by $x$-coordinate, $O(n\log n)$ time.
- Perform the rotational sweep, $O(n\log n)$ time.
- Do it for all $n$ corners, $O(n^2\log n)$

Complete algorithm for the shortest path:

- Compute visibility graph: $O(n^2\log n)$ time. $O(n)$ vertices and $O(n^2)$ edges.
- Find shortest path using Dijkstra: $O(n\log n+n^2)=O(n^2)$.
- In total $O(n^2\log n)$.