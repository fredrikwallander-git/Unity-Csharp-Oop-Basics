# 06 - Intermission - Lambdas & Attributes

Before we continue, I would like to explain Lambdas & Attributes as an intermission since we've already touched upon the key features of it without a proper explanation.

---
## Learning Goals
By the end of this intermission, students will:

- Understand `lambda` expressions and how to use them for concise inline methods.
- Learn about **Unity attributes** like `[SerializeField]`, `[Header]`, `[Range]`, and how they affect the Inspector.
- Be able to use both concepts to **write cleaner, more expressive gameplay code**.

---
## Theory Summary

### Lambda Expressions

Lambdas are **short-hand functions** that can be written _inline_.

There are two types of lambas:
- The expression lambda
  - >`(input-parameters) => expression`
- The statement lambda
  - >`(input-parameters) => { <sequence-of-statements> }`

Both makes use of the lambda declaration operator: `=>`

To create a lambda expression, you specify input parameters (if any) on the left side of the lambda operator and an expression or a statement block on the other side.

Instead of this:

```csharp
button.onClick.AddListener(OnButtonClick);

void OnButtonClick()
{
    Debug.Log("Clicked!");
}
```

You can write:
```csharp
button.onClick.AddListener(() => Debug.Log("Clicked!"));
```

`()=>` means: “an anonymous function that takes no parameters and returns nothing.”
If you have parameters, you can write them like this: `(x) => x * x`.

---
### Example: Filtering or Finding Objects

```csharp
List<GameObject> enemies = new List<GameObject>();
var strongEnemies = enemies.Where(e => e.GetComponent<Enemy>().Health > 50);
```

Here:

- `e` = each element in the list
- `=>` = defines what to do with it
- the whole line reads: “get all enemies whose health > 50”

Tip: Use the `var` keyword to implicitly declare and initialize a variable and let the compiler explicitly figure out the correct type when compiling.

---
### Example: Inline Event Actions

Imagine you have a coin that should disappear and update the score when collected:

```csharp
public class Coin : MonoBehaviour
{
    public static event System.Action<int> OnCoinCollected;

    void OnTriggerEnter(Collider other)
    {
        if (other.CompareTag("Player"))
        {
            OnCoinCollected?.Invoke(1); // Add 1 coin
            Destroy(gameObject);
        }
    }
}

public class ScoreUI : MonoBehaviour
{
    int score = 0;

    void Start()
    {
        Coin.OnCoinCollected += (amount) => {
            score += amount;
            Debug.Log("Score: " + score);
        };
    }
}
```

Here the lambda `(amount) => { ... }` makes a quick inline event handler without needing a separate named method.

---
## Attributes

Attributes are small instructions you place above variables, classes, or methods to modify how Unity or the compiler treats them.
They add metadata information about the types in your program and, you can also create your own custom attributed to treat data in a specific way.

We will learn how to make them in time but, for now, it's best to get familiar with the most commonly used ones and how they look:

```csharp
[SerializeField] private int speed;
[Header("Player Settings")]
[Range(1, 10)] public float jumpHeight;
```

---
### Commonly Used Unity Attributes

| Attribute                         | Purpose                                       |
| --------------------------------- | --------------------------------------------- |
| `[SerializeField]`                | Makes private fields visible in the Inspector |
| `[HideInInspector]`               | Hides public fields from the Inspector        |
| `[Range(min, max)]`               | Creates a slider for numeric values           |
| `[Header("Title")]`               | Adds a labeled header above Inspector fields  |
| `[Tooltip("Description")]`        | Adds hover text in the Inspector              |
| `[RequireComponent(typeof(...))]` | Auto-adds required components                 |
| `[ContextMenu("Action Name")]`    | Adds a button in the component’s context menu |

---
## Example: Player Setup Script
```csharp
using UnityEngine;

[RequireComponent(typeof(Rigidbody))]
public class PlayerController : MonoBehaviour
{
    [Header("Movement Settings")]
    [Range(1f, 10f)] 
    [Tooltip("How fast the player moves")]
    [SerializeField] private float moveSpeed = 5f;

    [Header("Jump Settings")]
    [SerializeField] private float jumpForce = 7f;

    private Rigidbody rb;

    void Awake()
    {
        rb = GetComponent<Rigidbody>();
    }

    void Update()
    {
        float h = Input.GetAxis("Horizontal");
        float v = Input.GetAxis("Vertical");
        Vector3 dir = new Vector3(h, 0, v);
        rb.velocity = dir * moveSpeed + new Vector3(0, rb.velocity.y, 0);
    }

    [ContextMenu("Jump")]
    void Jump()
    {
        rb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);
    }
}
```

Now the Inspector is cleaner, the code is safer (fields are private), and you can test “Jump” right from the context menu!

Hint: The context menu is accessed by right-clicking the script, in this case the `PlayerController`, and here you'll find the "Jump" item to use.
Clicking it will execute the `Jump` method in the `PlayerController` script.

---
## Extras: Events

Events enable a `class` or `object` to notify other `classes` or `objects` when something of interest occurs. The class that sends (or **raises**) the event is called the **publisher** and the classes that receive (or **handle**) the event are called **subscribers**.

Create a static and all accessible event that has a parameter of int:

```csharp
public static event System.Action<int> OnCoinCollected;
```

Which can then be subscribed to: (using the `+=` operator)

```csharp
OnCoinCollected += MethodThatUpdatesCoinsText; 
// or use short-hand lambda...  x += (amount) => UpdateCoinsText
```

And to unsubscribe: (using the `-=` operator)

```csharp
OnCoinCollected -= MethodThatUpdatesCoinsText;
```

The class that owns the event can then trigger (or `Invoke`) it and the subscribers will then be notified and can act as they see fit to the event.

---
## Key Takeaways

| Concept                | Description                                            |
| ---------------------- | ------------------------------------------------------ |
| **Lambda expressions** | Inline anonymous methods for compact logic             |
| **Attributes**         | Decorators that change how Unity or C# interprets data |
| **[SerializeField]**   | Makes private fields visible in Inspector              |
| **[ContextMenu]**      | Adds quick actions for debugging and testing           |

---
## Exercises

### Exercise 1 - The Collector’s Reward

Create a collectable item and update a hypothetical managers visual output.

- Create a `Coin` prefab that calls a custom event `OnCollected` when touched by the player.
- Use a **lambda** to update a hypothetical `ScoreManager`’s score (no separate method allowed).
- Use `[SerializeField]` and `[Header]` to make your `ScoreManager`’s Inspector fields organized and clear.

Bonus: Use `[Range]` on the coin’s spin speed so it’s adjustable in the Inspector.

---
### Exercise 2 - Enemy Factory

- Create an `Enemy` which has a static event `OnDeath`.
- Create an EnemySpawner that spawns enemies at random positions.
- Use a lambda inside Start() to subscribe to each enemy’s OnDeath event.
- When an enemy dies, print "Enemy defeated!" using the lambda.
  
Use `[ContextMenu("Spawn Enemy")]` so you can spawn one manually in the Editor.

Hint: Maybe the enemy can just die by simply being `Destroy(enemy, 3f)` after 3 seconds of spawning.

---
### Exercise 3 - The Debug Menu

- Create a `GameManager` and `Player` script.
- Add a `[ContextMenu("Reset Game")]` method that resets the score and repositions the player.
- Add `[Header]` and `[Tooltip]` attributes for all adjustable variables.
