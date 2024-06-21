---
title: Gilbert–Johnson–Keerthi Algorithm
author: AsyncOperator
date: 2024-06-21 17:45:00 +0300
categories: [game development, game programming]
tags: [collision testing]
---


GJK algorithm is a method used in computational geometry to determine if two convex shapes intersect.
You can check the following sources to get information about the subject.

- [Source 1](https://computerwebsite.net/writing/gjk?)
- [Source 2](https://cse442-17f.github.io/Gilbert-Johnson-Keerthi-Distance-Algorithm/)
- [Source 3](https://www.youtube.com/watch?v=ajv46BSqcK4)

---

![Support function](/assets/img/posts/support-function.gif)
_Getting all the support point for each shape in every possible direction_

![Minkowski sum and difference](/assets/img/posts/minkowski-sum-and-difference.png)
_Minkowski sum (green colored shape) and difference (red colored shape) for circle and square shapes_

![GJK algorithm result visualization](/assets/img/posts/gjk-algorithm-result-visualization.png)
_Minkowski difference shape highlighted with red color, and the triangle simplex that contains origin point_
