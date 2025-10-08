# 01 - Classes And Objects

## Learning Goals
By the end of this lesson, students should be able to:

- Understand what a class is and how it relates to an object
- Be able to define a class with simple fields
- Create and use objects in the Main() method
- Learn the idea that each object has its own data

## Theory Summary

### What is a Class?

A **class** is a _blueprint_ or _template_ for creating objects.
It describes what data (fields) and actions (methods) objects of that class can have.
Right now we’ll only look at **fields**.

### What is an Object?

An **object** is an **instance** of a class — a real, individual example of it.
You create objects with the `new` keyword.

Each object stores its **own copy** of the fields defined by the class.

### Dot Notation

Once you have an object, you use the dot (`.`) to reach its members:
`objectName.fieldName`

---

## Example Code
```csharp
class Car
{
    public string brand;
    public string color;
    public int year;
}

class Program
{
    static void Main()
    {
        // Create first car
        Car myCar = new Car();
        myCar.brand = "Volvo";
        myCar.color = "Red";
        myCar.year = 2020;

        // Create second car
        Car yourCar = new Car();
        yourCar.brand = "Toyota";
        yourCar.color = "Blue";
        yourCar.year = 2018;

        Console.WriteLine($"My car is a {myCar.color} {myCar.brand} from {myCar.year}.");
        Console.WriteLine($"Your car is a {yourCar.color} {yourCar.brand} from {yourCar.year}.");
    }
}
```
Output:

```csharp
My car is a Red Volvo from 2020.
Your car is a Blue Toyota from 2018.
```

## Exercises
 
### Exercise 1 — Create Your Own Class

- Create a class called `Book`.
- Give it these public fields:
  - `string title`
  - `string author`
  - `int pages`
- In `Main()`, create two `Book` objects with different data and print:
  - `"Title: <title>, Author: <author>, Pages: <pages>"`

### Exercise 2 — Multiple Objects

- Create a class called `Student` with public fields:
  - `string name`
  - `int age`
  - `string favoriteSubject`
- In `Main()`, create **three** students and print their info.