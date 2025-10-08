# 04 - Constructors

## Learning Goals
By the end of this lesson, students should be able to:

- Understand what a **constructor** is and when it is called
- Know the syntax for declaring constructors in C#
- Use **default**, **parameterized**, and **overloaded** constructors
- Understand the relationship between **fields**, **constructors**, and **object initialization**
- Differentiate between using constructors and assigning fields manually

## Theory Summary
### What Is a Constructor?

A **constructor** is a special method that runs automatically when an object is created using the `new` keyword. 

Its job is to **initialize** the object - set up its initial state.

```csharp
Car myCar = new Car();
```

When this line runs, C# automatically calls the constructor of the `Car` class.

### Constructor Rules

- Has the **same name as the class**
- Has **no** return type (not even `void`)
- Can have **parameters** (to set values when creating the object)
- You can **overload** constructors (have more than one with different parameters)

### Default Constructor

If you don’t write any constructor yourself, C# automatically provides a default constructor that sets all fields to default values (`0`, `null`, `false`, etc.).

## Example Code
```csharp
class Car
{
    public string brand;
    public int speed;

    // Default constructor
    public Car()
    {
        brand = "Unknown";
        speed = 0;
        Console.WriteLine("Car created with default values.");
    }

    // Parameterized constructor
    public Car(string carBrand, int startSpeed)
    {
        brand = carBrand;
        speed = startSpeed;
        Console.WriteLine($"Car created: {brand} with speed {speed} km/h.");
    }

    public void Drive()
    {
        Console.WriteLine($"{brand} is driving at {speed} km/h.");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Car car1 = new Car(); // Uses default constructor
        car1.Drive();

        Car car2 = new Car("Toyota", 50); // Uses parameterized constructor
        car2.Drive();

        Car car3 = new Car("Ferrari", 120);
        car3.Drive();
    }
}
```

```genericsql
Car created with default values.
Unknown is driving at 0 km/h.
Car created: Toyota with speed 50 km/h.
Toyota is driving at 50 km/h.
Car created: Ferrari with speed 120 km/h.
Ferrari is driving at 120 km/h.
```

### Why Use Constructors?

- Ensures every object is initialized properly
- Makes your code cleaner — instead of setting fields after creation:

```csharp
Car c = new Car();
c.brand = "Volvo";
c.speed = 60;
```
- you can write:

```csharp
Car c = new Car("Volvo", 60);
```

### Constructor Overloading

You can have multiple constructors to provide flexibility:

```csharp
public Car() { ... }
public Car(string brand) { ... }
public Car(string brand, int speed) { ... }
```

C# picks the correct one based on the arguments you pass when using **new**.

### Best Practices

- Keep constructors simple — avoid long logic
- Initialize only what’s needed
- Use constructor chaining if multiple constructors share logic: ( more on this later )

```csharp
public Car() : this("Unknown", 0)
{
    // Delegates work to another constructor
}
```

## Exercises
### Exercise 1 — Default Constructor

Create a class `Animal` with fields:
- `string species`
- `int age`

Give it a **default constructor** that prints `"A new animal has been created!"`

Then, in `Main()`, create an instance of `Animal` and print its default field values.

### Exercise 2 — Parameterized Constructor

Expand the `Animal` class:

- Add a constructor that takes `species` and `age` as **parameters**
- Print `"Created a <species>, age <age>"` inside the constructor
- Create two different animals in `Main()` and print their data

### Exercise 3 — Constructor Overloading

Create a class `Book` with fields:

- `string title`
- `string author`
- `int year`

Add:
- Default constructor → sets placeholders `"Unknown Title"`, `"Unknown Author"`, `0`
- Constructor with `title` and `author` only
- Constructor with **all three parameters**
- In `Main()`, create and print three books using different constructors
