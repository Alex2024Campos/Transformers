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
 >- `Nenhuma ainda.`

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
 Agora, a classe jogador serve como um molde para o nosso player, atribuindo já dentro do próprio código a vida e o dano do mesmo, os quais serão fixos por toda a jogatina. 

- Inimigo:
 A classe inimigo, no contexto de nosso jogo, serve para fornecer algumas informações do que serão os inimigos do jogo: dano, vida, etc. A vida do jogador é puxada para essa classe para ser efetuado o cálculo do dano causado pelo inimigo. Além disso, também conta com as linhas de códigos que geram os projéteis do jogo.

- Wave:
 A wave serve como uma forma de monitorar e administrar o Spawn do prefab do inimigo, utilizando-se de um atributo Boolean para identificar a passagem de waves e permitir a criação de novas instâncias do prefab já determinado. Além disso, permite variar os atributos de e vida do inimigo com o intuito de variar um pouco a dificuldade para eliminar cada um. Sempre que se passa uma horda no jogo, o valor de prefabs é multiplicado por um valor fixo e assim, cria novas instâncias.

- UI:
 A classe menu serve unicamente para identificar os cliques do jogador no nosso menu e assim atribuir a cena do botão respectivo. 
<br>


_________________________________________________________________

# Codificação do Jogo:
lore


<br>

# UI do Jogo:
- Menu inicial
 Foi feito um Canvas para fazer o menu inicial, o qual pelo seu nome deixa a entender, será o primeiro que o jogador verá e permitirá que o mesmo escolha entre começar a jogatina ou sair do nosso jogo. 

- Menu pausa 
 O menu de pausa terá a única função de permitir que o nosso player consiga pausar o jogo quando quiser ou precisar. É um Canvas que é chamado quando a tecla "Esc" é clicada e que desaparece quando "Tab" é pressionado. O código está presente no script do jogador.

- Menu gameover
 Como é de se imaginar, perder o nosso jogo, ou seja, chegar ao valor 0 de vidas, fará a ação de puxar o menu de gameover e permitirá o jogador decidir se vai reiniciar o jogo ou voltar ao menu inicial. Ocódigo está presente no script do jogador.
<br>

# Paleta de Cor do Jogo:
- Junção de cores cinzas com cores mais chamativas e claras.
<br>

_________________________________________________________________

# Referências:
