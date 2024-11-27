>[!Important]
 > ` - Projeto:`
>- Turma: 2°Mtec Desenvolvimento de Jogos Digitais.
>- Escola: Etec Prof. Basilides de Godoy.
>- Sobre: Repositório para a documentação do jogo Transformers, um trabalho proposto pelas professoras Aline Firmino Brito e Nivia Maria Domingues, da Etec Prof. Basilides de Godoy.
>- Datas: Apresentar o jogo (terça-feira que vem, 26/11/2024).
>- Projeto (drive): https://drive.google.com/drive/folders/1_ls9ojh6CY0qQvsZfG7ksWS_e6w3Heri?usp=sharing

>[!Important]
 > ` - Equipe:`
>- Alex Campos de Oliveira
>- Felipe Mussato Rodrigues

> [!NOTE]
 > `- Alterações do Jogo:`
 >- `Acrescento de um projétil.`
 >- `Exclusão dos estilos de luta e variação do inimigo (mudar vida, etc).`
>- `Mudança nos principais personagens.`

> [!NOTE]
>- `Gameplay`:
(Video)
_________________________________________________________________

# História do Jogo:
Sam e Mikaela, fugindo de um Decepticon, fogem de forma desesperada até um depósito de carros abandonado. Então, o Autobot, antes transformado em carro e que estava a ajudar os dois a fugir do inimigo, freia bruscamente e se transforma em robô, pronto para defender os humanos. O confronto é intenso, com o Autobot trocando golpes poderosos com o Decepticon e seus "minions" (inimigos menores, os quais estão auxiliando o inimigo da cena a lutar contra nosso jogador) em meio as pilhas de carros sucateados.
<br>

# Mecânica do Jogo:
O jogo contará com algumas mecânicas, sendo elas: pontuação, hordas de inimigos e variação dentre os tipos de inimigos. Especificando melhor cada:
 - Pontuação: Cada inimigo destruído pelo nosso jogador, nesse caso o Autobot, gerará pontos para o mesmo. O objetivo do jogador é se manter vivo enquanto tenta atingir o máximo de pontuação possível.
   
 - Hordas de inimigos: Um sistema de "wave", o qual ditará a quantidade de inimigos que serão gerados durante a cena com base no valor atual da horda, o qual se altera com base na destruição de todos os inimigos na cena. Ou seja, sempre que você eliminar uma horda de inimigos inteira, a wave terá o acrescento de +1, multiplicando a quantidade de inimigos presentes no mapa.
<br>

# Classes do Jogo:
As classes do jogo são: "Jogador", "Inimigo", "Wave" e "Menu". Abaixo estará melhor detalhadas essas classes:

- Jogador: 
 A classe jogador serve como um molde para o nosso player, atribuindo-lhe algumas informações como: velocidade, frequência de disparo, pontuação, dentre outros. O jogador será capaz de disparar projéteis de fogo para eliminar os robôs inimigos.

- Inimigo:
 No contexto de nosso jogo, a classe inimigo servirá para fornecer algumas informações que serão utilizadas de base para outros métodos de outros scripts. Através da classe wave, criaremos novas instâncias do inimigo do jogo, as quais serão cada vez maiores conforme as waves forem passando.

- Wave:
 A Wave serve como uma forma de monitorar e administrar o Spawn de instâncias do prefab do inimigo, utilizando-se de alguns métodos para identificar o Spawn e a passagem de wave (que aumentará o valor de inimigos a serem criados). Utilizamos também da coroutine para criar um tipo de "timer" dentre a criação de uma wave (ou seja, passou da 1° wave pra segunda, será dado um tempo para que os códigos dentro dela sejam efetuados novamente).

- UI:
 A classe menu serve unicamente para identificar os cliques do jogador no nosso menu e assim atribuir a cena do botão respectivo. 
<br>


_________________________________________________________________

# Codificação do Jogo:
 Abaixo teremos os códigos do nosso projeto e uma explicação mais detalhada das principais linhas de códigos deles.

