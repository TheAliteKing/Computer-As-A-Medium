// ==========================================
// 1. Bullet.cs (Unchanged)
// ==========================================
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Bullet : MonoBehaviour
{
    void Start()
    {
        
    }

    void Update()
    {
        transform.Translate(new Vector3(0, 1, 0) * Time.deltaTime * 8f);
        if (transform.position.y > 6.5f)
        {
            Destroy(this.gameObject);
        }
    }
}

// ==========================================
// 2. Enemy.cs (Enemy Type 1 - Unchanged)
// ==========================================
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Enemy : MonoBehaviour
{
    void Start()
    {
        
    }

    void Update()
    {
        transform.Translate(new Vector3(0, -1, 0) * Time.deltaTime * 3f);
        if (transform.position.y < -6.5f)
        {
            Destroy(this.gameObject);
        }
    }
}

// ==========================================
// 3. EnemyTwo.cs (TASK 2: New Enemy Type)
// ==========================================
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class EnemyTwo : MonoBehaviour
{
    private float horizontalSpeed = 4f;
    private float verticalSpeed = 4f;
    private float horizontalScreenLimit = 9f;

    void Start()
    {
        
    }

    void Update()
    {
        // Moves diagonally downward and bounces off the left and right edges
        transform.Translate(new Vector3(horizontalSpeed, -verticalSpeed, 0) * Time.deltaTime);

        if (transform.position.x > horizontalScreenLimit)
        {
            horizontalSpeed = -Mathf.Abs(horizontalSpeed);
        }
        else if (transform.position.x < -horizontalScreenLimit)
        {
            horizontalSpeed = Mathf.Abs(horizontalSpeed);
        }

        if (transform.position.y < -6.5f)
        {
            Destroy(this.gameObject);
        }
    }
}

// ==========================================
// 4. GameManager.cs (TASK 2: Spawns Enemy 2)
// ==========================================
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GameManager : MonoBehaviour
{
    public GameObject enemyOnePrefab;
    public GameObject enemyTwoPrefab;

    void Start()
    {
        // Enemy 1: starts after 1s, repeats every 2s
        InvokeRepeating("CreateEnemyOne", 1f, 2f);
        // Enemy 2: starts after 3s, repeats every 3.5s
        InvokeRepeating("CreateEnemyTwo", 3f, 3.5f);
    }

    void Update()
    {
       
    }

    void CreateEnemyOne()
    {
        Instantiate(enemyOnePrefab, new Vector3(Random.Range(-9f, 9f), 6.5f, 0), Quaternion.identity);
    }

    void CreateEnemyTwo()
    {
        Instantiate(enemyTwoPrefab, new Vector3(Random.Range(-8.5f, 8.5f), 6.5f, 0), Quaternion.identity);
    }
}

// ==========================================
// 5. Player.cs (TASK 1: Bottom-Half Movement)
// ==========================================
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Player : MonoBehaviour
{
    private float playerSpeed;
    private float horizontalInput;
    private float verticalInput;

    private float horizontalScreenLimit = 9.5f;
    private float verticalScreenLimit = 6.5f;

    public GameObject bulletPrefab;

    void Start()
    {
        playerSpeed = 6f;
    }

    void Update()
    {
        Movement();
        Shooting();
    }

    void Shooting()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            Instantiate(bulletPrefab, transform.position + new Vector3(0, 1, 0), Quaternion.identity);
        }
    }

    void Movement()
    {
        horizontalInput = Input.GetAxis("Horizontal");
        verticalInput = Input.GetAxis("Vertical");

        transform.Translate(new Vector3(horizontalInput, verticalInput, 0) * Time.deltaTime * playerSpeed);

        // Player wraps horizontally from left to right and vice versa
        if (transform.position.x > horizontalScreenLimit || transform.position.x <= -horizontalScreenLimit)
        {
            transform.position = new Vector3(transform.position.x * -1, transform.position.y, 0);
        }

        // Player stays ONLY in the bottom half of the screen (between -verticalScreenLimit and 0)
        if (transform.position.y > 0f)
        {
            transform.position = new Vector3(transform.position.x, 0f, 0);
        }
        if (transform.position.y < -verticalScreenLimit)
        {
            transform.position = new Vector3(transform.position.x, -verticalScreenLimit, 0);
        }
    }
}

// That is the end of Week 11 