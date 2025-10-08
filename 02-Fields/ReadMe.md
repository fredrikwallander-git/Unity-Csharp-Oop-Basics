# Exercise

## Item Values

### Goal

````
Output:100
Output:200
Output:300
````
### Instructions

- Create or re-use the same project from 01
- Create a `MonoBehaviour` class named Item (easiest it to use the create menu in Unity)
  - Add a field of type int named goldValue
- Create an Array containing three Items
- Assign the value 100 to the first Item's goldValue, 200 to the second's goldValue and 300 to the third's. 
- In the `Start()` method, print the value of each Item's goldValue to the Console
- Don't forget to add the script as a component to your game object

Use `Debug.Log("Your Message Here");` to print out any information to the console.

## Player Gold

### Goal
````
Output:200
````
### Instructions

- Create a new project or re-use the one from 01
- Create a class named Player ( Do NOT inherit from `MonoBehaviour` in this one )
   - Add a public field of type int named gold
   - Add [Serializable] on top of the class in order for Unity to be able to display it in the inspector.

````
[Serializable]
public class Player { 

}
````

- Create a class named PlayerController ( This should inherit from `MonoBehaviour` ) 
- Define a private variable of type Player in the `PlayerController`
- Create and assign a new instance of Type Player to the variable in the `Start()` method
- After creation, assign the value 200 to the field gold of that Player instance
- Print the value of field gold of that Player instance to the Console.

## All information please

### Goal

- You are designing a Player Class to contain all Player information of an RTS:

>It's an RTS Game in which the player can collect Gold, Stone and Wood as Resources. The player has a level and experience and can have an active or inactive VIP Subscription as well as PVP enabled or disabled. The place can configure his display name and enter his email address.

- This is an example of a player:

>Player A has 200 Gold, 110 Stone, 53 Wood. He is level 12 and has 123 Experience. He has an active VIP subscription, but PVP disabled. His display name is Fredrik and his e-mail address fredrik@forsbergsskola.se

### Instructions

- Create a new project or re-use the one from 01
- Design a Class that contains all information needed for the above player.
- Assign the information to a new instance of said class or in the `Start()` method if you used a `MonoBehaviour`
- Print all information of that class to the console.

Here you can use either a c# class with or without inheriting from `MonoBehaviour` but, take note that without `MonoBehaviour` you cannot use this class as a custom component on a game object.

Option A: Use `MonoBehaviour` and the `Start()` method to initialize all the information or..
Option B: Create a `MonoBehaviour` "parent" class and a non `MonoBehaviour` class and use it in the "parent" class.