- Inimigo:
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class Enemy : MonoBehaviour
{
    Animator animator;
    private float speed = 5f;
    private CharacterController characterController;
    public GameObject player;
    public GameObject explosionEffect; 
    public float explosionHeightOffset = 1f; 
    private float gravity = -9.8f;
    private float verticalVelocity;
    public Wave waveManager;
    public Player playerScript;

    [System.Obsolete]
    void Start()
    {
        animator = GetComponent<Animator>();
        characterController = GetComponent<CharacterController>();
        playerScript = FindObjectOfType<Player>();

    }

    void Update()
    {
       

        Vector3 direction = (player.transform.position - transform.position).normalized;

        Quaternion lookRotation = Quaternion.LookRotation(direction);
        transform.rotation = Quaternion.Slerp(transform.rotation, lookRotation, Time.deltaTime * 5f);

        if (characterController.isGrounded)
        {
            verticalVelocity = 0; 
        }
        else
        {
            verticalVelocity += gravity * Time.deltaTime;
        }

        Vector3 move = direction * speed * Time.deltaTime;
        move.y = verticalVelocity * Time.deltaTime; 
        characterController.Move(move);

        bool isMoving = direction.magnitude > 0.1f;
        if (animator != null)
        {
            animator.SetBool("Walk", isMoving);
            animator.SetBool("Idle", !isMoving);
        }
    }

    private void OnTriggerEnter(Collider other)
    {
        if (other.CompareTag("Player"))
        {
            playerScript.TakeDamage();
            Explode();

        }

        if (other.CompareTag("Projectile")) 
        {
            playerScript.AddPoints(10f);
            Explode();
        }
    }

    public void Explode()
    {
        if (explosionEffect != null)
        {
            Vector3 explosionPosition = transform.position + Vector3.up * explosionHeightOffset;
            Instantiate(explosionEffect, explosionPosition, Quaternion.identity);
        }
        Destroy(gameObject);
    }
    public void OnDestroy()
    {
        waveManager.EnemyKilled();
    }
}
```

- Jogador:
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using TMPro;

public class Player : MonoBehaviour
{
    public int lives = 3;
    public float points = 0f;
    public TMP_Text pointsText;
    public TMP_Text livesText;
    public GameObject gameOverScreen;
    public GameObject pauseMenu;

    public GameObject projectilePrefab;
    public Transform shootPoint;
    public float projectileSpeed = 20f;
    public float fireRate = 0.5f;
    private float lastShotTime;

    private Animator animator;
    public CharacterController controller;
    public Transform playerObj;
    public Transform cam;
    public float moveSpeed = 5f;
    public float gravity = -9.81f;
    private Vector3 velocity;
    private bool isGrounded;



    private bool isPaused = false;

    private void Start()
    {
        UpdateUI();
        gameOverScreen.SetActive(false);
        pauseMenu.SetActive(false);
        animator = GetComponent<Animator>();

    }

    private void Update()
    {
        Pause();

        if (Input.GetMouseButton(0) && Time.time >= lastShotTime + fireRate)
        {
            FireProjectile();
            lastShotTime = Time.time;
        }

        isGrounded = controller.isGrounded;

        if (isGrounded && velocity.y < 0)
        {
            velocity.y = -2f;
        }

        float horizontalInput = Input.GetAxisRaw("Horizontal");
        float verticalInput = Input.GetAxisRaw("Vertical");

        Vector3 forward = new Vector3(cam.forward.x, 0, cam.forward.z).normalized;
        Vector3 right = new Vector3(cam.right.x, 0, cam.right.z).normalized;
        Vector3 moveDir = (forward * verticalInput + right * horizontalInput).normalized;

        controller.Move(moveDir * moveSpeed * Time.deltaTime);

        velocity.y += gravity * Time.deltaTime;
        controller.Move(velocity * Time.deltaTime);

        bool isMoving = moveDir.magnitude > 0.1f;
        if (animator != null)
        {
            animator.SetBool("Run", isMoving);
            animator.SetBool("Idle", !isMoving);
        }

        LookAtMouse();
    }

    private void LookAtMouse()
    {
        Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
        if (Physics.Raycast(ray, out RaycastHit hitInfo))
        {
            Vector3 targetPoint = hitInfo.point;
            Vector3 lookDirection = targetPoint - playerObj.position;
            lookDirection.y = 0;
            playerObj.rotation = Quaternion.LookRotation(lookDirection);
        }
    }

    void FireProjectile()
    {
        if (projectilePrefab == null || shootPoint == null) return;
        GameObject projectile = Instantiate(projectilePrefab, shootPoint.position, shootPoint.rotation);
        Rigidbody rb = projectile.GetComponent<Rigidbody>();
        if (rb != null)
        {
            rb.velocity = shootPoint.forward * projectileSpeed;
        }
        Destroy(projectile, 5f);
    }

    private void Pause()
    {
        if (Input.GetKeyDown(KeyCode.Escape))
        {
            isPaused = !isPaused;

            if (isPaused)
            {
                Time.timeScale = 0f;
                pauseMenu.SetActive(true);
            }
            else
            {
                Time.timeScale = 1f;
                pauseMenu.SetActive(false);
            }
        }
    }

    public void TakeDamage()
    {
        lives--;

        if (lives <= 0)
        {
            GameOver();
        }
        else
        {
            UpdateUI();
        }
    }

    public void AddPoints(float amount)
    {
        points += amount;
        UpdateUI();
    }

    private void UpdateUI()
    {
        pointsText.text = "Points: " + points.ToString();
        livesText.text = "Lives: " + lives.ToString();
    }

    private void GameOver()
    {
        gameOverScreen.SetActive(true);
    }
}
```

