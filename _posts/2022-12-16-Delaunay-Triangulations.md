---
layout: post
read_time: true
show_date: true
title:  Delaunay Triangulations
date:   2022-12-16 11:48:21 -0600
description: Algorithms for Delaunay Triangulations
img: posts/cover/CG-Delaunay.png
tags: [Computational Geometry]
author: Thomas Men
toc: yes
mathjax: yes
---

A terrain is a 2-dimensional surface in 3-dimensional space with a special property: every vertical line intersects it in a point, if it intersects it at all. In other words, it is the graph of a function $f:A\subset \mathbb{R}^2\rightarrow\mathbb{R}$ that assigns a height $f(p)$ to every point $p$ in the domain, $A$, of the terrain.

<img src="./assets/img/posts/typora-user-images/image-20210317132256993.png" alt="image-20210317132256993" style="zoom:67%;" />

Nevertheless, some triangulations look more natural than others. We will rank triangulations by comparing their smallest angle. Since there is only finite number of different triangulations of a given point set $P$, this implies that there must be an optimal triangulation, **one that maximizes the minimum angle**.

## Triangulations of Planar Point Set

Let $P:=\{p_1,p_2,\cdots,p_n\}$ be a set of points in the plane. Define a *maximal planar subdivision* as a subdivision $\mathfrak S$ such that no edge connecting two vertices can be added to $\mathfrak S$ without destroying its planarity. In other words, any edge that is not in $S$ intersects one of the existing edges. 

A *triangulation* of $P$ is now defined as a maximal planar subdivision whose vertex set is $P$. The number of triangles is the same in any triangulation of $P$, this holds for the number of edges.

`Theorem 9.1` Let $P$ be a set of $n$ points in the plane, not all colinear, and let $k$ denote the number of points in $P$ that lie on the boundary of the convex hull of $P$. Then any triangulation of $P$ has $2n-2-k$ triangles and $3n-3-k$ edges.

