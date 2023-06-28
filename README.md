# Avaliação Final
## MIEIC-PLOG

**Swack Grupo 5 turma.**
João Basto do Rosário (up201806334) 
Jorge Levi Perdigoto da Costa (up201706518)

**Contribuição**
João Basto do Rosário 65%
Jorge Levi Perdigoto da Costa 35%

### Como executar o código:
1. Abrir o programa SICStus (Versão testada : 4.6.0)
2. Alterar a fonte do programa para MS Gothic (Ir a 'settings' -> 'font' -> selecionar 'MS Gothic')
3. Fazer consult do ficheiro `main.pl`
4. Escrever `play.`

#### Descrição do Jogo

**Swack** é um jogo de território jogado por dois jogadores, um controla as peças brancas e o outro as pretas e pode ser jogado num tabuleiro de qualquer tamanho 
No nosso caso o tamanho máximo será `10*10` e o mínimo será `3*3`.
O Tamanho recomendado é `9*9`.
No começo do jogo o tabuleiro esta coberto por peças pretas e brancas num padrão em xadrex (o espaço do meio está vazio caso o tabuleiro seja par). Cada jogador tem tambem peças na mão.

##### Inicio
Quem controla as peças pretas começa.
Cada jogador pode na sua fazer dois tipos de jogada.

##### Jogadas
No seu turno um jogador pode:
1. Passar a vez ao outro jogador.
2. Mover uma peça do top de uma stack para o top de uma stack de cor oposta e altura igual. 
Posteriormente deve colocar uma peça do oponente em cima da stack de qual foi movida a primeira peça.

Cada elemento do tabuleiro é um **átomo** que tem a seguinte estrutura xy, em x representa preto (p) ou branco (b) , e em que y é a altura da stack.

##### Fim
O jogo termina quando ambos os jogadores passarem a sua jogada em seguida. O vencedor é determinado

### Lógica do jogo

#### Representação Interna do Estado de Jogo

Cada um dos estados tem um predicado que representa a atual 'board'.
##### Estado Inicial
```prolog
initial([
  [b1, p1, b1, p1, b1, p1, b1, p1, b1],
  [p1, b1, p1, b1, p1, b1, p1, b1, p1],
  [b1, p1, b1, p1, b1, p1, b1, p1, b1],
  [p1, b1, p1, b1, p1, b1, p1, b1, p1],
  [b1, p1, b1, p1, b1, p1, b1, p1, b1],
  [p1, b1, p1, b1, p1, b1, p1, b1, p1],
  [b1, p1, b1, p1, b1, p1, b1, p1, b1],
  [p1, b1, p1, b1, p1, b1, p1, b1, p1],
  [b1, p1, b1, p1, b1, p1, b1, p1, b1]
  ]).
```


##### Estado Intermédio
```prolog
mid([
  [b2, p1, p3, p1, p1, p1, b4, p1, p1],
  [p1, p1, b4, b2, b4, p1, b2, p3, p1],
  [p1, p1, b2, b4, p1, p3, p1, p1, p1],
  [p1, p3, p1, p1, p3, p1, b2, p1, p1],
  [p3, p1, p1, b4, p1, p3, p1, p1, p1],
  [p1, b4, b2, b2, p1, p1, b2, b2, p1],
  [p1, p3, p1, p3, p3, p1, b2, p1, p1],
  [b4, p1, b2, p1, b2, p1, p3, p1, p1],
  [p1, p1, p3, p1, p1, p3, p1, p1, b4]
  ]).
```


##### Estado Final
```prolog
final([
  [b4, p1, p3, p1, b2, p1, b4, p1, p1],
  [p1, p1, b4, b2, b4, p1, b2, p3, p1],
  [b2, b2, b2, b4, p1, p3, p1, b2, b2],
  [p1, p3, p1, p1, p3, p1, b2, p1, p1],
  [p3, p1, p1, b4, p1, p3, p1, b2, b2],
  [p1, b4, b2, b2, p1, p1, b2, p3, p1],
  [p1, p3, p1, p3, p3, p1, b2, p1, p1],
  [b4, b2, b2, p1, b2, p1, p3, p1, p1],
  [p1, p1, p3, b2, p1, p3, p1, b2, b4]
  ]).
```
#### Visualização do estado de jogo
O programa é inciado por play/0, demonstrando em seguida um menu, este pedindo o size do Board e o tipo de jogo: Player vs Player, Player vs Computer, Computer vs Computer. Dependendo do tipo de jogo, pede o nível de jogo do Computador (1 e 2), sendo que no nível 1 o Computador faz jogadas random, e no nível 2 faz jogadas inteligentes.