- Wave:
```csharp
using System.Collections;
using System.Collections.Generic;
using TMPro;
using UnityEngine;

public class Wave : MonoBehaviour
{
    public GameObject enemyPrefab;
    public Transform[] spawnPoints; 
    public TMP_Text waveText; 
    private int currentWave = 1; 
    private int enemiesToSpawn = 2; 
    private int enemiesAlive = 0;

    private void Start()
    {
        UpdateWaveText(); 
        StartCoroutine(SpawnWave()); 
    }

    private IEnumerator SpawnWave()
    {
        yield return new WaitForSeconds(2f);

        for (int i = 0; i < enemiesToSpawn; i++)
        {
            Transform spawnPoint = spawnPoints[Random.Range(0, spawnPoints.Length)];
            Instantiate(enemyPrefab, spawnPoint.position, spawnPoint.rotation);

            enemiesAlive++; 
            yield return new WaitForSeconds(0.5f);
        }
    }

    public void EnemyKilled()
    {
        enemiesAlive--; 
        if (enemiesAlive <= 0)
        {
            AdvanceWave();
        }
    }

    private void AdvanceWave()
    {
        currentWave++; 
        enemiesToSpawn += 3; 
        UpdateWaveText();
        StartCoroutine(SpawnWave()); 
    }

    private void UpdateWaveText()
    {
        waveText.text = "Wave: " + currentWave.ToString();
    }
}
```

- UI:
```csharp
using System.Collections;
using System.Collections.Generic;
using Unity.VisualScripting;
using UnityEditorInternal;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;
using UnityEngine.UIElements;

public class UI : MonoBehaviour
{
    private bool VerEscolha;
    private AudioSource AudioMenu;
    public AudioSource AudioJogo;

    private void Iniciar()
    {
        AudioMenu = GetComponent<AudioSource>();
        SceneManager.LoadSceneAsync("Armazem");
    }
    private void Sair()
    {
        Application.Quit();
    }
    private void Reiniciar(){
        SceneManager.LoadSceneAsync("Armazem");
    }



    void Update()

    {
        if (gameObject.tag == "Entrar")
        {
            Iniciar();
        }

        else if (gameObject.tag == "Sair")
        {
            Sair();
        }
        else if (gameObject.tag == "GameOver")
        {
            Reiniciar();
        }
    }
}
```
<br>

