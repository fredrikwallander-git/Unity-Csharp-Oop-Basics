# Classes

`Note`: We will use the `public` keyword a lot in the coming chapters.
Its basic meaning is that what you are about to define can be seen and accessed outside of the current scope.

## Class definition

```
public class Player {

}

public class Camera {

}

public class Item {

}
```

## Types

Classes are `Data Types` and can be used as:
- Variables
- Method Parameter Types or
- As Method return types

```
Player player; // Variable

public void Follow(Camera camera) { // Method param
}

public Item BuyItem() { // Method return type
}
```

## Objects

When you use a class as a variable's Data Type, the variable can be used to store Objects, which are class instances.
### Null

They are null per default, which means, they don't exist. More on this topic later in `#4 The Heap`.

``Player player = default; // null``

## Object Instantiation

They can be instantiated using the `new` keyword:

```
public Item BuyItem() { // Method return type
    return new Item();
}
```

We will learn the details of this in the future chapters, but when it comes to Unity and component scripts we do not have to create a new instance as this is handled automatically for us when we `inherit` from a `MonoBehaviour`.