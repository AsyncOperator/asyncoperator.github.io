---
title: Cool C# Features
author: AsyncOperator
date: 2025-03-16 10:15:00 +0300
categories: [programming]
tags: [clean code]
---

ValueTuple to swap values
```csharp
int[] arr = new int[] { 1, 2 };

// Old fashioned technique
int temp = arr[0];
arr[0] = arr[1];
arr[1] = temp;

// Classy way to do it
(arr[0], arr[1]) = (arr[1], arr[0]);
```

---

Use discard `_` variable for unused variables
```csharp
// Returns true if hit something
private bool CollisionDetection(out object hitObject)
{
  // ...
}

// Just interested whether collision occured or not
bool collisionOccured = CollisionDetection(out _);
```

---

Use ValueTuple instead of multiple `out` declared method parameters
```csharp
private void Foo(out int value1, out int value2)
{
  // ...
}

// ValueTuple solution
private (int value1, int value2) Foo()
{
  // ...
}
```

---

Use `switch expression` to save some lines and improve readability
```csharp
// The old, boring switch statement implementation
string response;
switch (message)
{
    case "Hey!":
    {
        response = "Hi!";
    } break;
    case "Goodbye!":
    {
        response = "See you later!";
    } break;
    default:
    {
        response = string.Empty;
    } break;
}

// Much compact and good looking switch expression implementation
string response = message switch
{
    "Hey!" => "Hi!",
    "Goodbye!" => "See you later!",
    _ => string.Empty
};
```

These are the ones I can think of right now, I may add new ones in the future.
