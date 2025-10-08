# 02 - Methods And Fields

## Learning Goals

By the end of this lesson, students should be able to:
- Define fields (already introduced) and methods in a class
- Understand method signature: return type, name, parameters, body
- Call methods on an object from Main()
- Learn how methods can read/write fields of their own object

## Theory Summary

### Fields (**recap**)
- Fields are variables declared inside a class to hold data about each instance.
- Each object (instance) has its own copy of non-static fields.

### Methods
- A method is a function defined inside a class that describes some behavior or action.
- A method may:
  - Take **parameters** (zero or more)
  - Return a value (or be `void` if it returns nothing)
  - Use the object’s fields (read or modify them)

A method signature looks like:

```scss
[access_modifier] returnType MethodName(parameters) {
    // method body
}
```

Example: `public void Drive(int increment)`
- `public` → accessible from outside
- `void` → does not return a value
- `Drive` → method name
- `(int increment)` → parameter list

Methods are invoked with dot notation: `myCar.Drive(10)`

Method & Field Interaction
- Inside a method, you can read or write the object’s fields (e.g. `this.speed += increment`).
- Use `this` optionally to refer to the current instance (e.g. `this.speed = 5`) — though it’s often optional when there is no naming conflict.

## Example Code
```csharp
class Car
{
    public string brand;
    public int speed;

    // Method to increase speed
    public void Drive(int increment)
    {
        speed += increment;
        Console.WriteLine($"{brand} drives at {speed} km/h.");
    }

    // Method to apply brakes
    public void Brake(int decrement)
    {
        speed -= decrement;
        if (speed < 0)
            speed = 0;
        Console.WriteLine($"{brand} slows down to {speed} km/h.");
    }

    // Method that returns a value
    public bool IsMoving()
    {
        return speed > 0;
    }
}

class Program
{
    static void Main(string[] args)
    {
        Car car = new Car();
        car.brand = "Honda";
        car.speed = 0;

        car.Drive(30);    // Honda drives at 30 km/h.
        car.Drive(20);    // Honda drives at 50 km/h.
        car.Brake(15);    // Honda slows down to 35 km/h.

        bool moving = car.IsMoving();
        Console.WriteLine($"Is the car moving? {moving}");  // True

        car.Brake(40);    // Honda slows down to 0 km/h.
        Console.WriteLine($"Is the car moving now? {car.IsMoving()}");  // False
    }
}
```

What’s happening here?

- `Drive(int)` and `Brake(int)` are **void methods**: they do not return anything, but perform actions (modify fields).
- `IsMoving()` is a non-void method: it returns a boolean (`true` or `false`).
- Methods use the object’s `speed` and `brand` fields to perform logic and output text.

## Exercises

Exercise 1 — Simple Methods

- Create a `Lamp` class with fields:
  - `public bool isOn`
- Add these methods:
  - `public void TurnOn()`→ sets `isOn = true` and prints `"Lamp is ON."`
  - `public void TurnOff()` → sets `isOn = false` and prints `"Lamp is OFF."`
  - `public void Toggle()`→ flips the state (`isOn = !isOn`) and prints the new state.
- In `Main()`, test it by creating a `Lamp` object, calling `TurnOn()`, `Toggle()`, `Toggle()`, `TurnOff()`.

Exercise 2 — Methods with Return Values

- Create a `Rectangle` class with fields:
  - `public double width`
  - `public double height`
- Add methods:
  - `public double Area()` → returns area (`width * height`)
  - `public double Perimeter()` → returns perimeter (`2 * (width + height)`)
- In `Main()`, create one rectangle, assign width/height, print area and perimeter.

Exercise 3 — Cumulative Behavior

- Create a `BankAccount` class with fields:
  - `public double balance`
- Add methods:
  - `public void Deposit(double amount)` → adds `amount` to `balance`
  - `public bool Withdraw(double amount)` → if `amount <= balance`, subtract it and return `true`, otherwise print “Insufficient funds” and return `false`
  - `public string GetBalanceInfo()` → returns e.g. `"Current balance: <balance>"`
- In Main(), simulate a sequence:
  - Deposit 100
  - Withdraw 25
  - Withdraw 90
  - Print final balance via `GetBalanceInfo()`