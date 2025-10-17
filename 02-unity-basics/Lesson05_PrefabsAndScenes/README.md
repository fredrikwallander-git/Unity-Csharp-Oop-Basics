# 05 - Prefabs And Scenes

## Learning Goals

By the end of this lesson, students will:

- Understand what **prefabs** are and why they’re essential for reusability.
- Learn how to **instantiate** and modify prefabs at runtime.
- Understand how **scenes** work as levels or contexts.
- Learn how to **save**, **load**, and **switch** scenes in Unity.

## Theory Summary

### What Are Prefabs?
A **Prefab** is a **saved template** of a GameObject, sort of like a blueprint.
When you make a prefab, Unity stores all of that object’s components, hierarchies, and values so you can reuse it easily.

You can think of a prefab as:
>"A reusable GameObject asset that you can place and instantiate multiple times."

---

### Creating a Prefab

1. Create a GameObject (e.g., a cube or player).
2. Drag it from the **Hierarchy** into the Project window.
3. Unity turns it blue — it’s now a prefab!
4. You can place multiple instances in the scene.

If you change the original prefab, **all instances** update automatically.

---

### Prefab Variants

Prefab variants let you:

- Create a **base prefab** (e.g., `EnemyBase`).
- Derive **variants** (e.g., `EnemyFast`, `EnemyTank`).
- Each variant can override some values without losing the link to the base.

---

## Example: Instantiating Prefabs at Runtime
```csharp
using UnityEngine;

public class Spawner : MonoBehaviour
{
    public GameObject enemyPrefab; // Drag prefab here in Inspector

    void Start()
    {
        // Spawn 5 enemies in a row
        for (int i = 0; i < 5; i++)
        {
            Vector3 spawnPos = new Vector3(i * 2, 0, 0);
            Instantiate(enemyPrefab, spawnPos, Quaternion.identity);
        }
    }
}
```

Note: `Instantiate()` creates a clone of the prefab at runtime.

It’s great for spawning bullets, enemies, collectibles, or effects.

---

## Scenes in Unity

A **Scene** in Unity is like a level or area in your game.
Each scene contains its own hierarchy of GameObjects.

You can think of it as:

>"A self-contained environment that represents one part of your game."

Examples:

- `MainMenu.unity`
- `Level1.unity`
- `BossArena.unity`

--- 

### Saving and Loading Scenes

To **save** a scene:

`File → Save Scene or Ctrl + S`

To **load** a scene manually:

`File → Open Scene`

You can also load scenes at **runtime** using the Scene Management API.

---

## Example: Loading Scenes in Code
```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class SceneLoader : MonoBehaviour
{
    public void LoadNextScene()
    {
        int currentIndex = SceneManager.GetActiveScene().buildIndex;
        SceneManager.LoadScene(currentIndex + 1);
    }

    public void ReloadScene()
    {
        Scene current = SceneManager.GetActiveScene();
        SceneManager.LoadScene(current.name);
    }

    public void LoadByName(string sceneName)
    {
        SceneManager.LoadScene(sceneName);
    }
}
```

To use these, make sure all scenes are added in:

>File → Build Settings → Scenes In Build

---

## Combining Prefabs and Scenes

Prefabs let you **reuse** complex GameObjects across multiple scenes —
for example:

- Your **player character** appears in every level.
- Your **UI prefab** (e.g. health bar, pause menu).
- Your **enemies**, **items**, or **projectiles**.

By keeping these objects as prefabs, you:
- Maintain consistency
- Save development time
- Reduce scene file size
- Simplify updates

---

💡 Example Workflow

1. Build your player GameObject in the Hierarchy.
2. Turn it into a **prefab** by dragging it to the Project window.
3. Create a new scene.
4. Drag the player prefab into the new scene.
5. Add a script that spawns the player if not already present.
---

## Example: Player Prefab Spawner
```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class PlayerSpawner : MonoBehaviour
{
    public GameObject playerPrefab;
    private static GameObject playerInstance;

    void Awake()
    {
        if (playerInstance == null)
        {
            playerInstance = Instantiate(playerPrefab);
            DontDestroyOnLoad(playerInstance);
        }
    }
}
```

Explanation:

- The player is spawned only once.
- `DontDestroyOnLoad()` ensures it **persists** between scenes.
- Great for games with persistent characters or UI.
---

## Key Takeaways

| Concept               | Description                            |
| --------------------- | -------------------------------------- |
| **Prefab**            | Blueprint for reusable GameObjects     |
| **Prefab Variant**    | Modified version of a base prefab      |
| **Scene**             | A self-contained level or game context |
| **SceneManager**      | API for loading and switching scenes   |
| **DontDestroyOnLoad** | Keeps objects between scene changes    |

---

## Exercise

- Create a **Player prefab**.
- Make a Spawner script that spawns multiple collectibles randomly.
- Make a SceneChanger script that can change the scene.
- Create **two scenes**: `Level1` and `Level2`.
- Add a **button** that loads `Level2` when clicked.
  - In the hierarchy, right click and select `UI` -> `Button - TextMeshPro`.
  - Select the **button** in the hierarchy and in the inspector -> Button Component, you'll find the section: `On Click ()`.
  - Hit `plus` icon on that and **drag** your `SceneChanger` script onto it, and then set the function to your `ChangeScene()` method.
- Use DontDestroyOnLoad to persist your player between scenes.

### Bonus Challenge

- Create a **Collectible prefab**.
- Add a UI text element showing how many collectibles the player picked up.
- Make sure it stays visible across scenes (hint: make the UI prefab persistent too)