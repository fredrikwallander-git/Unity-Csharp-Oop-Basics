# Exercises

## People

This exercise asks you to define a Method that reads a Value from its Field.

### Instructions

- Create or re-use the Unity project from before
- Create a Person Class
- Give it a Field for their `Name`
- Give it a Method named `Greeting()`
   - It should print "Hello, I'm Fred!" (or whatever their name is) to the Console
- Create a People class and let it inherit from MonoBehaviour 
- Add an Array field called People and give it a size of 3
- Create three People and add three random names in the `Start()` method to the array using the .member-of access to the Name field
- After that invoke `Greeting()` on all People
- Make sure to add the People class to a game object and hit play

## OpenClose

This exercise asks you to define a Method that changes a class Field.

### Instructions

- Create or re-use the Unity project from before
- Create a House Class
  - It has a door that can be open or closed (bool)
  - It has a method OpenDoor that opens the door
  - It has a method CloseDoor that closes the door
- Create a Street `MonoBehaviour` class and 2 house fields
- In the `Start()`method create two houses blueHouse and redHouse
  - Print for both houses whether the door is open (should be false and false)
- Open the Door at blueHouse and redHouse
  - Print for both houses whether the door is open (should be true and true)
- close the Door at redHouse
  - Print for both houses whether the door is open (should be true and false)

## Level Up

````
Output:0 Level and 0 Experience.
Input:99
Output:0 Level and 99 Experience.
Input: 5
Output: 1 Level and 4 Experience.
Input: 96
Output: 2 Level and 0 Experience.
Input: 301
Output: 5 level and 1 Experience.
````

### Instructions

- For this, create a console project named LevelUp
- Define a class named Player
- Define a Method named GrantExperience
   - It takes an argument that defines how much Experience the player is supposed to get.
   - It adds that experience to the Player's Experience.
   - Every time, the Player has 100 Experience or more, he levels up.
   - He keeps his extra experience.
   - He can theoretically level up multiple Levels at once.
- Create an instance of class Player
- Allow the user repeatedly to define, how much Experience he wants to grant the Player and use the GrantExperience Method to grant it.

Later we will take a look at how to use buttons and bind methods to the buttons inside Unity.