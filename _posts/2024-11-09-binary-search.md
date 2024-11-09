---
title: Binary Search
author: AsyncOperator
date: 2024-11-09 22:55:00 +0300
categories: [programming]
tags: [algorithm]
---


Imagine you have a list of sorted items, like numbers in order, and you need to find one specific number.
Instead of checking each item one by one, binary search lets you *divide and conquer*.
You start in the middle of the list; if the middle item is too high, you ignore the top half, and if it’s too low, you ignore the bottom half.
Repeat this halving process until you find your item—or confirm it’s not there.
This quick narrowing down makes binary search super efficient, especially for large lists.

---

> Unlike my other blog posts, this time I will write the application in a console project.
{: .prompt-info }

```csharp
   static void Main(string[] args)
   {
      const int RANDOM_SEED = 45;
      const int MAX_VALUE = 2_000_000_000;
      const int COUNT = 100_000_000;

      var list = new List<int>(COUNT);

      var random = new Random(RANDOM_SEED);

      for (int i = 0; i < COUNT; i++)
      {
         list.Add(random.Next(MAX_VALUE));
      }

      list.Sort();

      var stopwatch = Stopwatch.StartNew();
      var valueFound = BinarySearch(list, value: 43);
      stopwatch.Stop();

      //var stopwatch = Stopwatch.StartNew();
      //var valueFound = BinarySearch(list, value: 43);
      //stopwatch.Stop();

      Console.WriteLine(valueFound);
      Console.WriteLine("Searching through the list takes about "
         + stopwatch.Elapsed.TotalSeconds + " secs");
   }

   private static bool LinearSearch(List<int> list, int value)
   {
      var count = list.Count;

      for (int i = 0; i < count; i++)
      {
         if (list[i] == value)
         {
            return true;
         }
      }

      return false;
   }

   private static bool BinarySearch(List<int> list, int value)
   {
      var count = list.Count;

      var currMinIndex = 0;
      var currMaxIndex = count - 1;

      while (currMinIndex <= currMaxIndex)
      {
         var midIndex = (int)System.MathF.Floor((currMinIndex + currMaxIndex) / 2);
         var midIndexValue = list[midIndex];

         if (value > midIndexValue)
         {
            currMinIndex = midIndex + 1;
         }
         else if (value < midIndexValue)
         {
            currMaxIndex = midIndex - 1;
         }
         else
         {
            return true;
         }
      }

      return false;
   }
```

> By the way, I use underscores when assigning large numbers to my variables.
This makes it much easier to read large numbers like millions or billions in my text editor.
{: .prompt-tip }
