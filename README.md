>[!Important]
 > ` - Projeto:`
>- Turma: 2°Mtec Desenvolvimento de Jogos Digitais.
>- Escola: Etec Prof. Basilides de Godoy.
>- Sobre: Repositório para a documentação do jogo Transformers, um trabalho proposto pelas professoras Aline Firmino Brito e Nivia Maria Domingues, da Etec Prof. Basilides de Godoy.
>- Datas: Apresentar o jogo (terça-feira que vem, 26/11/2024).
>- Projeto (drive): 

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
   
 - Variação de inimigos: De forma resumida e simples, será uma forma de trazer aquele algo a mais ao jogo. Os inimigos gerados, então, estarão causando uma quantidade de dano diferente de um para outro, além de carregar consigo uma quantidade de vida também aleatória, ou seja, dentro dos limites impostos por nós, os adversários do jogador poderão contar com mais ou menos vida e dano, servindo então ou como um dificultor ou como um facilitador do jogo.
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

```

- Jogador:
```csharp

```

- Wave:
```csharp

```

- UI:
```csharp

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

| Material | Hexadecimal |
| --- | --- |
| Piso | 708080 |
| Muro | 1E8063 |
| Bumbleebe1 | 7D4C30 |
| Inimigo1 | 747B7A |
| Menu_Inicial | 7D4C30 |
| Menu_GameOver | 7D4C30 |
| Menu_Pausa | 7D4C30 |
<br>
_________________________________________________________________

# Diagrama UML:

- Jogador1:
(Associação -----> UI. Jogador precisa apertar algo pra rodar o jogo).

- Inimigo:
(Associação Inimigo <-----> Jogador. Precisa do jogador pra existir).

- Wave:
Associação (Wave <-----> Inimigo. Inimigo precisa de uma Wave pra nascer e Wave precisa do Inimigo).

- UI: 



_________________________________________________________________

# Referências:
 