# Relatório Intercalar
## MIEIC-PLOG

**Swack Grupo 5 turma.**
João Basto do Rosário (up201806334)
Jorge Levi Perdigoto da Costa (up201706518)

### Como executar o código:
1. Abrir o programa SICStus (Versão testada : 4.6.0)
2. Alterar a fonte do programa para MS Gothic (Ir a 'settings' -> 'font' -> selecionar 'MS Gothic')
3. Fazer consult do ficheiro `main.pl`
4. Escrever `play.`

#### Descrição do Jogo

**Swack** é um jogo de território jogado por dois jogadores, um controla as peças brancas e o outro as pretas e pode ser jogado num tabuleiro de qualquer tamanho 
No nosso caso o tamanho máximo será `12*12` e o mínimo será `3*3`.
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

#### Representação Interna do Estado de Jogo

**Disclaimer** Devido a termos descoberto que o tabuleiro poderia ser de multiplas dimensões a comlexidade de representar o board aumentou significativamente pelo que no momento de entrega
os boards usados são todos `9*9` e a função que lhes da display é 'hard coded', devido a isso, falta também identificar o tipo de cada peça nos predicados apresentados.

Cada um dos estados tem um predicado que representa a atual 'board'.
##### Estado Inicial
```prolog
initial([
  [1, 1, 1, 1, 1, 1, 1, 1, 1],
  [1, 1, 1, 1, 1, 1, 1, 1, 1],
  [1, 1, 1, 1, 1, 1, 1, 1, 1],
  [1, 1, 1, 1, 1, 1, 1, 1, 1],
  [1, 1, 1, 1, 1, 1, 1, 1, 1],
  [1, 1, 1, 1, 1, 1, 1, 1, 1],
  [1, 1, 1, 1, 1, 1, 1, 1, 1],
  [1, 1, 1, 1, 1, 1, 1, 1, 1],
  [1, 1, 1, 1, 1, 1, 1, 1, 1]
  ]).
```


##### Estado Intermédio
```prolog
mid([
  [2, 1, 3, 1, 1, 1, 4, 1, 1],
  [1, 1, 4, 2, 4, 1, 2, 3, 1],
  [1, 1, 2, 4, 1, 3, 1, 1, 1],
  [1, 3, 1, 1, 3, 1, 2, 1, 1],
  [3, 1, 1, 4, 1, 3, 1, 1, 1],
  [1, 4, 2, 2, 1, 1, 2, 2, 1],
  [1, 3, 1, 3, 3, 1, 2, 1, 1],
  [4, 1, 2, 1, 2, 1, 3, 1, 1],
  [1, 1, 3, 1, 1, 3, 1, 1, 4]
  ]).
```


##### Estado Final
```prolog
final([
  [2, 1, 3, 1, 2, 1, 4, 1, 1],
  [1, 1, 4, 2, 4, 1, 2, 3, 1],
  [2, 2, 2, 4, 1, 3, 1, 2, 2],
  [1, 3, 1, 1, 3, 1, 2, 1, 1],
  [3, 1, 1, 4, 1, 3, 1, 2, 2],
  [1, 4, 2, 2, 1, 1, 2, 3, 1],
  [1, 3, 1, 3, 3, 1, 2, 1, 1],
  [4, 2, 2, 1, 2, 1, 3, 1, 1],
  [1, 1, 3, 2, 1, 3, 1, 2, 4]
  ]).
```

#### Visualização do estado de jogo

Por ordem seguem se imagens da consola que representam cada um dos estados de jogos possíveis.

##### Estado Inicial
![Estado Inicial](https://github.com/Deadrosas/PLOG-Project/blob/main/resources/initial.PNG)
##### Estado Intermédio
![Estado Intermédio](https://github.com/Deadrosas/PLOG-Project/blob/main/resources/mid.PNG)
##### Estado Final
![Estado Final](https://github.com/Deadrosas/PLOG-Project/blob/main/resources/final.PNG)