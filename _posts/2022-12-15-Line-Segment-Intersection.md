---
layout: post
read_time: true
show_date: true
title:  Line Segment Intersection
date:   2022-12-15 12:38:20 -0600
description: Introduction to Line Segment Intersection
img: posts/cover/CG-LineIntersection.png
tags: [Computational Geometry]
author: Thomas Men
toc: yes
mathjax: yes
---

## Line Segment intersection

`Problem` Given a set $S$ of $n$ closed segments in the plane, report all intersection points among the segments in $S$.

`Theorem 2.4` Let $S$ be a set of $n$ line segments in the plane. All intersection points in $S$, with for each intersection point the segments involved in it, can be reported in $O(n\log n+I\log n)$ time and $O(n)$ space, where $I$ is the number of intersection points.

## The double-Connected Edge List 

Some Definition:

-  A *face* of the subdivision is connected subset of the plane that doesn't contain a point on an edge or a vertex. Thus a face is an open polygon region whose boundary is formed by edges and vertices from the subdivision.

- The *complexity* of a subdivision is defined as the sum of the number of vertices, the number of edges, and the number of faces it consists of.

- If a vertex is the endpoint of an edge, then we say that the vertex and the edge are *incident*.

Our goal is to:

- require that we can walk around the boundary of a given face,
- we can access one face from an adjacent one if we are given a common edge. 
- visit all the edges around a given vertex. 

Our answer is called the `doubly-connected edge list`. A DCEL contains a record for each face, edge, and vertex of the subdivision.

The doubly-connected edge list consists of three collections of records: one for the vertices, one for the faces, and one for the half-edges. These records store the following geometric and topological information:

- The vertex record of a vertex $v$ stores the coordinates of $v$ in a field called *Coordinates(v)*. It also stores a pointer *IncidentEdge(v)* to an arbitrary half-edge that has $v$ as its origin.
- The face record of a face $f$ stores a pointer *OuterComponent(f)* to some half-edge on its outer boundary. For the unbounded face this pointer is *nil*. It also stores a list *InnerComponents(f)*, which contains for each hole in the face a pointer to some half-edge on the boundary of the hole.
- The half-edge record of a half-edge $\overrightarrow{e}$ stores a pointer *Origin($\overrightarrow{e}$)* to its origin, a pointer *Twin($\overrightarrow{e}$)* to its twin half-edge, and a pointer *IncidentFace($\overrightarrow{e}$)* to the face that it bounds. We don’t need to store the destination of an edge, because it is equal to *Origin(Twin($\overrightarrow{e}$))*.  The origin is chose such that *IncidentFace($\overrightarrow{e}$)* lies to the left of $\overrightarrow{e}$ when it is traversed from origin to destination. The half-edge record also stores pointers *Next($\overrightarrow{e}$)* and *Prev($\overrightarrow{e}$)* to the next and previous edge on the boundary of *IncidentFace($\overrightarrow{e}$)* that has the destination of $\overrightarrow{e}$ as its origin, and *Prev($\overrightarrow{e}$)* is the unique half-edge on the boundary of *IncidentFace($\overrightarrow{e}$)* that has *Origin($\overrightarrow{e}$)* as its destination.

![image-20210225221914680](./assets/img/posts/typora-user-images/image-20210225221914680.png)

![image-20210225221925751](./assets/img/posts/typora-user-images/image-20210225221925751.png)