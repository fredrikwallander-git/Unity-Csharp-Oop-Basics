# 04 - Input And Movement

## Learning Goals

- Understand how to use **Unity’s Input System** (keyboard or controller).
- Move and rotate objects using Transform.
- Learn what **Transforms** are in 3D space (position, rotation, scale).
- Understand how **matrices** represent transformations in Unity and 2D/3D graphics in general.

## Theory Summary

### Unity Input Basics

Unity provides two main systems for player input:

- **The old Input Manager** (`Input.GetAxis`, `Input.GetKey`, etc.)
- **The new Input System package** (action-based, more flexible)

For simplicity in this lesson, we’ll start with the **classic system**, since it’s perfect for quick prototypes.

---
### Example: Basic Keyboard Movement

```csharp
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    public float moveSpeed = 5f;

    void Update()
    {
        float horizontal = Input.GetAxis("Horizontal");
        float vertical = Input.GetAxis("Vertical");

        Vector3 direction = new Vector3(horizontal, vertical, 0f);
        transform.Translate(direction * moveSpeed * Time.deltaTime);
    }
}
```
Explanation:
- `Input.GetAxis("Horizontal")` gives a smooth value from -1 to +1 depending on key press (`A`/`D` or `←`/`→`).
- `transform.Translate()` moves the GameObject in local space.
- Multiplying by `Time.deltaTime` ensures frame rate–independent movement.

---

### Understanding Transforms

Every GameObject in Unity has a **Transform** component that defines:

- **Position** → where it is
- **Rotation** → which direction it faces
- **Scale** → how big it is

Together, these form the **local-to-world transformation** - how Unity knows where the object is in the world.

The Three Spaces

- **Local Space** – relative to the object’s parent.
- **World Space** – relative to the scene’s global coordinates.
- **Screen Space** – coordinates on your screen (used for UI).

You can think of the Transform as a **mini coordinate system** attached to the object.

---

### Transform Properties

```csharp
transform.position    // world position (Vector3)
transform.localPosition // position relative to parent
transform.rotation     // world rotation (Quaternion)
transform.localScale   // size relative to parent
```

---

### Matrices: The Math Behind Transforms

At the core, every Transform is represented internally by a **Matrix** — specifically, a **4×4 transformation matrix**.

A matrix combines **translation, rotation, and scaling** in a single structure.

Here’s how Unity conceptually builds an object’s final position:

>Local → Parent → World

Each step applies a **matrix multiplication**:

>WorldMatrix = ParentMatrix × LocalMatrix

That’s why when you move a parent GameObject, its children move with it —
they inherit its transformation matrix.

### Visualization Example

Imagine a simple 2D world:

| Operation     | Description          | Example                                                     |
| ------------- | -------------------- | ----------------------------------------------------------- |
| **Translate** | Moves the object     | `transform.Translate(2, 0, 0)` → move right 2 units         |
| **Rotate**    | Spins around an axis | `transform.Rotate(0, 0, 90)` → rotate 90° clockwise         |
| **Scale**     | Resizes the object   | `transform.localScale = new Vector3(2, 2, 1)` → double size |

Each of these operations updates the internal 4×4 matrix, which defines how Unity draws the object in the scene.

---

### Why Matrices Matter

Even if you don’t need to do the math manually now, understanding that transforms are matrix multiplications helps later with:

- Camera view transformations
- Local vs world movement confusion
- Understanding an ECS’s `LocalTransform`
- Shader and graphics programming

---

## Example Code - Local vs World Movement
```csharp
void Update()
{
    if (Input.GetKey(KeyCode.W))
        transform.Translate(Vector3.up * Time.deltaTime * 5f, Space.World);

    if (Input.GetKey(KeyCode.S))
        transform.Translate(Vector3.down * Time.deltaTime * 5f, Space.World);

    if (Input.GetKey(KeyCode.A))
        transform.Translate(Vector3.left * Time.deltaTime * 5f, Space.Self);

    if (Input.GetKey(KeyCode.D))
        transform.Translate(Vector3.right * Time.deltaTime * 5f, Space.Self);
}
```

### Try rotating your GameObject 45° in the Editor.

Then run the game — notice how `Space.Self` moves along the object’s local axes,
while `Space.World` moves in the global directions

## Exercises

