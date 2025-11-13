# 03 - Abstract Classes and Interfaces

## Why Use Abstraction?

When you start building more complex systems, you’ll notice that many game objects share **similar behaviors** but are implemented differently.

For example:

- Both **Player** and **Enemy** can _take damage_.
- Both **Sword** and **Gun** can _attack_.
- Both **Door** and **Chest** can _be opened_.

You want to handle all of these uniformly (e.g., a single “damage” system or “interaction” script), **without writing separate code for each type**.

That’s where **abstract classes** and **interfaces** come in.
They let you **define what something** **_can do_**, not how it does it.

## Abstract Classes

An abstract class is a blueprint for other classes.
It cannot be instantiated on its own, but provides shared logic and structure for derived classes.

### Example: `CharacterBase`
```csharp
using UnityEngine;

public abstract class CharacterBase : MonoBehaviour
{
    [SerializeField] protected float health = 100f;

    public virtual void TakeDamage(float amount)
    {
        health -= amount;
        Debug.Log($"{name} took {amount} damage. Remaining health: {health}");
        if (health <= 0)
            Die();
    }

    // Abstract methods must be implemented by subclasses
    protected abstract void Die();
}

```

Now, subclasses _must_ implement `Die()`:

```csharp
public class Player : CharacterBase
{
    protected override void Die()
    {
        Debug.Log("Player has fallen!");
        // Respawn or show Game Over
    }
}

public class Enemy : CharacterBase
{
    protected override void Die()
    {
        Debug.Log($"{name} explodes into loot!");
        Destroy(gameObject);
    }
}
```

Use abstract classes when:

- You want shared logic or data
- But also need specific behavior per subclass
- Example: `CharacterBase`, `WeaponBase`, `InteractableBase`

## Interfaces

An interface defines a contract — what methods a class must have — but not how they are implemented.
They contain no logic or data, only method signatures.

### Example: `IDamageable`

```csharp
public interface IDamageable
{
    void TakeDamage(float amount);
}
```

Now, **any** object that can take damage just implements it:

```csharp
public class Enemy : MonoBehaviour, IDamageable
{
    public void TakeDamage(float amount)
    {
        Debug.Log($"Enemy takes {amount} damage!");
    }
}

public class BreakableCrate : MonoBehaviour, IDamageable
{
    public void TakeDamage(float amount)
    {
        Debug.Log("Crate breaks apart!");
        Destroy(gameObject);
    }
}
```

You can now treat _both enemies and crates_ the same way:

```csharp
void DealDamage(GameObject obj, float dmg)
{
    if (obj.TryGetComponent<IDamageable>(out var damageable))
    {
        damageable.TakeDamage(dmg);
    }
}
```

That’s **powerful** - no inheritance needed, and it works with any class that follows the contract!

## Abstract Class vs Interface — What’s the Difference?

| Feature                        | Abstract Class     | Interface                                |
| ------------------------------ | ------------------ |------------------------------------------|
| Can contain logic & data       | ✅ Yes              | ❌ No (and yes)                           |
| Can inherit from another class | ✅ Yes              | ❌ No                                     |
| Can implement multiple         | ❌ Only one         | ✅ Multiple                               |
| Purpose                        | Base functionality | Shared behavior across unrelated classes |
| Example                        | `CharacterBase`    | `IDamageable`, `IInteractable`           |


## Interfaces with Default Implementations (C# 8+)

Traditionally, interfaces could only **define method signatures** - meaning any class implementing the interface had to provide the logic.

Now, **with default interface methods**, you can provide a **shared implementation** directly _inside_ the interface.

This can reduce boilerplate and allow versioning flexibility (adding new interface members without breaking existing code).

## Example: Interface with Default Implementation

You can now do this:
```csharp
public interface IDamageable
{
    int Health { get; set; }

    void TakeDamage(int amount)
    {
        Health -= amount;
        Debug.Log($"Took {amount} damage. Remaining health: {Health}");
    }
}
```

Then, a class can simply implement the interface without needing to write the body again:

```csharp
public class Enemy : MonoBehaviour, IDamageable
{
    public int Health { get; set; } = 50;
}

public class Player : MonoBehaviour, IDamageable
{
    public int Health { get; set; } = 100;

    // Optionally override
    public void TakeDamage(int amount)
    {
        Health -= amount;
        Debug.Log($"Player screams and loses {amount} HP! Remaining: {Health}");
    }
}
```

Notice that:

- `Enemy` doesn’t implement `TakeDamage()` explicitly → it uses the **default one** from the interface.
- `Player` provides a **custom override**, just like inheritance but more flexible.

## Using Both Together

You can combine both for maximum flexibility:

```csharp
public interface IInteractable
{
    void Interact();
}

public abstract class InteractableObject : MonoBehaviour, IInteractable
{
    public abstract void Interact();

    protected void PlaySound(AudioClip clip)
    {
        AudioSource.PlayClipAtPoint(clip, transform.position);
    }
}

public class Door : InteractableObject
{
    public override void Interact()
    {
        Debug.Log("Door opens!");
    }
}

public class Chest : InteractableObject
{
    public override void Interact()
    {
        Debug.Log("Chest opens, loot appears!");
    }
}
```

Now both `Door` and `Chest`:

- Use shared helper logic (`PlaySound`)
- Are accessible through a single interface (`IInteractable`)

## Example: Default Reset Behavior

```csharp
public interface IResettable
{
    void ResetPosition(Transform transform)
    {
        transform.position = Vector3.zero;
    }
}
```

Now you can do this:

```csharp
public class Enemy : MonoBehaviour, IResettable
{
    private void Start()
    {
        ResetPosition(transform); // uses the default implementation
    }
}

public class Boss : MonoBehaviour, IResettable
{
    public void ResetPosition(Transform transform)
    {
        transform.position = new Vector3(10, 5, 0); // override default
    }
}
```

---

### A Few Important Notes

- Unity **does not show** interface members (default or not) in the Inspector.
- If you use `default interface methods` on `MonoBehaviour` components, you must call them explicitly (e.g., `((IResettable)this).ResetPosition(transform)` if you need to ensure the interface version runs).
- You can’t use instance fields in an interface - only **properties** and **auto-implemented methods** with logic.
- Unity serialization won’t store values declared in interfaces - those belong to implementing classes or ScriptableObjects.

---

## Exercise: Build a Simple Interaction System

### Objective:

Create an interaction system where the player can press a key to interact with nearby objects.

### Steps

1. Define an interface IInteractable with a single method void Interact().
2. Implement it on two GameObjects:
   - A Door that prints “Door opens!”
   - A Chest that prints “Chest opens and gives loot!”
3. Create a `PlayerInteraction` script that detects objects in front of the player (using `Physics2D.Raycast`) and calls `Interact()` if they implement `IInteractable`.

## Summary

| Concept            | Description                                                  | Example                              |
| ------------------ | ------------------------------------------------------------ | ------------------------------------ |
| **Abstract Class** | Base blueprint with shared logic                             | `CharacterBase`, `WeaponBase`        |
| **Interface**      | Behavior contract with no logic                              | `IDamageable`, `IInteractable`       |
| **Use Both**       | Abstract class for shared code, interface for shared actions | `InteractableObject : IInteractable` |
