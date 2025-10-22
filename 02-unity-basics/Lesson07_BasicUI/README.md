# 07 - Basic UI

## Learning Goals
By the end of this lesson, students will:

- Understand the **Unity UI system** (Canvas, RectTransform, and EventSystem).
- Know how to create and style **Text**, **Image**, and **Button** components.
- Wire up buttons using **lambdas** or Unity Events.
- Display and update data dynamically (like score, health, or collected items).
- Create a simple **Main Menu** and **HUD** using prefabs.

## Theory Summary

### The Building Blocks of Unity UI

Unity’s UI system (UGUI) is based on **GameObjects** — but they live inside a **Canvas** instead of the regular 3D world.

| Component         | Description                                                           |
| ----------------- | --------------------------------------------------------------------- |
| **Canvas**        | Root element that draws UI. Every UI element must be under one.       |
| **RectTransform** | Like Transform, but for 2D positioning and scaling inside the Canvas. |
| **Text (TMP)**    | Displays text. Use TextMeshPro for crisp, scalable text.              |
| **Image**         | Displays a sprite or color rectangle.                                 |
| **Button**        | Combines an image with an interactive click area.                     |
| **EventSystem**   | Handles input events (auto-created when you add UI).                  |


---

## Creating a Simple HUD

Let’s display the player’s score in the corner of the screen.

- Right-click in the Hierarchy → UI → Canvas
- Inside Canvas → UI → Text (TMP)
- Rename it to ScoreText
- Anchor it to the top-left and set the text to "Score: 0"

### Example: Updating UI from Code
```csharp
using UnityEngine;
using TMPro;

public class ScoreUI : MonoBehaviour
{
    [SerializeField] private TextMeshProUGUI scoreText;
    private int score = 0;

    void Start()
    {
        UpdateScore(0);
    }

    public void UpdateScore(int amount)
    {
        score += amount;
        scoreText.text = $"Score: {score}";
    }
}
```

Now, whenever something happens (like collecting a coin), call:

```csharp
FindObjectOfType<ScoreUI>().UpdateScore(1);
```

---

## Buttons and Interactions

Buttons are interactive UI elements that can trigger code when clicked.

- Example: Button Setup with Lambda
- Create a Button under the Canvas.
- Add this script:

```csharp
using UnityEngine;
using UnityEngine.UI;

public class ButtonExample : MonoBehaviour
{
    [SerializeField] private Button clickButton;

    void Start()
    {
        clickButton.onClick.AddListener(() => {
            Debug.Log("Button clicked!");
        });
    }
}
```

- Drag the Button from the scene into the script’s clickButton field.

Now you can click the button and see the message appear in the Console.

--- 

## Canvas and Anchors Deep Dive

UI elements use a RectTransform, not a regular Transform.

- Anchors define where your element is pinned in the Canvas.
- Pivot defines the rotation/scale origin.
- Position is relative to the anchors.

This allows your UI to automatically adapt to different resolutions and screen sizes.

### Example
| Use Case                      | Recommended Anchor |
| ----------------------------- | ------------------ |
| Score text (top left)         | Top-left           |
| Health bar (top center)       | Top                |
| Pause button (top right)      | Top-right          |
| Mobile joystick (bottom left) | Bottom-left        |

---

## Example: A Simple Main Menu

Let’s make a functional main menu with “Play” and “Quit” buttons.

### Setup

- Create a new scene called **MainMenu**.
- Add a Canvas → inside, add two Buttons: “Play” and “Quit”.
- Add this script to an empty GameObject called `MenuManager`:

```csharp
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;

public class MenuManager : MonoBehaviour
{
    [SerializeField] private Button playButton;
    [SerializeField] private Button quitButton;

    void Start()
    {
        playButton.onClick.AddListener(() => SceneManager.LoadScene("Level1"));
        quitButton.onClick.AddListener(() => Application.Quit());
    }
}
```

When you build the game, `Application.Quit()` will work.
In the Editor, it just does nothing - that’s expected and how they built the feature.

---

## Example: Pause Menu with Lambdas and CanvasGroups

A CanvasGroup lets you easily hide or show entire UI panels.

```csharp
using UnityEngine;
using UnityEngine.UI;

public class PauseMenu : MonoBehaviour
{
    [SerializeField] private CanvasGroup pauseMenu;
    [SerializeField] private Button resumeButton;

    private bool isPaused = false;

    void Start()
    {
        resumeButton.onClick.AddListener(() => TogglePause(false));
        pauseMenu.alpha = 0;
        pauseMenu.interactable = false;
        pauseMenu.blocksRaycasts = false;
    }

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Escape))
            TogglePause(!isPaused);
    }

    void TogglePause(bool value)
    {
        isPaused = value;
        pauseMenu.alpha = isPaused ? 1 : 0;
        pauseMenu.interactable = isPaused;
        pauseMenu.blocksRaycasts = isPaused;
        Time.timeScale = isPaused ? 0 : 1;
    }
}
```

## Key Takeaways

| Concept           | Description                                       |
| ----------------- | ------------------------------------------------- |
| **Canvas**        | Root object for all UI elements                   |
| **RectTransform** | 2D version of Transform                           |
| **TextMeshPro**   | High-quality text rendering                       |
| **Button**        | UI element for triggering actions                 |
| **CanvasGroup**   | Controls visibility and interaction for UI groups |

---

## Exercises

### Exercise 1 — Score HUD

- Create a **HUD** showing player score and health.
- Update the score when the player collects coins.
- Use `[Range]` attributes to make editable test values.

### Exercise 2 — Main Menu

- Create a **Main Menu** scene with Play, Credits, and Quit buttons.
- Add transitions between scenes using lambdas and SceneManager.
- Use `[Header]` attributes in your script to keep button fields organized.

Hint: Backtrack to lesson 05 if you forgot how to load scenes.

### Exercise 3 — Pause Menu

- Create a **Pause Menu** with Pause and Resume buttons.
- Freeze time using `Time.timeScale`.
- Un-freeze time when resuming.

### Bonus Challenge — Dynamic Health Bar

- Use a `Slider` UI element as a health bar.
- Update it in real-time from a `PlayerHealth` script.
- Change the bar color dynamically based on remaining health.