# UI do Jogo:
- Menu inicial
 Foi feito um Canvas para fazer o menu inicial, o qual pelo seu nome deixa a entender, será o primeiro que o jogador verá e permitirá que o mesmo escolha entre começar a jogatina ou sair do nosso jogo. Contém um material, o qual foi inserida uma imagem que havia sido feita no Canvas, a sua imagem estará abaixo:

- Menu pausa 
 O menu de pausa terá a única função de permitir que o nosso player consiga pausar o jogo quando quiser ou precisar. É um Canvas que é chamado quando a tecla "Esc" é clicada e que desaparece quando "Tab" é pressionado. O código está presente no script do jogador. Contém um material, o qual foi inserida uma imagem que havia sido feita no Canvas, a sua imagem estará abaixo:

- Menu gameover
 Como é de se imaginar, perder o nosso jogo, ou seja, chegar ao valor 0 de vidas, fará a ação de puxar o menu de gameover e permitirá o jogador decidir se vai reiniciar o jogo ou voltar ao menu inicial. Ocódigo está presente no script do jogador. 
<br>

# Paleta de Cor do Jogo:
- No ambiente sombrio e desbotado do depósito, o amarelo vibrante de nosso protagonista se destaca e serve como forma de enfatizar sua presença protetora e heroica. O contraste do ambiente com a cor do personagem reforça o seu papel como defensor, trazendo o seu elemento de segurança e confiança visual. Para se contrastar com Bumblebee, teremos o principal inimigo, lacaios Decepticons enviados por Barricade para capturar Sam e que contarão com tons escuros, metálico e ferrugem, passando uma sensação de ameaça constante. As cores frias fazem dele uma figura intimidante e perigosa, especialmente quando combinado com o ambiente sombrio do depósito, destacando seu papel como inimigo e ameaça. Por fim, o cenário é dominado por cores desgastadas e metálicas, com tons de cinza, marrom e ferrugem, criando uma sensação de abandono e isolamento e tornando o confronto entre Bumblebee e os lacaios de Barricade ainda mais intenso.

- Materials de exemplo:
 Vale ressaltar que os hexadecimais de cada um dos materials são de exemplo e aproximados, pois não foi-se utilizado de um material próprio com uma cor específica no RGB para podemos retirar essa informação e colocar aqui.

| Textura | Imagem | Hexadecimal |
| --- | --- | --- |
| Piso | ![image](https://github.com/user-attachments/assets/36cd2cf1-04d5-499f-b256-b538b3e10a46)| 000000 |
| Muro | ![image](https://github.com/user-attachments/assets/115e2b22-88a5-4480-989f-83d79701bc28)| 868080 |
| Bumbleebe1 | ![image](https://github.com/user-attachments/assets/179cd790-02c5-4827-aa7a-b3964f78cad9)| FFFD00 |
| Inimigo1 |  ![image](https://github.com/user-attachments/assets/20979302-5573-428b-8ffd-3805a9a2ab7e)  ![image](https://github.com/user-attachments/assets/cd32eb2b-46ec-4502-bf62-e0bf9cf5133a)| 5A5A5A |
| Menu_Inicial | ![image](https://github.com/user-attachments/assets/ff84eed9-bca4-49c1-9386-ca5840443d65)| N/A |
| Menu_GameOver | ![image](https://github.com/user-attachments/assets/439f1145-9765-4702-9bbb-64204148cafa)| N/A |
| Menu_Pausa | ![image](https://github.com/user-attachments/assets/95560d2f-24c2-4b18-847e-a53436f3ba41)| N/A|
<br>
_________________________________________________________________

# Diagrama UML:

_________________________________________________________________

# Referências:
 
