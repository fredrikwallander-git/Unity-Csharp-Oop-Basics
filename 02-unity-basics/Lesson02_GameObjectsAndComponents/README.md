# 02 - GameObjects And Components

## Learning Goals

By the end of this lesson, students will be able to:

- Understand what a GameObject is in Unity
- Understand what a Component is and how they add behavior
- Understand the high-level concept of the Unity base class: `MonoBehaviour`
- Add and remove components from GameObjects
- Create and attach scripts as custom components
- Use the Transform component to move, rotate, and scale objects through code
- Understand the difference between active and inactive GameObjects

## Theory Summary
### What Is a GameObject?

- A GameObject is the fundamental building block of every Unity scene.
- It acts as a container that can hold various components, which define what the object is and how it behaves.

Without components, a GameObject is just an empty container.

| GameObject | Components                                                |
| ---------- | --------------------------------------------------------- |
| Player     | Transform, Sprite Renderer, Rigidbody2D, PlayerController |
| Light      | Transform, Light                                          |
| Camera     | Transform, Camera                                         |

### What Are Components?

A Component is a piece of functionality that can be attached to a GameObject.
Unity’s component system lets you mix and match features without inheritance.

Examples of built-in components:

- Transform → position, rotation, scale
- MeshRenderer or SpriteRenderer → makes an object visible
- Collider → enables collision detection
- Rigidbody → adds physics
- AudioSource → plays sound
- MonoBehaviour (script) → custom logic you write yourself

You can think of the **GameObject** as a lego starting plate where each **Component** is a different lego brick that gives it new powers.

---

### Component-Based Design

Unity follows composition over inheritance, similar to OOP’s composition principle:

>Instead of “Player is a Character”,
we think “Player has Movement, has Renderer, has Collider”.

This means we can combine different components to create new behaviors quickly.

---

### The Transform Component

Every GameObject automatically has a Transform component, it controls where and how the object appears in the scene.

| Property | Description                                 |
| -------- | ------------------------------------------- |
| Position | X, Y, Z coordinates in world or local space |
| Rotation | Object’s rotation (Euler angles)            |
| Scale    | Size in each axis                           |

---

## Creating Your First Component

Create a new C# script in Unity and attach it to a GameObject.

```csharp
using UnityEngine;

public class Mover : MonoBehaviour
{
    public float speed = 5f;

    void Update()
    {
        transform.Translate(Vector3.right * speed * Time.deltaTime);
    }
}
```

This `Mover` component makes a GameObject move to the right every frame.

Attach it to a GameObject with a visible sprite, press Play, and watch it move!

---

## Introduction To MonoBehaviour

When you create a script in Unity, you’ll notice it inherits from `MonoBehaviour`:

```csharp
public class Mover : MonoBehaviour {
    // fields etc..
}
```

So what does that mean?

`MonoBehaviour` is a special Unity base class that connects your script to the Unity Engine lifecycle.

It gives you access to built-in event methods such as:

- `Start()` → Runs once before the first frame.
- `Update()` → Runs once per frame.
- `FixedUpdate()` → Runs at a fixed time step (used for physics).
- `OnCollisionEnter2D()` → Called when a collider touches another collider.
- `OnTriggerEnter2D()` → Called when a trigger collider overlaps another.

Without MonoBehaviour, these Unity-specific methods won’t run automatically.

## Example Code 1 - Moving Objects Through Code

- Create a new script: `Mover.cs`
- Attach it to a cube (`GameObject` → `3D Object` → `Cube`)

```csharp
using UnityEngine;

public class Mover : MonoBehaviour
{
    public float speed = 3f;

    void Update()
    {
        // Move forward continuously
        transform.Translate(Vector3.forward * speed * Time.deltaTime);
    }
}
```
---
Press ▶️ (Play) — your cube should move forward in the scene.

## Example Code 2 - Rotating Objects Through Code

Create another script: `Spinner.cs`

```csharp
using UnityEngine;

public class Spinner : MonoBehaviour
{
    public float rotationSpeed = 90f;

    void Update()
    {
        transform.Rotate(0f, rotationSpeed * Time.deltaTime, 0f);
    }
}
```

Attach it to a **different GameObject** (like a sphere).
Try setting different rotation speeds in the Inspector.

---

### Understanding the Inspector

When you select a GameObject:

- Each **component** appears as a section in the Inspector.
- You can **enable/disable** components using the checkbox beside their name.
- You can **reorder** components by dragging their titles.
- You can **add components** via the `Add Component` button.

---

## Example Code 3 - Accessing Other Components

You can access other components from your script using `GetComponent<T>()`:

```csharp
using UnityEngine;

public class ColorChanger : MonoBehaviour
{
    private Renderer rend;

    void Start()
    {
        rend = GetComponent<Renderer>();
    }

    void Update()
    {
        float colorValue = Mathf.PingPong(Time.time, 1f);
        rend.material.color = new Color(colorValue, 0.5f, 1f - colorValue);
    }
}
```

Attach this script to a cube or sphere to make it change color over time.

---

### Active vs Inactive GameObjects

GameObjects can be **enabled** or **disabled** in the Hierarchy:

- Active → visible, runs logic
- Inactive → hidden, doesn’t run logic or physics

You can toggle this with:

- `gameObject.SetActive(false);`

---

### Destroying GameObjects

You can remove a GameObject from the scene using:

- `Destroy(gameObject);`

Example — make an object disappear after 3 seconds:

```csharp
void Start()
{
    Destroy(gameObject, 3f);
}
```

---

### Key Concepts Recap

| **Concept**       | **Description**                                   |
| ------------- | --------------------------------------------- |
| GameObject    | The basic entity in Unity                     |
| Component     | A piece of behavior or data                   |
| Transform     | Defines position, rotation, scale             |
| MonoBehaviour | Base class for custom scripts                 |
| GetComponent  | Used to access other components               |
| Active State  | Determines whether the object updates/renders |

---

## Summary

- GameObjects are **containers** for Components
- Components define **what** a GameObject does
- The **Transform** component is always present (sometimes called **RectTransform** ( UI ))
- **Custom scripts** are also Components
- You can combine **multiple** Components to create **complex** behavior
- **MonoBehaviour** allows your scripts to “plug in” to Unity’s update loop.
- The **Unity lifecycle** gives you key event hooks like `Start()`, `Update()`, and `FixedUpdate()`.

---

## Exercises

### Exercise 1 — Floating Object

Create a script `FloatingObject.cs` that makes a cube float up and down smoothly using `Mathf.Sin()` and `Time.time`.

---

### Exercise 2 — Color Pulsing

Modify `ColorChanger` so the color pulses faster or slower based on a public variable `speed`.

---

###  3 — Dynamic Interaction

- Create a GameObject with both `Mover` and `Spinner` components.
- Allow enabling/disabling either component via a keypress in `Update()`:

```csharp
if (Input.GetKeyDown(KeyCode.M)) mover.enabled = !mover.enabled;
if (Input.GetKeyDown(KeyCode.S)) spinner.enabled = !spinner.enabled;
```

- Observe how turning components on/off changes behavior.

---

## Optional Challenge

Create a simple scene called “Moving” and add the following to it:

- A sphere that moves forward and bounces back when reaching an edge
- A cube that rotates in place
- A capsule that changes color dynamically
- Each with its own custom script component