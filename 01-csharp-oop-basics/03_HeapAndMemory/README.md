# 03 - The Heap & Memory Basics

## Learning Goals
By the end of this lesson, students should be able to:

- Distinguish between **stack** and **heap** in memory (at a high level)
- Know where **objects** are stored in memory
- Understand **references** (variables pointing to object instances)
- Grasp why two reference variables might refer to the **same** object

## Theory Summary
### Stack vs. Heap (**High-Level**)

- The `stack` stores local variables and method call frames (value types, references)
- The `heap` stores objects/instances created with `new`

When you write:

```csharp
Car carA = new Car();
```

- `carA` (the reference) lives on the **stack**, pointing to
- The actual `Car` object, which lives in the **heap**

### References & Aliasing

- A `reference` is like an address or pointer to the object in the heap
- If two references point to the same object, changes through one reference are visible via the other

```csharp
Car carA = new Car();
Car carB = carA;
carB.brand = "Ferrari";
Console.WriteLine(carA.brand);  // “Ferrari” — since both refer to the same object
```

### Value Types vs Reference Types

- **Value types** (e.g. `int`, `double`, `struct`) store the actual data (on stack or inline)
- **Reference types** (classes, arrays, strings) store a reference on stack; actual data lives on heap

So:
```csharp
int x = 5;
int y = x;
y = 10; // x is still 5

Car a = new Car();
Car b = a;
b.brand = "Changed"; // a.brand also "Changed"
```

### Garbage Collection (Very Brief)

- .NET automatically reclaims heap memory when objects are no longer referenced
- You generally don’t manually free memory in C#

## Example Code
```csharp
class Car
{
    public string brand;
}

class Program
{
    static void Main()
    {
        Car carA = new Car();
        carA.brand = "Volvo";

        Car carB = carA;       // carB references the same object
        carB.brand = "BMW";

        Console.WriteLine(carA.brand);  // BMW
        Console.WriteLine(object.ReferenceEquals(carA, carB));  // True

        // Value type example
        int x = 5;
        int y = x;
        y = 10;
        Console.WriteLine($"x = {x}, y = {y}");  // x = 5, y = 10
    }
}
```

Output:
```genericsql
BMW
True
x = 5, y = 10
```

## Exercises
### Exercise 1 — Same Object?

- Create a class `Person` with a public `string name` field.
- In `Main()`:
  - `Person p1 = new Person();`
  - `p1.name = "Alice";`
  - `Person p2 = p1;`
  - `p2.name = "Bob";`
  - Print both `p1.name` and `p2.name`.
  - Print whether `p1 == p2` (or `ReferenceEquals(p1, p2)`).

### Exercise 2 — Separate Objects

- Create two separate Person objects (p3 and p4), each with their own name.
- Modify p3.name, then print both names — verify they don’t affect each other.