---
title: Graham Scan
author: AsyncOperator
date: 2024-08-25 20:20:00 +0300
categories: [game development, game programming]
tags: [algorithm]
---


The Graham scan algorithm compute the convex hull of a set of points in a plane.
The convex hull is the smallest convex polygon that can enclose all the points in the set, much like stretching a rubber band around the points and letting it snap into place.

- [Source 1](https://www.algorithm-archive.org/contents/graham_scan/graham_scan.html)
- [Source 2](https://www.youtube.com/watch?v=9rQMLpQn5xQ)

## How the algorithm works?

### Find the point with the lowest y-coordinate

First, the algorithm identifies the point with the lowest y-coordinate.
This point is guaranteed to be part of the convex hull.

```csharp
   Array.Sort(m_Points, delegate (Vector2 v1, Vector2 v2) { return v1.y.CompareTo(v2.y); });
```

### Sort the other points by polar angle

The other points are then sorted based on the polar angle they make with the lowest y-coordinate point.

```csharp
   Vector2 lowestYCoordinatePoint = m_Points[0];
   Array.Sort(m_Points, 1, m_Points.Length - 1, new Vector2AngleComparer(lowestYCoordinatePoint));
```

### Build the convex hull

The algorithm then iterates through the sorted points, and for each point, it determines whether moving from the last two points in the current convex hull to this point constitutes a "left turn" or a "right turn."

```csharp
   var convexHull = new Stack<Vector2>();
   
   convexHull.Push(m_Points[0]);
   convexHull.Push(m_Points[1]);
   
   for (int i = 2; i < m_Points.Length; i++)
   {
      var candidatePoint = m_Points[i];
   
      while (convexHull.Count >= 2 && !IsCCWTurn(PeekAtStackSecondTop(convexHull), convexHull.Peek(), candidatePoint))
      {
         convexHull.Pop();
      }
   
      convexHull.Push(candidatePoint);
   }
   
   convexHull.Push(m_Points[0]);
```

---

![Graham scan algorithm demonstration](/assets/img/posts/graham-scan.gif)
_Graham scan algoritm implemented inside Unity Engine_
