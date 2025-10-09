# 07 - Composition

## Learning Goals

By the end of this lesson, students should be able to:
- Understand the concept of composition in object-oriented programming
- Recognize the difference between composition ("has-a") and inheritance ("is-a")
- Implement composition by including instances of other classes within a class
- Appreciate the advantages of composition over inheritance in certain scenarios

## Theory Summary
### What Is Composition?

Composition is a design principle where a class contains instances of other classes that implement the desired functionality, rather than inheriting functionality from a parent class. In other words, it follows the “has-a” relationship rather than the “is-a” relationship that characterizes inheritance.

For example:

```csharp
public class Engine
{
    public void Start() 
    { 
        Console.WriteLine("Engine starting...");
    }
}

// .. + other classes like Wheel and Person

public class Car
{
    public Engine Engine;
    public Wheel[] Wheels;
    public Person Driver;

    public Car()
    {
        Engine = new Engine();
        // .. + other fields
    }

    public void Drive()
    {
        Engine.Start();
        Console.WriteLine("Car is driving...");
    }
}
```

Here, `Car` **has-a** `Engine`, `Wheels` and `Driver`, demonstrating composition.

### Composition vs. Inheritance

- Inheritance: Represents an "is-a" relationship. For example, a `Dog` is a `Animal`.
- Composition: Represents a "has-a" relationship. For example, a `Car` has a `Engine`.

While inheritance is suitable for establishing hierarchical relationships, composition provides greater flexibility and modularity by allowing objects to be composed of other objects.

## Example Code
```csharp
public class Engine
{
    public void Start() 
    { 
        Console.WriteLine("Engine starting...");
    }
}

public class Car
{
    public Engine Engine;

    public Car()
    {
        Engine = new Engine();
    }

    public void Drive()
    {
        Engine.Start();
        Console.WriteLine("Car is driving...");
    }
}

class Program
{
    static void Main()
    {
        Car myCar = new Car();
        myCar.Drive();
    }
}
```

```csharp
Engine starting...
Car is driving...
```

## Exercises

### Exercise 1 — Composition in Action

- Create a class `Person` with fields:
  - `string Name`
  - `int Age`
- Create a class `Address` with fields:
  - `string Street`
  - `string City`
  - `string Country`
- In the `Person` class, include an instance of `Address` to represent the person's address.
- In `Main()`, create a `Person` object and assign values to both `Person` and `Address` fields. Print the person's details.

### Exercise 2 — Composition with Methods

- Create a class `Book` with fields:
  - `string Title`
  - `string Author`
  - `int Year`
- Create a class `Publisher` with fields:
  - `string Name`
  - `string Country`
- In the `Book` class, include an instance of `Publisher` to represent the book's publisher.
- In `Main()`, create a Book object and assign values to both `Book` and `Publisher` fields. Print the book's details.

### Exercise 3 — Composition with Multiple Components

- Create a class `Computer` with fields:
  - `string Processor`
  - `int RAM`
  - `int Storage`
- Create classes `Monitor`, `Keyboard`, and `Mouse` with relevant fields.
- In the `Computer` class, include instances of `Monitor`, `Keyboard`, and `Mouse` to represent the computer's components.
- In `Main()`, create a `Computer` object and assign values to all fields. Print the computer's details.