### Exercise 1: Move and Rotate

- Create a `Player` GameObject with a visible sprite.
- Add this script:

```csharp
using UnityEngine;

public class SimpleMover : MonoBehaviour
{
    public float moveSpeed = 3f;
    public float rotationSpeed = 180f;

    void Update()
    {
        float move = Input.GetAxis("Vertical") * moveSpeed * Time.deltaTime;
        float turn = Input.GetAxis("Horizontal") * rotationSpeed * Time.deltaTime;

        transform.Translate(Vector3.up * move);
        transform.Rotate(Vector3.forward * -turn);
    }
}
```

- Move with `W/S` and rotate with `A/D`.

Now you have a basic movement script for 2D player controls.

---

### Exercise 2: Follow a Target

- Create a second GameObject as a target.
- Write a script to make your player move toward it.

```csharp
public class FollowTarget : MonoBehaviour
{
    public Transform target;
    public float speed = 2f;

    void Update()
    {
        Vector3 direction = (target.position - transform.position).normalized;
        transform.position += direction * speed * Time.deltaTime;
    }
}
```

### What’s happening here:

- We compute a direction vector using subtraction.
- `.normalized` makes it length 1 (so speed is consistent).
- Multiply by speed and deltaTime to move smoothly.

---

### Challenge Exercise: Matrix Thinking

Create a parent GameObject called `Spaceship`.
Inside it, create a child called `Laser`.

Move and rotate the spaceship.
Notice how the laser inherits all the movement automatically -
because its **local transform** is multiplied by the spaceship’s world matrix.

### Try this experiment:

- Move the **child** independently.
- Rotate the **parent**.
- Observe how their coordinate systems combine.

---

## Key Takeaways

- Input is gathered each frame using `Input.GetAxis` or `Input.GetKey`.
- `Transform` defines an object’s position, rotation, and scale in space.
- Transformations are internally stored as 4×4 matrices.
- Movement in `Space.Self` follows the object’s local rotation.
- Movement in `Space.World` follows the global axes.
- Matrix multiplication is the reason hierarchies and parenting work.

---

## New Input System (short version)

### Step 1: Install (if necessary) and Enable

- Go to **Window → Package Manager → Unity Registry**.
- Install **Input System**.
- When prompted, switch the project to the new input backend.

_In later versions Unity has the new input system added and enabled by default so you do not have to follow these steps._

---

### Step 2: Create an Input Actions Asset

- Right-click in the **Project window → Create → Input Actions**.
- Open the asset.
- Create a “**Player**” **Action Map**.
- Add an **Action** called `Move`, set its **Action Type** to _Value_, and **Control Type** to _Vector2_.
- Add bindings like:
- Keyboard → WASD / Arrow Keys
- Gamepad → Left Stick

Save the asset and **Generate C# Class**.

_Again, in the latest Unity versions, this is already added and completed for you by default._

---

### Step 3: Player Movement Script Example

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerController : MonoBehaviour
{
    private PlayerInputActions inputActions;
    private Vector2 moveInput;

    public float moveSpeed = 5f;

    private void Awake()
    {
        inputActions = new PlayerInputActions();
    }

    private void OnEnable()
    {
        inputActions.Player.Enable();
        inputActions.Player.Move.performed += OnMove;
        inputActions.Player.Move.canceled += OnMove;
    }

    private void OnDisable()
    {
        inputActions.Player.Move.performed -= OnMove;
        inputActions.Player.Move.canceled -= OnMove;
        inputActions.Player.Disable();
    }

    private void OnMove(InputAction.CallbackContext context)
    {
        moveInput = context.ReadValue<Vector2>();
    }

    private void Update()
    {
        Vector3 move = new Vector3(moveInput.x, 0, moveInput.y);
        transform.Translate(move * moveSpeed * Time.deltaTime, Space.World);
    }
}
```

- The **New Input System** allows for **multiple control schemes** (keyboard, gamepad, touch).
- Inputs are **action-based**, not hardcoded.
- You can easily map **different devices** to the same gameplay actions.

---

### Mini Exercise for New Input System

- Create a simple **3D cube or 2D sprite**.
- Implement player movement using the New Input System.
- Print the player’s world position to the console each frame.
- (Bonus) Rotate the player to face the direction of movement.