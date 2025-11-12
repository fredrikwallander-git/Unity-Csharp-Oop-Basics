# 02 - Inheritance vs Composition

### The Core Idea

In traditional OOP (like in C# console or enterprise programming), **inheritance** is one of the key tools to reuse code:
You define a **base class**, and **derived classes** extend or override its behavior.

However, in Unity, that approach can get messy fast - especially when you try to build gameplay systems that need flexibility (e.g., an enemy that can fly _and_ shoot _and_ patrol).

That’s where **composition** comes in.
Instead of relying on a tall class hierarchy, you **compose** behaviors by adding _components_ to GameObjects.

## Inheritance Recap

Inheritance allows one class to extend another and reuse its logic.

```csharp
public class CharacterBase : MonoBehaviour
{
    [SerializeField] protected float health = 100f;

    public virtual void TakeDamage(float damage)
    {
        health -= damage;
        Debug.Log($"{name} took {damage} damage!");
    }
}

public class Enemy : CharacterBase
{
    public override void TakeDamage(float damage)
    {
        base.TakeDamage(damage);
        Debug.Log($"{name} growls angrily!");
    }
}
```

Pros:

- Code reuse
- Logical grouping of shared behaviors

Cons:

- Becomes rigid as hierarchy deepens
- Hard to mix and match features
- Difficult debugging when base logic changes

## Composition in Unity

Composition means **building complex behavior by combining small, reusable components**.
Each script focuses on _one thing only_ - movement, attack, health, etc.

For example:

```csharp
// Handles movement
public class MoveBehaviour : MonoBehaviour
{
    public float speed = 5f;

    void Update()
    {
        transform.Translate(Vector2.right * speed * Time.deltaTime);
    }
}

// Handles shooting
public class ShootBehaviour : MonoBehaviour
{
    public GameObject bulletPrefab;
    public Transform firePoint;

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            Instantiate(bulletPrefab, firePoint.position, firePoint.rotation);
        }
    }
}
```

Now, you can create:

- A Player GameObject with **MoveBehaviour** and **ShootBehaviour**
- An Enemy GameObject with **MoveBehaviour** only

Both share code, but there’s no inheritance between them!

## Combining Both Approaches

Unity encourages you to use **inheritance** for _conceptual hierarchies_
(e.g., a base class for shared logic),
and **composition** for _behavior modularity_.

For instance:

```csharp
public abstract class CharacterBase : MonoBehaviour
{
    [SerializeField] protected float health = 100f;
    public abstract void Attack();
}

public class Player : CharacterBase
{
    [SerializeField] private MoveBehaviour movement;
    [SerializeField] private ShootBehaviour shooter;

    public override void Attack()
    {
        shooter.TryShoot();
    }
}
```

Here, **Player** inherits from `CharacterBase` but _composes_ `MoveBehaviour` and `ShootBehaviour`.
That gives the best of both worlds.

---

### Visualizing the Difference

**Inheritance Tree (traditional OOP)**

```
CharacterBase
├── Player
│   └── MagePlayer
│       └── FireMagePlayer
└── Enemy
    └── FlyingEnemy
        └── BossEnemy
```

→ Rigid and hard to modify.

**Composition (Unity style)**

```
Player GameObject
├── MoveBehaviour
├── JumpBehaviour
├── ShootBehaviour
└── HealthSystem
```

→ Flexible and easy to mix & match.

## Example: Switching to Composition

Let’s say you first designed your enemies using inheritance:

```csharp
public class Enemy : MonoBehaviour
{
    public virtual void Move() { /* default movement */ }
}

public class FlyingEnemy : Enemy
{
    public override void Move() { /* flying logic */ }
}

public class BossEnemy : FlyingEnemy
{
    public override void Move() { /* hover + attack logic */ }
}
```

Now if you want a **ground boss** that doesn’t fly?
You’ll need a whole new branch in your inheritance tree.

Instead, using composition:

```csharp
public class Enemy : MonoBehaviour
{
    private IMovement movement;

    void Awake()
    {
        movement = GetComponent<IMovement>();
    }

    void Update()
    {
        movement?.Move(transform);
    }
}

public interface IMovement
{
    void Move(Transform transform);
}

public class GroundMovement : MonoBehaviour, IMovement
{
    public void Move(Transform transform)
    {
        transform.Translate(Vector2.left * Time.deltaTime);
    }
}

public class FlyingMovement : MonoBehaviour, IMovement
{
    public void Move(Transform transform)
    {
        transform.Translate(Vector2.up * Mathf.Sin(Time.time) * Time.deltaTime);
    }
}
```

Now you can **swap out movement styles** just by changing a component.
No need for deep inheritance trees!

## Exercise: Building Enemies with Inheritance vs Composition

Goal:

Understand the difference between using **inheritance** and **composition** in Unity by creating two versions of an enemy system:
1. One version using **inheritance**
2. Another using **composition**

---

Step 1

Create a simple inheritance tree where different enemy types extend a base class:

- Create an `EnemyBase` class which inherits from `MonoBehaviour`
  - Add a **serialized protected float** field of `speed`
  - Add a Virtual Move method that translates the transform to the left or right
- Create two different `Enemy` types (flying and fast enemy) that inherits from the `EnemyBase` class
  - The flying enemy should **extend** the `Move()` method by also flying
  - The fast enemy should increase the movement speed (maybe multiply it by 2)

Now test and see by creating three `GameObjects` and attaching the `EnemyBase`, `FlyingEnemy` and `FastEnemy` to each.

Press play and observe the behaviour.

---

Step 2

Rebuild it all with composition approach, instead of relying on inheritance, make small modular component that can be mixed and matched.

- Create a `MoveLeft` or `MoveRight` component
  - Add movement in the `Update()` method using a speed variable
- Create a `FlyingMotion` component
  - Add two fields to control `amplitude` and `frequency` (small values like 0.5f and 1.0f)
  - Add a private field to store the start position
- In the `Start()` method, cache the starting position
- In the `Update()` method, update the transform position using:
  - `startPos.y + Mathf.Sin(Time.time * frequency) * amplitude`
  - To calculate a new Y position each frame

Now test it out by adding a `GameObject` which uses the `MoveLeft` or `MoveRight` component and another one that uses the `MoveLeft` or `MoveRight` + `FlyingMotion`.

Both uses the same small, reusable pieces: No inheritance needed!