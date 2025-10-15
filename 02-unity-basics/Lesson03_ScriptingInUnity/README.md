# 03 - Scripting In Unity

## Learning Goals

- Understand how Unity executes your scripts.
- Learn about the MonoBehaviour lifecycle (`Awake()`, `Start()`, `Update()`, etc.).
- Learn how to control which fields appear in the Inspector.
- Revisit encapsulation and how it applies in Unity.

## Theory Summary

Every script that inherits from `MonoBehaviour` follows a specific order of events.
Unity automatically calls these methods when the game runs.

Here’s the most common sequence:

```scss
Awake()        → Called when the GameObject is created (even before it's enabled)
OnEnable()     → Called when the object becomes active
Start()        → Called before the first frame update
Update()       → Called once per frame
FixedUpdate()  → Called on a fixed timer (for physics)
LateUpdate()   → Called after all Updates (useful for following cameras)
OnDisable()    → Called when the object is disabled
OnDestroy()    → Called when the object is destroyed
```

Hint: You don’t call these methods yourself - Unity does it for you.

---
## Example Code - The Lifecycle in Action
```csharp
using UnityEngine;

public class LifecycleExample : MonoBehaviour
{
    void Awake()
    {
        Debug.Log("Awake: Object initialized.");
    }

    void Start()
    {
        Debug.Log("Start: First frame about to begin.");
    }

    void Update()
    {
        Debug.Log("Update: Frame running...");
    }

    void OnDestroy()
    {
        Debug.Log("OnDestroy: Object destroyed.");
    }
}
```
---

Try attaching this to a GameObject and watch the Console as you press **Play** and **Stop**.

You’ll see how Unity automatically calls these methods in order.

---
## Understanding the Inspector and Fields

When you add a script to a GameObject, Unity shows **all public fields** in the **Inspector** that can be serialized by Unity.

Example:

```csharp
public class Player : MonoBehaviour
{
    public string playerName = "Foxy";
    public int health = 100;
}
```

These values can be changed directly in the Inspector — no need to edit code or restart the game.

---
## Encapsulation in Unity

In pure C#, we learned:

- `public` → visible and accessible by everyone.
- `private` → hidden and only accessible inside the same class.

In Unity, **private fields are hidden from the Inspector** by default.
But sometimes you want to keep them private **while still visible in the Inspector**.
That’s where `[SerializeField]` comes in.

```csharp
public class Player : MonoBehaviour
{
    [SerializeField] private int health = 100;
    [SerializeField] private float speed = 5f;

    void Update()
    {
        transform.Translate(Vector3.right * speed * Time.deltaTime);
    }
}
```

Now `health` and `speed` are:
- Private (protected from outside modification in code),
- But **still editable in the Inspector**.

This is good practice, it keeps your variables safe and your Inspector flexible.

---
## Quick Comparison

| Modifier                   | Visible in Inspector | Accessible from other scripts |
| -------------------------- | -------------------- | ----------------------------- |
| `public`                   | ✅ Yes                | ✅ Yes                         |
| `private`                  | ❌ No                 | ❌ No                          |
| `[SerializeField] private` | ✅ Yes                | ❌ No                          |
| `[HideInInspector] public` | ❌ No                 | ✅ Yes                         |

---
## Key Takeaways

- Unity automatically calls MonoBehaviour methods like `Start()` and `Update()`.
- `Awake()` happens even before `Start()`.
- Use `[SerializeField]` to expose private fields in the Inspector safely.
- Encapsulation is about protecting data while allowing safe access through methods.

---
## Exercises

### Exercise 1: Lifecycle Explorer

Create a new script that logs messages in:
- `Awake()`
- `Start()`
- `Update()`
- `OnEnable()`
- `OnDisable()`

Then:
- Attach it to a GameObject.
- Enable/disable the GameObject during Play mode.
- Observe the Console log order.

### Exercise 2: Inspector Practice

- Create a `Car` script:

```csharp
public class Car : MonoBehaviour
{
    public string model = "Speedster";
    [SerializeField] private float speed = 10f;

    void Update()
    {
        transform.Translate(Vector3.right * speed * Time.deltaTime);
    }
}
```

- Attach it to a GameObject.
- Modify the `model` and `speed` values in the Inspector.
- Observe how the behavior changes at runtime.

### Challenge Exercise: Keep It Safe

Modify your `Car` class so that:

- The speed can be increased/decreased only through methods (not directly in code from other scripts).
- The method `Boost()` **temporarily doubles the speed**.

```csharp
public void Boost()
{
    speed *= 2f;
}
```