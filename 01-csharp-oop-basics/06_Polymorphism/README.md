# 06 - Polymorphism

## Learning Goals

By the end of this lesson, students should be able to:
- Understand what **polymorphism** is in C#
- Treat objects of derived classes as instances of their **base class**
- Use **virtual** and **override** methods to achieve polymorphic behavior
- Observe **dynamic method dispatch** at runtime
- Apply polymorphism in **collections of base class objects**

## Theory Summary
### What is Polymorphism?

Polymorphism literally means “**many forms**”.
In object-oriented programming, it allows you to **use a single interface or base type** to work with multiple derived types.

Example:

```csharp
Animal myAnimal = new Dog();
myAnimal.Speak();  // calls Dog’s override, not Animal’s
```

- The variable type is `Animal`
- The actual object is `Dog`
- The runtime decides which `Speak()` method to execute → this is **dynamic dispatch**

### Why use Polymorphism?

- Write flexible, reusable code
- Treat a group of objects in a uniform way
- Avoid repetitive `if/switch` statements for type-specific behavior

## Example Code
```csharp
class Animal
{
    public string Name;
    public Animal(string name) { Name = name; }

    public virtual void Speak()
    {
        Console.WriteLine($"{Name} makes a generic sound.");
    }
}

class Dog : Animal
{
    public Dog(string name) : base(name) { }

    public override void Speak()
    {
        Console.WriteLine($"{Name} says: Woof!");
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
    static void Main()
    {
        List<Animal> animals = new List<Animal>();
        animals.Add(new Dog("Rex"));
        animals.Add(new Cat("Whiskers"));
        animals.Add(new Dog("Buddy"));

        foreach (Animal a in animals)
        {
            a.Speak();  // Polymorphic behavior: each object calls its own override
        }
    }
}
```

```yaml
Rex says: Woof!
Whiskers says: Meow!
Buddy says: Woof!
```

Side note: `Arrays` have a fixed size that is set when they are created, while `lists` however, can grow or shrink dynamically as items are added or removed.
With a list you can use the `foreach` loop to iterate over the objects:

```csharp
// syntax
foreach (type variableName in itemName)
{
  // code block to be executed
}

// example
List<Car> cars = new List<Car> 
{
    new Car("Volvo"), new Car("Mazda"), new Car("Honda")
};

foreach (Car car in cars) 
{
  Console.WriteLine(car.Model);
}

* foreach also works with arrays
```

- Lists provide a number of methods to operate with such as
  - .Add()
  - .Remove()
  - .IndexOf()
  - .Reverse()

Read more [here](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1?view=net-9.0) about lists.

## Exercises

### Exercise 1 — Simple Polymorphism

- Use the `Vehicle` base class from the Inheritance lesson.
- Create at least three derived classes (`Car`, `Motorcycle`, `Truck`) and override `Start()` method.
- Make a `List<Vehicle>` containing one of each derived object.
- Iterate over the list and call `Start()`. Observe which method executes for each object.

### Exercise 2 — Polymorphism with Methods

- Create a base class `Shape` with:
  - `public virtual double Area()` → returns 0
  - `public virtual double Perimeter()` → returns 0
- Create two derived classes: `Rectangle` and `Circle`
  - Override `Area()` and `Perimeter()` appropriately
- Create a `List<Shape>` with several rectangles and circles, then iterate and print area & perimeter.

### Exercise 3 — Dynamic Assignment

- Create base class `Employee` with `public virtual void Work()`
- Create derived classes `Developer` and `Designer` that override `Work()`
- Assign `Employee e = new Developer()` and `Employee e2 = new Designer()`
- Call `Work()` on both variables — verify the correct override runs