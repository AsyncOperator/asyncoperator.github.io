---
title: Get a Random Point Inside Circle?
author: AsyncOperator
date: 2025-04-13 17:15:00 +0300
categories: [game development, game programming]
tags: [math]
---


## The problem

I've realized that all this time, I’ve been picking a random point inside a circle in a way that isn’t actually all that random — without even noticing it.
And I have a feeling that many of us are still doing the same thing without realizing.

```csharp
   float theta = Random.value * 2.0f * Mathf.PI;
   float r = Random.value * radius;
   Vector2 pos = r * new Vector2(Mathf.Cos(theta), Mathf.Sin(theta));
```

Is your code for generating random points inside a circle something like this?
But have you ever tested this code on a large number of samples?

Let’s visualize the random points this code generates with 5,000 samples.
![](/assets/img/posts/random-points-inside-circ-wrong.png)

As you can see, the random point generation that we expected to have a uniform distribution produced a result with a higher density of points near the center of the circle.

The issue comes from the fact that, while the variable `r` is uniformly distributed between 0 and 1, the area of the circle increases quadratically with `r` (specifically, as π * r²).
In other words, the number of `r` values generated between 0 and 0.1 is the same as the number generated between 0.9 and 1.0 — but the difference in the area they cover is not.
For the first range, the area is (0.1² - 0.0²) = 0.01.
For the second range, it's (1.0² - 0.9²) = 1.0 - 0.81 = 0.19.
So, even though both ranges have the same number of samples, the outer region covers a much larger area.

## Fixing the problem

So, to fix the problem, we need to apply a transformation to our uniformly distributed random number in a way that matches the rate at which the area of the circle grows.
That way, each region of the circle will end up with an equal number of points, resulting in a uniform distribution across the entire area.

Since the area of a circle is `A = π * r²`, we can think of `u` — uniformly distributed random number — as representing the normalized area. In that case, we set `u = r²`, which means `r = sqrt(u)`.
This way, the distribution becomes linear in terms of area, and we get points that are uniformly spread across the whole disk.

> To better understand why this transformation gives us a uniform distribution — and how similar ideas apply to other problems — it helps to know that concepts like the `probability density function` **(PDF)** and `cumulative distribution function` **(CDF)** play a key role here.
I won’t go into detail in this post, but if you’re curious, looking into those topics can really help solidify the intuition behind what’s going on.
{: .prompt-tip }

> If you'd like to explore this idea visually, I’ve created an interactive [Desmos graph](https://www.desmos.com/calculator/dr1wt7n8vy) that shows how the transformation affects the distribution.
You can play with it to see how area accumulates differently before and after applying the `r = sqrt(u)` transformation.
{: .prompt-tip }

But for now let’s apply the transformation to our uniform random variable `u` and take another look at the result.

```csharp
   float theta = Random.value * 2.0f * Mathf.PI;
   float r = Mathf.Sqrt(Random.value) * radius;
   Vector2 pos = r * new Vector2(Mathf.Cos(theta), Mathf.Sin(theta));
```

Again, let’s visualize the random points this code generates with 5,000 samples.
![](/assets/img/posts/random-points-inside-circ-correct.png)
