# 01 - Unity Setup And Interface

## Learning Goals

By the end of this lesson, students will be able to:
- Understand what Unity is and what it’s used for
- Install and configure Unity Hub and the Unity Editor
- Create and organize a Unity project
- Navigate the Unity interface (Scene, Game, Hierarchy, Inspector, Project, Console)
- Place, move, rotate, and scale GameObjects in a scene
- Understand the relationship between GameObjects and Components

## Theory Summary
### What Is Unity?

Unity is a **game engine and editor** that allows you to create 2D, 3D, VR, and AR experiences.
It combines tools for design, scripting, and rendering, making it ideal for both beginners and professionals.

### Unity Hub & Editor

- **Unity Hub**: A launcher for managing Unity installations, projects, and versions.
- **Unity Editor**: The actual environment where you create games.

### Installation & Setup

### Download Unity Hub: 
https://unity.com/download

### Install a Unity version:
- Use the **LTS** (**Long-Term Support**) version — e.g. Unity 6 (6000.x LTS)
- Add the **Editor module**, and optionally:
  - Windows Build Support (IL2CPP)
  - Mac Build Support
  - WebGL Build Support

### Create a new project:
- Open Unity Hub → “New Project”
- Select template: **2D** or **3D** (**URP**)
- Name: `MyFirstUnityProject`
- Location: Choose a folder (no spaces or special characters)

### Unity Interface Overview

When the project opens, you’ll see several key panels:

| Panel              | Description                                                                        |
| ------------------ | ---------------------------------------------------------------------------------- |
| **Scene View**     | Where you build your level or scene. You can move, rotate, and place objects here. |
| **Game View**      | Shows what the player will see when the game is running.                           |
| **Hierarchy**      | Lists all GameObjects in the active scene.                                         |
| **Inspector**      | Displays details and components of the selected GameObject.                        |
| **Project Window** | Shows all assets (scripts, textures, prefabs, sounds, etc.).                       |
| **Console**        | Displays messages, warnings, and errors from your code.                            |

### Scene Navigation Shortcuts

| Action          | Shortcut            |
| --------------- | ------------------- |
| Move camera     | Right-click + WASD  |
| Zoom            | Scroll wheel        |
| Focus on object | Select object + `F` |
| Move object     | `W`                 |
| Rotate object   | `E`                 |
| Scale object    | `R`                 |

### Understanding GameObjects & Components

In Unity:

- **Everything in a scene is a GameObject** (e.g. Camera, Light, Cube, Player).
- **Components** define what the object does (e.g. Transform, Collider, Script).

Example:

| GameObject | Components                                                         |
| ---------- | ------------------------------------------------------------------ |
| Cube       | Transform, Mesh Renderer, Box Collider                             |
| Light      | Transform, Light                                                   |
| Player     | Transform, Sprite Renderer, Rigidbody2D, PlayerController (Script) |

This system is called composition over inheritance, where behavior is built by adding modular pieces.

## Exercises

### Exercise 1 — Setting Up the First Scene

Goal: Create a simple scene using primitives.

- Open Unity → Create new 3D project
- In the Hierarchy, right-click → 3D Object → Cube
- Move the cube (`W`), rotate (`E`), and scale (`R`)
- Create a Plane (floor) and a Light (Directional Light)
- Adjust the light angle to create nice shadows
- Rename GameObjects properly (e.g., `Ground`, `PlayerCube`)
- Save the scene as `MainScene.unity` in `Assets/Scenes/`
 
### Exercise 2 — Adding a Simple Script

- In the Project Window, create a folder named Scripts
- Create a new C# script: RotateObject.cs
- Attach it to the cube by dragging it into the Inspector

```csharp
using UnityEngine;

public class RotateObject : MonoBehaviour
{
    public float rotationSpeed = 45f;

    void Update()
    {
        transform.Rotate(0, rotationSpeed * Time.deltaTime, 0);
    }
}
```
- Press ▶️ (Play) — your cube should rotate!

---

## Folder Structure Recommendation
```
Assets/
┣ Scenes/
┣ Scripts/
┣ Materials/
┣ Prefabs/
┗ Textures/
```