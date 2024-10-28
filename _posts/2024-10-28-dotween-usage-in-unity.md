---
title: Better way to work with DOTween
author: AsyncOperator
date: 2024-10-28 23:05:00 +0300
categories: [game development, game programming]
tags: [clean code]
---


In my experience with DOTween, it has proven invaluable across numerous projects.
Typically, animation parameters are defined as class-level variables, which works well for a small number of similar objects in a scene.
However, for objects appearing hundreds or even thousands of times simultaneously, this approach becomes less efficient, both for memory usage and code clarity.
Using class-level variables in these cases is best avoided to maintain optimal performance and readability.

```csharp
public class MyClass : MonoBehaviour
{
   [Header("Tween Settings")]
   [SerializeField]
   private int vibrato;
   [SerializeField]
   private float duration, strength, randomness;
   [SerializeField]
   private bool snapping, fadeOut;
   
   private void PlayShakePositionTween()
   {
      transform.DOShakePosition(duration, strength, vibrato, randomness, snapping, fadeOut);
   }
}
```

<br>

We can utilize simple scriptable objects for this purpose.
This way, instead of storing animation parameters at the class level for each object, we can read them from a single asset file.
This eliminates the need for dozens of class-level animation parameter variables and allows us to create new scriptable objects for different type objects that may use the same animation in the future.

```csharp
[CreateAssetMenu(fileName = "NewShakePositionParams.asset", menuName = "Tween/Position/Shake params")]
public class ShakePositionParamsSO : ScriptableObject
{
   public int vibrato;
   public float duration, strength, randomness;
   public bool snapping, fadeOut;
}
```

<br>

> The state of the source code after implementing the scriptable object solution
{: .prompt-info }

```csharp
public class MyClass : MonoBehaviour
{
   //[Header("Tween Settings")]
   //[SerializeField]
   //private int vibrato;
   //[SerializeField]
   //private float duration, strength, randomness;
   //[SerializeField]
   //private bool snapping, fadeOut;

   [SerializeField]
   private ShakePositionParamsSO shakePositionParams;

   private void PlayTween()
   {
      //transform.DOShakePosition(duration, strength, vibrato, randomness, snapping, fadeOut);

      transform.DOShakePosition(shakePositionParams.duration,
         shakePositionParams.strength,
         shakePositionParams.vibrato,
         shakePositionParams.randomness,
         shakePositionParams.snapping,
         shakePositionParams.fadeOut);
   }
}
```