##### Menu
![Menu](https://github.com/Deadrosas/PLOG-Project/blob/main/resources/menu.PNG)

Por ordem seguem se imagens da consola que representam cada um dos estados de jogos possíveis.

##### Estado Inicial
![Estado Inicial](https://github.com/Deadrosas/PLOG-Project/blob/main/resources/initial.PNG)
##### Estado Intermédio
![Estado Intermédio](https://github.com/Deadrosas/PLOG-Project/blob/main/resources/mid.PNG)
##### Estado Final
![Estado Final](https://github.com/Deadrosas/PLOG-Project/blob/main/resources/final.PNG)

#### Lista de jogadas válidas
Cada jogada corresponde a uma lista de 4 elementos sendo formatada da seguinte forma: **[X1, Y1, X2, Y2]**
X1 - Linha da peça que se vai mover;
Y1 - Coluna da peça que se vai mover;
X2 - Linha para onde a peça se vai mover;
Y2 - Coluna para onde a peça se vai mover;

A lista de Valid Moves é obtida através do predicado **valid_moves(+GameState, +Player, -ListOfMoves)** que realiza um findall de **[X1, Y1, X2, Y2]** 
com as condições para fazer a jogada no predicado **validmove(+GameState, +Player, -Move)**.

#### Execução de Jogadas
A jogada lida e validada anteriormente é usada no predicado **move(+GameState, +Move, -NewGameState)** e dentro do mesmo são realizados os predicados
**executeMove(+GameState, +Move, -NewGameState)** que muda o anterior Estado de Jogo para um novo Estado de Jogo, e o predicado **calcScore(+NewGameState, +Player)**
que calcula o Score do jogador no final daquela jogada.

**executeMove(+GameState, +Move, -NewGameState)** :
Pega no valor do move ([X1, Y1, X2, Y2]) e troca o valor do Board nas coordenadas X1 e Y2 para cor diferente (ex: p2 passa para b2), pois é sempre adicionado uma peça do adversário à casa de onde o jogador se move.
Troca também o valor nas coordenadas X2 e Y2 para cor diferente e adiciona 1 à stack (ex: b2 passa para p3).

#### Final de Jogo
O jogo termina através do predicado **game_over(+GameState, -Winner)** que verifica se a lista de ValidMoves de cada jogador está vazia ou se ambos os jogadores passaram a jogada no seu turno.

#### Avaliação do Tabuleiro
A avaliação do estado de jogo é feito através do predicado **value(+GameState, +Player, -Value)** que chama uma função que por sua vez itera todos os elementos do tabuleiro. Para cada elemento é chamada o predicado **createPath(Board, RowNumber, CollumnNumber, CurrentPath, BoardSize, 0, NewPath)** que recebe a posição do elemento no tabuleiro *(CollummNumber, RowNumber)*, verifica se o elemento faz parte do caminho atual de pontos do caminho e caso não faça adiciona-o a uma lista de **Point = [Elemento, CollumnNumber, RowNumber]**. De seguida o predicado verifica peças da mesma cor ortogonalmente adjacentes e para cada uma delas chama o predicado createPath. Após terem sido criados todos os caminhos a partir deste elemento, a lista de pontos é adicionada a uma lista de caminhos.
A seguir é calculado o score dessa lista de caminhos, somando todos os valores da stack dos elementos da lista de caminhos maior.

#### Jogada do Computador
O nosso trabalho tem dois níveis de dificuldade, um nível em que o computador é completamente aleatório e escolhe uma jogada de entre as ainda possíveis e outro, no qual o computador escolhe uma jogada baseada no quão mais alta vai ser a sua pontuação após tal jogada.
A jogada do computador nível 1 é executada pelo predicado **randomBot(Board, Turn, NewBoard)** que recebe o tabuleiro atual, cria uma lista de movimentos possíveis e escolhe um aleatóriamente.
A jogada do computador nível 2 é executada pelo predicado **smartBot(Board, Turn, NewBoard)** que tal como o random bot recebe o tabuleiro atual, cria uma lista de movimentos possíveis, calcula um tabuleiro novo para cada um desses movimentose calcula o value de cada um. O move que levar ao tabuleiro com maior valor é executada.

### Conclusões
A principio o trabalho parecia uma tarefa impossível de ser realizada pois não entendiamos bem o funcionamento desta nova linguagem PROLOG. Isto rapidamente mudou pois quando nos habituamos ao seu funcionamento conseguimos progredir com mais fluidez. Algumas funções que noutras linguagens eram facilmente criadas aqui provaram ser um grande desafio, damos o exemplo dos predicados value e move.
Esta cadeira também ajudou bastante na nossa aprendizagem relativamente a funções recursivas.
Conseguimos fazer tudo o que nos foi proposto, se bem que devido ao teste ser tão próximo da entrega do trabalho algumas coisas como uma melhor verificação de inputs ficou por fazer.
Em suma o trabalho foi um desafio extremamente interessante e ficamos bastante contentes com o resultado, tanto a nível visual pois entendemos que conseguimos criar uma excelente função de display, tanto a nível de estrutura de código, algo comprovado por exemplo pela velocidade das nossas funções.


### Referências
**https://www.swi-prolog.org/**
