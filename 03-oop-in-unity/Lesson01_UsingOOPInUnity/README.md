# 01 - Using OOP In Unity

### What is OOP?

**Object-Oriented Programming (OOP)** is a programming paradigm that structures code around objects - reusable building blocks that contain both **data** (fields) and **behaviors** (methods).

In Unity, every script you write is a class - meaning you’ve already been using OOP since day one!

OOP helps us:

- Organize code into logical parts
- Reuse and extend functionality
- Keep systems clean, modular, and easy to understand

## The Four Pillars of OOP (in a Unity context)

| Pillar            | Description                                                               | Unity Example                                                |
| ----------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------ |
| **Encapsulation** | Keeping data private and controlling access through methods or properties | `private float health;` with `TakeDamage()` method           |
| **Inheritance**   | A class can extend another class to reuse logic                           | `Enemy : CharacterBase`                                      |
| **Polymorphism**  | Different classes can share the same interface but behave differently     | Different enemies using `Attack()` differently               |
| **Abstraction**   | Hiding unnecessary details from the outside world                         | Using a `Move()` method without knowing how it’s implemented |


## Unity’s Component-Based System = OOP in Action

Unity is **component-oriented**, which is an evolution of OOP principles.
Each **GameObject** is like an “object,” and each **component (script)** attached to it adds specific functionality.

For example:

```csharp
// Each of these is a separate class using OOP principles
public class PlayerMovement : MonoBehaviour { /* handles movement */ }
public class PlayerHealth : MonoBehaviour { /* handles health */ }
public class PlayerInventory : MonoBehaviour { /* handles inventory */ }
```

Instead of one giant Player class, you compose your player out of **several smaller, focused classes**: this is modular OOP!

## Example Code: Creating a Reusable Base Class
Let’s make a base class that represents a character, and then extend it for specific character types.

### CharacterBase.cs
```csharp
using UnityEngine;

public class CharacterBase : MonoBehaviour
{
    [SerializeField] protected float health = 100f;

    public virtual void TakeDamage(float amount)
    {
        health -= amount;
        Debug.Log($"{name} took {amount} damage. Remaining health: {health}");
    }

    public virtual void Attack()
    {
        Debug.Log($"{name} attacks!");
    }
}
```

### Enemy.cs
```csharp
using UnityEngine;

public class Enemy : CharacterBase
{
    public override void Attack()
    {
        Debug.Log($"{name} bites the player!");
    }
}
```

### Player.cs
```csharp
using UnityEngine;

public class Player : CharacterBase
{
    public override void Attack()
    {
        Debug.Log($"{name} swings their sword!");
    }
}
```

Now, both the **Player** and **Enemy** classes reuse the logic from **CharacterBase**, but each defines its own attack behavior.

## Encapsulation Example

We can protect internal data while still allowing controlled access:

```csharp
using UnityEngine;

public class PlayerInventory : MonoBehaviour
{
    private int coins = 0;

    public void AddCoins(int amount)
    {
        coins += amount;
        Debug.Log($"Coins collected: {coins}");
    }

    public int GetCoins()
    {
        return coins;
    }
}
```

Here, other classes can’t just _set_ `coins` directly: they must go through methods like `AddCoins()`.

## Polymorphism Example: Interacting with Multiple Types

You can store multiple subclasses in a single collection using the base class type.

```csharp
CharacterBase[] characters = FindObjectsOfType<CharacterBase>();

foreach (var character in characters)
{
    character.Attack(); // Calls the correct version of Attack()
}
```

Even though they’re different types (Player, Enemy, etc.), Unity will call the right `Attack()` method automatically.

That’s **polymorphism** in action.

## Quick Tips for Using OOP in Unity

- Keep your scripts focused — one responsibility per class
- Use `protected` fields for shared logic between base and derived classes
- Use `virtual` and `override` to extend base behaviors
- Avoid deep inheritance chains — prefer composition when possible
- Use interfaces for shared behaviors across unrelated classes

## Exercise

- Create a base class called `VehicleBase` that includes:
  - A speed variable
  - A `Drive()` method that prints out the vehicle’s name and speed

- Then create:
  - A `Car` class and a `Motorcycle` class that inherit from `VehicleBase`
  - Override the `Drive()` method in each to print unique driving messages

Example output:
````
Car drives at 80 km/h.
Motorcycle zooms at 100 km/h!
````
