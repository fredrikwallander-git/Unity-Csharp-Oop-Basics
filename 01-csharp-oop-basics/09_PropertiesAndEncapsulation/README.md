# 09 - Properties & Encapsulation

## Learning Goals

By the end of this lesson, students will be able to:

- Understand the concept of encapsulation in OOP
- Use fields, properties, and access modifiers (public, private, etc.) effectively
- Protect internal class data using getters and setters
- Recognize the role of auto-properties and computed properties
- Apply encapsulation to make their classes cleaner, safer, and easier to maintain

## Theory Summary

### What Is Encapsulation?

Encapsulation is one of the four core principles of OOP.
It means hiding the internal data and behavior of an object from the outside world, and exposing only what’s necessary through a public interface.

This allows:

- Controlled access to data
- Validation when setting values
- Flexibility to change implementation later without breaking external code

### Example of a poor encapsulation:
```csharp
public class Player
{
    public string Name;
    public int Health;
}

class Program
{
    static void Main()
    {
        Player p = new Player();
        p.Health = -100; // ❌ Invalid! Direct field access allows unsafe state
    }
}
```
Here, the player’s health can be set to anything, even negative values.

---

### Using Private Fields and Public Properties:
```csharp
public class Player
{
    private int health;
    public string Name { get; set; }

    public int Health
    {
        get => health;
        set
        {
            if (value < 0) health = 0;
            else health = value;
        }
    }
}
```

Now we control how `Health` is set — this is **encapsulation in action**.

---

### Auto-Properties

When no extra logic is needed, we can use **auto-properties**:
```csharp
public class Item
{
    public string Name { get; set; }
    public float Weight { get; set; }
}
```

C# automatically creates a hidden field behind the scenes.

---

### Read-Only and Write-Only Properties

```csharp
public class Weapon
{
    public string Name { get; }
    public int Damage { get; private set; }

    public Weapon(string name, int damage)
    {
        Name = name;
        Damage = damage;
    }
}
```

- `Name` can only be read (readonly property)
- `Damage` can only be changed inside the class (private set)

---

### Computed Properties

Properties can return calculated values:

```csharp
public class Rectangle
{
    public double Width { get; set; }
    public double Height { get; set; }

    public double Area => Width * Height;
}
```

No `set` method needed — the value is computed automatically.

## Example Code
```csharp
public class BankAccount
{
    private decimal balance;

    public string Owner { get; private set; }
    public decimal Balance => balance;

    public BankAccount(string owner, decimal initialBalance)
    {
        Owner = owner;
        balance = initialBalance;
    }

    public void Deposit(decimal amount)
    {
        if (amount <= 0)
        {
            Console.WriteLine("Deposit amount must be positive.");
            return;
        }
        balance += amount;
        Console.WriteLine($"Deposited {amount:C}. New balance: {balance:C}");
    }

    public void Withdraw(decimal amount)
    {
        if (amount > balance)
        {
            Console.WriteLine("Insufficient funds.");
            return;
        }
        balance -= amount;
        Console.WriteLine($"Withdrew {amount:C}. New balance: {balance:C}");
    }
}

class Program
{
    static void Main()
    {
        BankAccount account = new BankAccount("Alice", 1000m);
        account.Deposit(250m);
        account.Withdraw(400m);

        Console.WriteLine($"Account owner: {account.Owner}");
        Console.WriteLine($"Final balance: {account.Balance:C}");
    }
}
```

Output:
```sparksql
Deposited $250.00. New balance: $1,250.00
Withdrew $400.00. New balance: $850.00
Account owner: Alice
Final balance: $850.00
```

## Exercises

### Exercise 1 — Safe Player Stats

- Create a class `Player` with private fields:
  - `string name`
  - `int health`
  - `int level`
- Create properties for all three fields:
  - `Health` should **never** go below 0 or above 100.
- Add a method `TakeDamage(int amount)` that decreases health.
- Add a method `Heal(int amount)` that increases health safely.
- Test in `Main()` by creating a player and simulating damage and healing.

### Exercise 2 — Shop Item

- Create a class `ShopItem` with:
  - `Property Name`
  - `Property Price`
  - Read-only property `IsExpensive` (returns true if price > 100)
- Print the details of several `ShopItem`s in `Main()`.

### Exercise 3 — Encapsulated Car

- Create a class `Car` with:
  - `string Brand`
  - `float Fuel` (private field, 0–100 range)
- Add a property `Fuel` that clamps value between 0 and 100.
- Add a method `Drive(float amount)` that reduces fuel.
- Add a method `Refuel(float amount)` that increases fuel.
- In `Main()`, create a `Car`, drive it, and refuel it.