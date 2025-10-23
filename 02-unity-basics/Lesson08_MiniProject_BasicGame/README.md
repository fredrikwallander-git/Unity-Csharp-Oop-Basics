# 08 - Mini Project: Basic Game

Let's go through a couple more things before we wrap this chapter up with your last big exercise!

### Physics Overview

Unity handles physics through components:

| Component                   | Description                                                    |
| --------------------------- |----------------------------------------------------------------|
| **Rigidbody / Rigidbody2D** | Makes an object respond to physics (gravity, forces).          |
| **Collider / Collider2D**   | Defines the physical shape for collision detection.            |
| **Trigger**                 | A collider that detects overlaps without physical interaction. |
| **Physics / Physics2D**     | Provides global physics utilities and settings.                |

Whether you are making a `3D` or `2D` game, remember that Unity provides a `2D` version for each physics component as well as the `3D` version.

The same goes for the event methods, for example:
- `OnCollisionEnter(Collision collision)`
- `OnCollisionEnter2D(Collision2D collision)`
- **_or_**
- `OnTriggerEnter(Collider collider)`
- `OnTriggerEnter2D(Collider2D collider)`

Notice the subtle difference?

Make sure that you use the correct version for your game.

---

### Example: 2D Player with Rigidbody and Collider
```csharp
using UnityEngine;

[RequireComponent(typeof(Rigidbody2D), typeof(Collider2D))]
public class PlayerPhysics : MonoBehaviour
{
    [SerializeField] private float moveSpeed = 5f;
    [SerializeField] private float jumpForce = 7f;
    private Rigidbody2D rb;
    private bool isGrounded;

    void Awake()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        float move = Input.GetAxis("Horizontal");
        rb.velocity = new Vector2(move * moveSpeed, rb.velocity.y);

        if (Input.GetButtonDown("Jump") && isGrounded)
            rb.AddForce(Vector2.up * jumpForce, ForceMode2D.Impulse);
    }

    void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.contacts[0].normal.y > 0.5f)
            isGrounded = true;
    }

    void OnCollisionExit2D(Collision2D collision)
    {
        isGrounded = false;
    }
}
```

This script allows horizontal movement and jumping with a crude and simple collision-based ground detection.

---

## Triggers for Pickups and Damage

A trigger collider doesn’t block movement - it simply notifies you when objects overlap.

### Example: 2D Coin Pickup with UI Feedback
```csharp
using UnityEngine;

public class Coin : MonoBehaviour
{
    [SerializeField] private int value = 1;

    void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Player"))
        {
            FindObjectOfType<ScoreUI>().UpdateScore(value);
            Destroy(gameObject);
        }
    }
}
```
Setup:
- Add a `CircleCollider2D` to the coin.
- Check **Is Trigger**.
- Add a UI text object and `ScoreUI` class to show score.

---

## Damage and Health UI Example

Let’s add a simple health system and show it on screen for a 2D game.

### PlayerHealth.cs
```csharp
using UnityEngine;
using UnityEngine.UI;

public class PlayerHealth : MonoBehaviour
{
    [SerializeField] private int maxHealth = 5;
    [SerializeField] private Slider healthBar;

    private int currentHealth;

    void Start()
    {
        currentHealth = maxHealth;
        healthBar.maxValue = maxHealth;
        healthBar.value = currentHealth;
    }

    public void TakeDamage(int amount)
    {
        currentHealth -= amount;
        healthBar.value = currentHealth;

        if (currentHealth <= 0)
            Debug.Log("Player Died!");
    }
}
```

### Enemy.cs
```csharp
using UnityEngine;

public class Enemy : MonoBehaviour
{
    [SerializeField] private int damage = 1;

    void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.collider.CompareTag("Player"))
        {
            collision.collider.GetComponent<PlayerHealth>().TakeDamage(damage);
        }
    }
}
```

- Add a **_tag_** for the Player game object.
- Add a `UI Slider` to display the health value.
- Implement movement or manually move the game object to test functionality.

---

## Final Project: Recreate a Classic

The grand finale of this chapter is upon us, now is your time to shine and recreate a classic game of your choosing.

The game can be `2D` or `3D`, entirely up to you, just try to keep the scope **_low_** and put **focus** on **_one level_** and **_mechanic_** and make them great.

### Objective:

Create a playable mini version of a classic retro game such as..
- **_Super Mario Bros_**
- **_Donkey Kong_**
- **_Battletoads_**
- **_Metroid_**
- **_Zelda_**
- **_Galaga_**
- **_Castlevania_**

.. or others.

---

## Requirements

Each project should feature:

| Feature                        | Description                                          |
| ------------------------------ | ---------------------------------------------------- |
| **Player**                     | Controllable character using physics and input       |
| **Level**                      | Prefab-based layout (platforms, hazards, or enemies) |
| **Collectibles / Score**       | Points, coins, or power-ups                          |
| **Basic UI**                   | Score, health, or status display                     |
| **Start / Restart Menu**       | Play, Restart, and Exit functionality                |
| **Scene Management**           | At least one gameplay scene and one menu scene       |
| **Encapsulation & Clean Code** | Classes, fields, and methods used responsibly        |
| **Prefab Usage**               | Player, enemies, collectibles created from prefabs   |

---

### Optional Stretch Goals:

- Implement animations (sprite swap or Animator).
- Add enemy AI (simple patrol or follow).
- Add power-ups that modify player stats.
- Add a pause menu with resume/restart options.
- Include music and sound effects.

---

## Submission

Each student should submit:
- A `GitHub` repository link.
- A **_short_** gameplay video or gif (20-30s).
- A README.md describing the chosen game and implemented features.

---

## Remember

- Use `GitHub` to **commit** and **push** your project to your repository.
- Keep the code private if you intend to use assets that are bought and not open source and freely sharable.
- Commit **_regularly_**, preferably after each milestone, source control is your friend!
- Reuse and adapt the previous example scripts if you want.
- Pick a **_small_**, **_achievable_** scope (_one_ _level_, _one_ _mechanic_).
- And last but not least.. **HAVE FUN!**

---
... " Thank you! "

" But our princess is in another castle! 

" - Toad." ...