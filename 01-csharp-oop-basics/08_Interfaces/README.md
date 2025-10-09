# 08 - Interfaces

## Learning Goals

By the end of this lesson, students should be able to:
- Understand what an interface is in C#
- Know the difference between interfaces and abstract/base classes
- Implement an interface in one or more classes
- Use interface references to access objects
- Recognize how interfaces promote polymorphism and decoupling

## Theory Summary
### What Is an Interface?

An **interface** is a contract that defines **what methods or properties a class must implement**, without providing the implementation itself.

Syntax:

```csharp
interface IShape
{
    double Area();
    double Perimeter();
}
```

- A class that implements an interface must **define all methods** declared in the interface.
- Interfaces allow **multiple unrelated classes** to implement the same behavior.

### Interface vs Base Class

| Feature                    | Interface                                | Base Class                    |
| -------------------------- | ---------------------------------------- | ----------------------------- |
| Can contain implementation | No (C# 8+ allows default implementation) | Yes                           |
| Multiple inheritance       | Yes, can implement multiple interfaces   | No, only single inheritance   |
| Purpose                    | Contract / behavior                      | Shared implementation / state |


### Implementing an Interface

```csharp
interface IMovable
{
    void Move();
}

class Car : IMovable
{
    public void Move()
    {
        Console.WriteLine("Car is moving forward!");
    }
}

class Robot : IMovable
{
    public void Move()
    {
        Console.WriteLine("Robot is walking forward!");
    }
}
```

`Car` and `Robot` both implement `IMovable`, but each provides its own behavior.

### Using Interface References

```csharp
IMovable obj = new Car();
obj.Move();  // Car’s implementation

obj = new Robot();
obj.Move();  // Robot’s implementation
```

- Similar to polymorphism with inheritance, but the classes **don’t need to share a common parent**.
- Useful for decoupling: code can operate on the interface without knowing the concrete type.

## Example Code
```csharp
interface IFlyable
{
    void Fly();
}

class Bird : IFlyable
{
    public string Name;
    public Bird(string name) { Name = name; }
    public void Fly() => Console.WriteLine($"{Name} is flying!");
}

class Airplane : IFlyable
{
    public string Model;
    public Airplane(string model) { Model = model; }
    public void Fly() => Console.WriteLine($"Airplane {Model} is taking off!");
}

class Program
{
    static void Main()
    {
        List<IFlyable> flyingObjects = new List<IFlyable>();
        flyingObjects.Add(new Bird("Eagle"));
        flyingObjects.Add(new Airplane("Boeing 747"));

        foreach (IFlyable obj in flyingObjects)
        {
            obj.Fly();  // Calls the correct implementation for each object
        }
    }
}
```

Output:
```csharp
Eagle is flying!
Airplane Boeing 747 is taking off!
```

## Exercises

### Exercise 1 — Single Interface

- Create an interface `ISwimmable` with a method `void Swim()`.
- Create a class `Fish` that implement `ISwimmable`.
- Instantiate an object of `Fish` and call `Swim()`.

### Exercise 2 — Interface with Multiple Implementers

- Create an interface `IDrawable` with method `void Draw()`.
- Implement it in at least **three** classes: `Circle`, `Rectangle`, `Triangle`.
- Store objects in a `List<IDrawable>` and call `Draw()` on each.
