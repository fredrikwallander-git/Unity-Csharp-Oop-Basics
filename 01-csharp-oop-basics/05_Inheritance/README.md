# 05 - Inheritance

## Learning Goals

By the end of this lesson, students should be able to:
- Understand the concept of inheritance in object-oriented programming
- Define a base (parent) class and one or more derived (child) classes
- Use : baseClassName syntax in C#
- Override or extend behavior in child classes
- Use the virtual, override, and optionally sealed keywords
- Understand how inheritance affects object type and polymorphism basics

## Theory Summary

### What Is Inheritance?

Inheritance allows one class (child/derived) to **reuse** code from another class (base/parent).
The child class inherits fields and methods from the parent.

For example:
```
Animal  ←– Parent / Base class  
↳ Dog, Cat ←– Derived / Child classes 
```

`Dog` and `Cat` automatically get what `Animal` defines (unless overridden or extended).

### Syntax in C#

```csharp
class BaseClass { ... }
class DerivedClass : BaseClass { ... }
```

### Extending vs Overriding

- **Extend**: add new fields or methods in the child class
- **Override**: replace (or refine) behavior of a method defined in the parent

To allow overriding, parent methods should be marked `virtual` (or `abstract`).
Child classes override with `override` keyword.

Example:
```csharp
class Animal
{
    public virtual void Speak()
    {
        Console.WriteLine("Some generic animal sound");
    }
}

class Dog : Animal
{
    public override void Speak()
    {
        Console.WriteLine("Woof!");
    }
}
```

**Using** `base` inside an overriding method, you can call `base.Method()` to invoke the parent version:

```csharp
public override void Speak()
{
    base.Speak();  // calls Animal.Speak()
    Console.WriteLine("and now Dog says Woof!");
}
```

### Polymorphism (Intro)

Because `Dog` is an `Animal`, you can treat a `Dog` instance as an `Animal` reference:

```csharp
Animal a = new Dog();
a.Speak();  // calls Dog’s override
```
This concept leads into polymorphism, which you’ll cover in the next lesson(s).

## Example Code
```csharp
class Animal
{
    public string Name;

    public Animal(string name)
    {
        Name = name;
    }

    public virtual void Speak()
    {
        Console.WriteLine($"{Name} makes a sound.");
    }

    public virtual void Describe()
    {
        Console.WriteLine($"I am an animal named {Name}.");
    }
}

class Dog : Animal
{
    public string Breed;

    public Dog(string name, string breed) : base(name)
    {
        Breed = breed;
    }

    public override void Speak()
    {
        Console.WriteLine($"{Name} says: Woof!");
    }

    public override void Describe()
    {
        base.Describe();  // call the base implementation
        Console.WriteLine($"I am a {Breed} dog.");
    }

    public void WagTail()
    {
        Console.WriteLine($"{Name} wags its tail!");
    }
}

class Cat : Animal
{
    public Cat(string name) : base(name) { }

    public override void Speak()
    {
        Console.WriteLine($"{Name} says: Meow!");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Dog dog = new Dog("Rex", "German Shepherd");
        Cat cat = new Cat("Whiskers");

        dog.Describe();
        dog.Speak();
        dog.WagTail();

        cat.Describe();
        cat.Speak();

        Console.WriteLine();

        Animal a1 = dog;
        Animal a2 = cat;
        a1.Speak();  // uses Dog’s override
        a2.Speak();  // uses Cat’s override
    }
}
```

Expected output:
```less
I am an animal named Rex.
I am a German Sheperd dog.
Rex says: Woof!
Rex wags its tail!
I am an animal named Whiskers.
Whiskers says: Meow!

Rex says: Woof!
Whiskers says: Meow!
```

## Exercises

### Exercise 1 — Simple Inheritance

- Create a base class `Vehicle` with fields:
  - `string brand`
  - `int speed`
  - And a method:
    - `public virtual void ShowInfo()` → prints brand and speed
- Create a child class `Car` that inherits `Vehicle`:
  - Add field `string model`
  - Override `ShowInfo()` to also print the model
- In `Main()`, instantiate a `Car` object and call its `ShowInfo()`.

### Exercise 2 — Multiple Derived Types

- Using the `Vehicle` base, add two more derived classes:
  - `Motorcycle`
  - `Truck`
- Each should have additional fields or behavior. For example:
  - `Motorcycle: bool hasSidecar`
  - `Truck: double cargoCapacity`
- Override methods as needed. In `Main()`, create one of each and call `ShowInfo()`.

### Exercise 3 — Polymorphism

- Create a `Vehicle[]` and add various `Car`, `Motorcycle`, `Truck` objects to it.
- Iterate over the array and call `ShowInfo()` on each element.
  - Observe how the overridden methods run correctly.
- Add a method in `Vehicle` called `public virtual void Start()` (e.g. prints “Vehicle starting…”).
  - Override it in child classes with more specific text.
  - Use the same loop to call `Start()` on each.
