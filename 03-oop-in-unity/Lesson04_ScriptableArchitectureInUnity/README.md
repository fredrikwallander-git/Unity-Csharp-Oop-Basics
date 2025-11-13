# 04 - Scriptable Architecture in Unity

## Why Scriptable Architecture?

So far, most of our code has stored game data inside **MonoBehaviours** - like player health, weapon stats, or movement speeds.

That works fine for small projects, but as soon as you want to:

- Reuse data between multiple objects,
- Change values without editing prefabs, or
- Keep logic and data separate for maintainability...

…it gets messy.

That’s where **ScriptableObjects** shine.

They let you **store data as assets**, separate from scene objects - following OOP’s **abstraction and encapsulation** principles, but in a Unity-friendly way.

## What Are ScriptableObjects?

A **ScriptableObject** is a special kind of class in Unity that:

- Lives as an **asset** in your project (not in a scene).
- Stores **persistent data**.
- Does **not** depend on a GameObject or MonoBehaviour.
- Can be **referenced by multiple objects** at once.

They’re perfect for shared configuration and modular systems.

## Example: Player Stats as Data Assets

Without ScriptableObjects, we might do this:
```csharp
public class Player : MonoBehaviour
{
    public float moveSpeed = 5f;
    public int maxHealth = 100;
}
```

But if you want to tweak player stats, or share the same stats across multiple levels, you’d have to edit every prefab.

Let’s fix that.

### Step 1 - Create a ScriptableObject

```csharp
using UnityEngine;

[CreateAssetMenu(fileName = "NewCharacterStats", menuName = "Game/Character Stats")]
public class CharacterStats : ScriptableObject
{
    public float moveSpeed = 5f;
    public int maxHealth = 100;
}
```

### Step 2 - Use It in a MonoBehaviour

```csharp
using UnityEngine;

public class Player : MonoBehaviour
{
    [SerializeField] private CharacterStats stats;

    private int currentHealth;

    void Start()
    {
        currentHealth = stats.maxHealth;
    }

    void Update()
    {
        transform.Translate(Vector2.right * stats.moveSpeed * Time.deltaTime);
    }
}
```

Now, you can create multiple stat files:

- `HeroStats.asset`
- `FastEnemyStats.asset`
- `BossStats.asset`

And reuse them without **editing your prefabs**.
That’s **data-driven design** - a core idea behind OOP and modular architecture.

## Using ScriptableObjects for Events (Decoupling Systems)

ScriptableObjects aren’t just for data — they’re great for decoupling systems too.
Instead of direct references between scripts, we can use Scriptable Events.

Example:
You want your `Enemy` to deal damage and your `UI` to update when the player gets hit - but you don’t want those classes to know about each other.

### Step 1 — Create an Event Asset

```csharp
using UnityEngine;
using System.Collections.Generic;

[CreateAssetMenu(fileName = "GameEvent", menuName = "Game/Event")]
public class GameEvent : ScriptableObject
{
    private List<GameEventListener> listeners = new List<GameEventListener>();

    public void Raise()
    {
        foreach (var listener in listeners)
            listener.OnEventRaised();
    }

    public void RegisterListener(GameEventListener listener) => listeners.Add(listener);
    public void UnregisterListener(GameEventListener listener) => listeners.Remove(listener);
}
```

### Step 2 — Create a Listener Component

```csharp
using UnityEngine;
using UnityEngine.Events;

public class GameEventListener : MonoBehaviour
{
    public GameEvent gameEvent;
    public UnityEvent response;

    private void OnEnable() => gameEvent.RegisterListener(this);
    private void OnDisable() => gameEvent.UnregisterListener(this);

    public void OnEventRaised() => response.Invoke();
}
```

Now any object can **raise** an event and **listen** for it - no direct coupling!

```csharp
public class Enemy : MonoBehaviour
{
    [SerializeField] private GameEvent onEnemyDeath;

    public void Die()
    {
        Debug.Log("Enemy has died!");
        onEnemyDeath.Raise();
        Destroy(gameObject);
    }
}
```

And your UI or audio system can listen to that event via a `GameEventListener`.
This makes your code modular, reusable, and easy to extend - an OOP principle in Unity form.


## Combining ScriptableObjects with Interfaces

You can also make ScriptableObjects implement interfaces for advanced modularity.

Example: a **damage effect system** where each effect is a ScriptableObject.

```csharp
public interface IDamageEffect
{
    void Apply(GameObject target, float damage);
}

[CreateAssetMenu(fileName = "FireEffect", menuName = "Game/Damage Effect/Fire")]
public class FireEffect : ScriptableObject, IDamageEffect
{
    public void Apply(GameObject target, float damage)
    {
        Debug.Log($"{target.name} takes {damage} fire damage!");
    }
}
```

Then use it in your attack logic:

```csharp
public class Weapon : MonoBehaviour
{
    [SerializeField] private IDamageEffect effect;

    public void Attack(GameObject target)
    {
        effect?.Apply(target, 10);
    }
}
```

This allows you to **drag** and **drop** effects in the Inspector - no code changes needed when adding new types of damage!

## Example System: ScriptableObject-Driven Weapons

A short, game-ready example.

### Step 1 — Define a WeaponData asset

```csharp
[CreateAssetMenu(fileName = "NewWeapon", menuName = "Game/Weapon")]
public class WeaponData : ScriptableObject
{
    public string weaponName;
    public float damage;
    public float cooldown;
}
```

### Step 2 — Create a Weapon component

```csharp
public class Weapon : MonoBehaviour
{
    [SerializeField] private WeaponData data;

    private float nextAttackTime;

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space) && Time.time >= nextAttackTime)
        {
            Debug.Log($"{data.weaponName} attacks for {data.damage} damage!");
            nextAttackTime = Time.time + data.cooldown;
        }
    }
}
```

Now, every weapon in your game (sword, bow, gun, etc.) can be a data asset instead of hardcoded logic.

## Exercise: Make a Scriptable-Based Item System

### Objective:

Create an item system that uses ScriptableObjects to define item data.

### Steps:

1. Create an `ItemData` ScriptableObject with:
   - `string itemName`
   - `Sprite icon`
   - `string description`
2. Create a `PickupItem` MonoBehaviour that references `ItemData` and displays its name on pickup.
3. Add a simple `Inventory` class that stores picked-up items (in a `List<ItemData>`).

### Bonus: 
Add ScriptableObjects for consumable effects that can modify player stats when used.

## Summary

| Concept                | Description                                              | Example                         |
| ---------------------- | -------------------------------------------------------- | ------------------------------- |
| **ScriptableObject**   | Serializable asset that stores data outside scenes       | Player stats, weapons, items    |
| **Scriptable Events**  | Decouple systems with event assets                       | Enemy death → UI updates        |
| **Data-driven Design** | Logic reads from data assets instead of hardcoded values | `WeaponData`, `CharacterStats`  |
| **Combine with OOP**   | Use with interfaces and composition for flexible design  | Damage effects, ability systems |

ScriptableObjects make Unity projects **scalable, flexible, and modular** - they’re one of the cleanest ways to apply OOP principles the _Unity way_.

They separate **data from logic**, reduce dependencies, and make it easier to balance and expand your game.