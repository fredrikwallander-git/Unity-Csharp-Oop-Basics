# Fields

Fields allow you to define the data that your class instances will contain. It basically works like a variable that belongs to each class instance.
Definition

## Definition

You define the field similar to a variable, but within the Scope { } of a class and using the public-Keyword (otherwise, you won't be able to access it in the next step):

```
public class Player {
    public int Health;
    public string Name;
}
```

In above example, every Player will have:

- an Integer health variable (Field)
- a Text name variable (Field)

## Assignment

To assign a value to a class Field you need:

- an object (class instance) reference
- to access the Member using the . (member-of) operator

```
Player first = new Player();
first.Health = 50;
first.Name = "Marc";
```

Each class instance will have its own individual values:

```
Player first = new Player();
Player second = new Player();
first.Name="Red";
second.Name="Blue";
```

## Access

To access a value to a class Field you need:

- an object (class instance) reference
- to access the Member using the . (member-of) operator

``Console.WriteLine(first.Health);``

## Initialization?

Class Members are initialized to their default values automatically, there is no need to initialize them manually like with variables:

```
public class Player {
    public int Health; // 0
    public bool WantsToQuit; // false (0)
    public float LevelBonus; // 0f (0)
    public char Initial; // '\0' (0)
    public string Name; // null (0)
}
```

It's valid to access these members without explicitly initializing them:

```
Player first = new PLayer();
Console.WriteLine(first.Health);
```

But that's not possible with regular variables:

```
int health;
Console.WriteLine(health); // ERROR
```

## Null Reference Exception

You can't access the members of "nothing":

```
Player first = null;
Console.WriteLine(first.Health); // NullReferenceException
```

## Null Check

You can check, whether a value is null before accessing a Member:

```
if(first != null) {
    Console.WriteLine(first.Health);
}
```

- BUT ONLY DO THIS IF YOU THINK THAT IT CAN BE null UNDER NORMAL CONDITIONS
  - e.g. if the Player sometimes carries an Item and sometimes not
  - or if an NPC might have a target to walk to and sometimes not
- DO NOT USE IT EVERYWHERE TO HIDE ERRORS
  - i.e. do not always put `if(x != null)` every time you find a NullReferenceException. There might be another underlying issue that you could be hiding.
  - Imagine for example, every Client (Player) is supposed to have a Character that he can move. If you just ignore if there is no movable Character (``if(playerGameObject != null){HandleInput(playerGameObject);}``), you might be hiding an underlying issue where players can't play without any error reporting.

## Unity

In Unity, variables work a little bit different when it comes to `MonoBehaviour` classes.

Unity initializes our variables per default with their default values:
- public int Health; // has a default value of 0
- public bool IsOpen; // has a default value of false

### Private

Only private members of a class needs to be initialized even if you `inherit` from a `MonoBehaviour` class.
- int damage = 10; // have to be initialized otherwise it wil throw an error when used.