Let $\mathfrak T$ be a triangulation of $P$, and suppose it has $m$ triangles. Let $\alpha_1,\alpha_2,\cdots,\alpha_{3m}$ be the resulting sequence of angles, where $\alpha_i\le\alpha_j$, for $i<j$. We can $A(\mathfrak T):=(\alpha_1,\alpha_2,\cdots,\alpha_{3m})$ the *angle-vector* of $\mathfrak T$. We say that the angle vector of $\mathfrak T$ is larger than the angle-vector of $\mathfrak T'$, denoted as $A(\mathfrak T)>A(\mathfrak T')$, if $A(\mathfrak T)$ is lexicographically larger than $A(\mathfrak T')$, or in other words, if there exists an index $i$ with $1\leq i\leq 3m$ such that:
$$
\alpha_j=\alpha_j', \forall j<i\quad and \quad \alpha_i>\alpha_i'
$$
A triangulation $\mathfrak T$ is called *angle-optimal* if $A(\mathfrak T)\geq A(\mathfrak T')$ for all triangulations $\mathfrak T'$ of $P$.

### When a triangulation is angle-optimal?

Below is a basic operation called *edge flip*:

<img src="./assets/img/posts/typora-user-images/image-20210317141229786.png" alt="image-20210317141229786" style="zoom:67%;" />

We call the edge $e=\overline{p_ip_j}$ an *illegal edge* if
$$
\min_{q\leq i\leq 6}\alpha_i<\min_{1\leq i \leq 6}\alpha_i'
$$
In other words, an edge is illegal if we can locally increase the smallest angle by flipping that edge.

`Obervation 9.3` Let $\mathfrak T$ be a triangulation with an illegal edge $e$. Let $\mathfrak T'$ be the triangulation obtained from $\mathfrak T$ by flipping $e$. Then $A(\mathfrak T')>A(\mathfrak T)$.

So how we check whether there is a illegal edge? It is not necessary to compute and compare the angles in value, below Lemma states a better approach:

`Lemma 9.4` Let edge $\overline{p_ip_j}$ be incident to triangles $p_ip_jp_k$ and $p_ip_jp_l$, and let $C$ be the circle through $p_i,p_j$ and $p_k$. The edge $\overline{p_ip_j}$ is illegal if and only if the point $p_l$ lies in the interior of $C$. Furthermore, if the points $p_i,p_j,p_k,p_l$ form a convex quadrilateral and do not lie on a common circle, then exactly one of $\overline{p_ip_j}$ and $\overline{p_kp_l}$ is an illegal edge.

Prove by applying the Thales's Theorem.

<img src="./assets/img/posts/typora-user-images/image-20210317142216546.png" alt="image-20210317142216546" style="zoom:67%;" />

Define a *legal triangulation* to be the triangulation that does not contain any illegal edge. Computing a legal edge is quite simple, once we are given an initial triangulation. We simply flip illegal edges until all edges are legal. Why does this algorithm terminate? It follows from Observation 9.3 that the angle-vector of $\mathfrak T$ increases in every iteration of the loop. Since there is only a finite number of different triangulations of $P$, then this algorithm terminates.

## The Delaunay Triangulation

Let $P$ be a set of $n$ points -- or *sites*. The Voronoi diagram of $P$ is denoted by $Vor(P)$. The region of a sit $p$ is called the Voronoi cell of $p$; it is denoted by $V(p)$. The dual graph of $Vor(P)$, $\mathfrak G$, has a node for every Voronoi cell and it has an arc between two nodes if the corresponding cells share an edge. The straight-line embedding of $\mathfrak G$, is called the *Delaunay graph* of $P$, and we denote it by $DG(P)$.

<img src="./assets/img/posts/typora-user-images/image-20210317143448101.png" alt="image-20210317143448101" style="zoom:50%;" />   <img src="./assets/img/posts/typora-user-images/image-20210317143506814.png" alt="image-20210317143506814" style="zoom:50%;" />

`Theorem 9.5` The Delaunay graph of a planar point set is a plane graph.

If $P$ in general position, then all vertices of the Voronoi diagram have degree three, and consequently all bounded faces of $DG(P)$ are triangles. This explains why $DG(P)$ is often called the *Delaunay triangulation* of $P$. Here we carefully call $DG(P)$ as *Delaunay graph* and we define a Delaunay triangulation to be any triangulation obtained by adding edges to the Delaunay graph.

`Theorem 9.6` Let $P$ be a set of points in the plane.

1. Three points $p_i,p_j,p_k\in P$ are vertices of the same face of the Delaunay graph of $P$ if and only if the circle through $p_i,p_j,p_k$ contains no point of $P$ in its interior.
2. Two points $p_i,p_j\in P$ form an edge of the Delaunay graph $P$ if and only if there is a closed disc $C$ that contains $p_i$ and $p_j$ on its boundary and does not contain any other point of $P$.

`Theorem 9.7` Let $P$ be a set of points in the plane, and let $\mathfrak T$ be a triangulation of $P$. Then $\mathfrak T$ is a Delaunay triangulation of $P$ if and only if the circumcircle of any triangle of $\mathfrak T$ does not contain a point $P$ in its interior.

`Theorem 9.8` Let $P$ be a set of points in the plane. A triangulation $\mathfrak T$ of $P$ is legal if and only if $\mathfrak T$ is a Delaunay triangulation of $P$.

`Proof` Prove by contradiction. 

<img src="./assets/img/posts/typora-user-images/image-20210317155546138.png" alt="image-20210317155546138" style="zoom:67%;" />

 Assume $\mathfrak T$ is a legal triangulation of $P$ that is not a Delaunay triangulation, this means $p_l$ lines in $C(p_ip_jp_k)$, pick $p_l$ to make $\angle p_ip_lp_j$ maximum, such an angle in contained in a $\triangle p_ip_jp_l$ which is not intersect with $\triangle p_ip_jp_k$. $p_m$ is the adjacent triangle to $\triangle p_ip_jp_k$ and thus $p_m\notin C(p_ip_jp_k)$, Now, assume $e=\overline{p_mp_j}$, however $\angle p_mp_lp_j>\angle p_ip_lp_j$, contradict to the assumption that $\angle p_ip_lp_j$ is maximum. Finish.

Since any angle-optimal triangulation must be legal, Theorem 9.8 implies that any angle-optimal triangulation of $P$ is a Delaunay triangulation of $P$. When $P$ is in general position, there is only one legal triangulation, which is then the only angle-optimal triangulation, namely the unique Delaunay triangulation that coincides with the Delaunay graph.

`Theorem 9.9` Let $P$ a set of points in the plane. Any angle-optimal triangulation of $P$ is a Delaunay triangulation of $P$. Furthermore, any Delaunay triangulation of $P$ maximizes the minimum angle over all triangulations of $P$.

## Computing Delaunay Triangulation

 We now start with a large triangle that contains the set $P$. We add two extra points $p_{-1}$ and $p_{-2}$, which locates far enough away. Now our task is to triangulate the point set $P\cup\{p_{-1},p_{-2}\}$. After the triangulation, we shall just remove them. 

<img src="./assets/img/posts/typora-user-images/image-20210317161626392.png" alt="image-20210317161626392" style="zoom:67%;" />

The algorithm is randomized incremental, so it adds the points in random order and it maintains a Delaunay triangulation of the current point set. 

Consider the addition of a point $p_r$. We first find the triangle of the current triangulation that contains $p_r$ and we add edges from $p_r$ to the vertices of this triangle.

<img src="./assets/img/posts/typora-user-images/image-20210317164347233.png" alt="image-20210317164347233" style="zoom:67%;" />

We now have a triangulation again, but not necessarily a Delaunay triangulation. This is because the addition of $p_r$ can make some of the existing edges illegal. Below is algorithm:

<img src="./assets/img/posts/typora-user-images/image-20210317164746898.png" alt="image-20210317164746898" style="zoom:67%;" />

### Turing Triangulation into Delaunay forms

Such goal is finished by flipping illegal edges. The question remains is which edges may become illegal due to the insertion of $p_r$. Observe that an edge $\overline{p_ip_j}$ that was legal before can only become illegal if one of the triangles incident to it has changed. So only the edges of the new triangles need to be checked. 

This is done using the subroutine $LEGALIZEDGE$, which tests and possibly flips an edge. If it flips an edge, other edges may become illegal, therefore the function calls itself recursively with such potentially illegal edges.

![image-20210317165905111](./assets/img/posts/typora-user-images/image-20210317165905111.png)

`Lemma 9.10` Every new edge created in $\text{DELAUNAYTRIANGULATION}$ or in $\text{LEGALIZEEDGE}$ during the insertion of $p_r$ is an edge of the Delaunay graph of $\{p_{-2},p_{-1},\cdots,p_r\}$.

The correctness of algorithm has been proved.

---

Remaining two problems:

1. How to find the triangle containing the point $p_r$ in line $7$ of $\text{DELAUNAYTRIANGULATION}$ and,
2. how to deal correctly with the points $p_{-1}$ and $p_{-2}$ in the test in line 2 in $\text{LEGALIZEEDGE}$.

### Find triangle containing $p_r$

While we build the Delaunay triangulation, we also build a point location structure $D$, which is a directed acyclic graph. The leaves of $D$ correspond to the triangles of the current triangulation $\mathfrak T$, and we maintain cross-pointers between those leaves and the triangulation. The internal nodes of $D$ correspond to triangles that were in the triangulation at some earlier stage, but have already been destroyed.

At some point we split a triangle $p_ip_jp_k$ of the current triangulation into three (or two) new triangles. The corresponding change in $D$ is to add three (or two) new leaves to $D$, and to make the leaf for $p_ip_jp_k$ into an internal node with outgoing pointers to those three (or two leaves).

<img src="./assets/img/posts/typora-user-images/image-20210317172902459.png" alt="image-20210317172902459" style="zoom:80%;" />

To use $D$ to locate the next point $p_r$ to be added in the current triangulation. We start at the root of $D$, which corresponds to the initial triangle $p_0p_{-1}p_{-2}$. We check the three children of the root to see in which triangle $p_r$ lies, and we descend to the corresponding child. We then check the children of this node, descend again, until we reach a leaf of $D$. Since the out-degree of any node is at most three, this takes linear time in the number of nodes on the search path, or other words, in the number of triangles stored in $D$ that contain $p_r$.

### Choose $p_{-1}$ and $p_{-2}$ 

What we do is to treat these points *symbolically*: we do not actually assign coordinates to them, but instead modify the tests for point location and for illegal edges such that they work *as if* we had chosen the points to be very far away.

## The Analysis

`Lemma 9.11` The expected number of triangles created by algorithm $\text{DELAUNAYTRIANGULATION}$ is at most $9n+1$.

`Theorem 9.12` The Delaunay triangulation of a set $P$ of $n$ points in the plane can be computed in $O(n\log n)$ expected time, using $O(n)$ expected storage.