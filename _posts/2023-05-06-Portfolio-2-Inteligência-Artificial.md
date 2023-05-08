---
title: Portfolio 2 - Inteligência Artificial
date: 2023-05-06 17:30:00 -0400 
categories: [portfolio - inteligência artificial, portfolio 2]
tags: [ai, portfolio, algoritmos de busca]
---

## Agente de soluções de problemas

Um agente de soluções de problemas é um agente inteligente que utiliza algoritmos e técnicas de inteligência artificial para resolver problemas específicos. Ele analisa o problema, gera soluções possíveis, avalia a eficácia de cada solução e seleciona a melhor opção.

A seguir, apresentamos um exemplo simples de uma classe Python que representa um agente de soluções de problemas genérico:

```python
class ProblemSolvingAgent:
    def __init__(self, problem):
        self.problem = problem

    def solve(self):
        raise NotImplementedError
```

Neste exemplo, a classe `ProblemSolvingAgent` possui um método `solve()` que deve ser implementado para resolver o problema específico ao qual o agente está destinado. Ao utilizar a classe `ProblemSolvingAgent` como base, é possível criar agentes especializados em diversos problemas, como o problema do caixeiro-viajante, a solução de labirintos ou problemas de otimização. Essa abordagem facilita a reutilização de código e a extensibilidade do sistema, permitindo a criação de novos agentes de solução de problemas de maneira simples e eficiente.

### Exemplo: Agente das oito rainhas

Um exemplo clássico de agente de soluções de problemas é o agente que resolve o problema das oito rainhas. Esse problema consiste em posicionar oito rainhas em um tabuleiro de xadrez 8x8 de tal forma que nenhuma delas ataque outra. 

A classe `EightQueensAgent` estende a classe base `ProblemSolvingAgent` e implementa o método `solve()` para resolver o problema das oito rainhas utilizando um algoritmo de backtracking.

```python
class EightQueensAgent(ProblemSolvingAgent):
    def solve(self):
        def is_safe(board, row, col):
            for i in range(col):
                if board[row][i] == 1:
                    return False

            for i, j in zip(range(row, -1, -1), range(col, -1, -1)):
                if board[i][j] == 1:
                    return False

            for i, j in zip(range(row, len(board), 1), range(col, -1, -1)):
                if board[i][j] == 1:
                    return False

            return True

        def solve_queens(board, col):
            if col >= len(board):
                return True

            for i in range(len(board)):
                if is_safe(board, i, col):
                    board[i][col] = 1

                    if solve_queens(board, col + 1):
                        return True

                    board[i][col] = 0

            return False

        n = 8
        board = [[0] * n for _ in range(n)]

        if not solve_queens(board, 0):
            return None

        return board

agent = EightQueensAgent(None)
solution = agent.solve()
for row in solution:
    print(row)
```

A função `is_safe()` verifica se é seguro posicionar uma rainha na posição atual do tabuleiro, enquanto a função `solve_queens()` utiliza backtracking para posicionar as rainhas no tabuleiro de forma que nenhuma delas se ataque. O método `solve()` da classe `EightQueensAgent` inicializa o tabuleiro e chama a função `solve_queens()` para encontrar uma solução.

Com base na explicação anterior do algoritmo de backtracking, é possível compreender o exemplo ilustrativo apresentado na animação. O objetivo do problema é posicionar um número determinado de damas em um tabuleiro de xadrez sem que elas se ataquem. Para isso, o algoritmo de backtracking começa posicionando uma dama em uma coluna que não cause conflito com as damas já posicionadas.

Caso não seja possível encontrar uma coluna segura para a dama atual, o algoritmo volta à última situação válida e tenta posicionar a dama em uma coluna diferente. Esse processo se repete até que todas as damas tenham sido posicionadas ou até que seja identificado que não é possível encontrar uma solução válida.

A animação ilustra esse processo de forma visual, mostrando como o algoritmo tenta posicionar cada dama em uma coluna segura e volta atrás caso não seja possível encontrar uma solução viável. Esse tipo de abordagem permite encontrar a solução para o problema de forma eficiente, sem precisar testar todas as combinações possíveis de posicionamento das damas no tabuleiro.

<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/1/1f/Eight-queens-animation.gif" alt="Animação do problema das oito rainhas">
  <br>Fonte: <a href="https://en.wikipedia.org/wiki/Eight_queens_puzzle">Wikipedia</a>
</div>



## Problemas de malha aberta e de malha fechada

- **Malha aberta**: Problemas de malha aberta são aqueles onde a solução é independente das ações do agente em relação ao ambiente. Um exemplo clássico é o problema do caixeiro-viajante, em que o objetivo é encontrar a menor rota para visitar um conjunto de cidades e retornar à cidade de origem.

```python
import itertools
from operator import itemgetter

cities = [(1, 2), (2, 4), (4, 4), (4, 1)]
dist = lambda a, b: ((a[0] - b[0]) ** 2 + (a[1] - b[1]) ** 2) ** 0.5
shortest_route = min(((sum(dist(a, b) for a, b in zip(route, route[1:] + [route[0]])), route) for route in itertools.permutations(cities)), key=itemgetter(0))[1]
print(shortest_route)
```

Neste exemplo em Python, o código resolve o problema do caixeiro-viajante encontrando a rota mais curta para um conjunto de cidades usando apenas algumas linhas de código.

- **Malha fechada**: Problemas de malha fechada são aqueles onde a solução depende das ações do agente e do ambiente, havendo interação e feedback constante entre eles. Um exemplo clássico é o jogo da velha, onde a solução depende das ações do agente e do adversário.

```python
from itertools import product

def game_result(board):
    for row, col, diag in product([0, 1, 2], repeat=2):
        if board[row][0] == board[row][1] == board[row][2] or board[0][col] == board[1][col] == board[2][col] or board[0][0] == board[1][1] == board[2][2] or board[0][2] == board[1][1] == board[2][0]:
            return board[row][col]
    return "Draw" if all(cell != 0 for row in board for cell in row) else None

board = [[1, 2, 0], [2, 1, 0], [2, 1, 1]]
print(game_result(board))
```

Neste exemplo em Python, o código verifica o resultado do jogo da velha em um tabuleiro 3x3 usando apenas algumas linhas de código. A solução deste problema envolve a interação entre os jogadores, com feedback constante do estado do tabuleiro, que é afetado pelas ações de ambos os agentes.

## Algoritmos de busca

### Busca cega

Os algoritmos de busca cega, também conhecidos como busca não-informada, não utilizam informações específicas do problema para tomar decisões. Aqui estão alguns algoritmos de busca cega comuns:

#### Depth First Search (DFS)

O DFS explora um caminho até o fim antes de retornar e explorar outros caminhos. Ele se aprofunda em um ramo do espaço de busca até que não haja mais nós a serem explorados ou até encontrar a solução.

```python
def depth_first_search(problem):
    frontier = [problem.initial_state()]
    explored = set()

    while frontier:
        node = frontier.pop()
        if problem.is_goal(node):
            return node
        explored.add(node)
        for child in problem.successors(node):
            if child not in explored and child not in frontier:
                frontier.append(child)
    return None
```

#### Breadth First Search (BFS)

O BFS explora todos os nós em um nível antes de passar para o próximo. Isso significa que ele explora todos os nós possíveis no nível atual antes de se aprofundar no próximo nível do espaço de busca.

```python
from collections import deque

def breadth_first_search(problem):
    frontier = deque([problem.initial_state()])
    explored = set()

    while frontier:
        node = frontier.popleft()
        if problem.is_goal(node):
            return node
        explored.add(node)
        for child in problem.successors(node):
            if child not in explored and child not in frontier:
                frontier.append(child)
    return None
```

#### Uniform Cost Search (UCS)

O UCS explora os nós com menor custo acumulado primeiro. Ele é útil para encontrar soluções de menor custo em problemas onde o custo de cada ação é relevante.

```python
import heapq

def uniform_cost_search(problem):
    frontier = [(0, problem.initial_state())]
    explored = set()

    while frontier:
        cost, node = heapq.heappop(frontier)
        if problem.is_goal(node):
            return node
        explored.add(node)
        for child, action_cost in problem.successors_with_costs(node):
            new_cost = cost + action_cost
            if child not in explored and child not in (n for _, n in frontier):
                heapq.heappush(frontier, (new_cost, child))
            elif child in (n for _, n in frontier):
                for i, (old_cost, old_node) in enumerate(frontier):
                    if old_node == child and old_cost > new_cost:
                        frontier[i] = (new_cost, child)
                        heapq.heapify(frontier)
    return None
```

#### Best First Search (BFS)

O Best First Search (não confundir com Breadth First Search) seleciona o próximo nó a ser explorado com base em uma função de avaliação. A busca Best First pode ser considerada cega se a função de avaliação não levar em conta informações específicas do problema.

```python
import heapq

def best_first_search(problem, evaluation_function):
    frontier = [(evaluation_function(problem.initial_state()), problem.initial_state())]
    explored = set()

    while frontier:
        _, node = heapq.heappop(frontier)
        if problem.is_goal(node):
            return node
        explored.add(node)
        for child in problem.successors(node):
            if child not in explored and child not in (n for _, n in frontier):
                heapq.heappush(frontier, (evaluation_function(child), child))
            elif child in (n for _, n in frontier):
                for i, (old_value, old_node) in enumerate(frontier):
                    if old_node == child and evaluation_function(child) < old_value:
                        frontier[i] = (evaluation_function(child), child)
                        heapq.heapify(frontier)
    return None
```

Neste exemplo, a busca Best First utiliza uma função de avaliação para decidir qual nó explorar a seguir. Se a função de avaliação não usar informações específicas do problema, a busca pode ser considerada cega.

Esses quatro algoritmos são exemplos de busca cega com diferentes estratégias de exploração. A escolha do método de busca apropriado depende das características e requisitos do problema específico.

Ao escolher um algoritmo de busca cega, é importante considerar os requisitos e as características específicas do problema em questão. Cada algoritmo tem suas vantagens e desvantagens, e a seleção do método de busca apropriado pode ter um impacto significativo no desempenho e na qualidade da solução.

Suponha que estamos trabalhando com um problema de roteamento, em que o objetivo é encontrar o caminho mais curto entre dois pontos em um mapa. Vamos analisar os quatro algoritmos de busca cega mencionados e como cada um deles pode ser usado neste contexto.

1. **Depth First Search (DFS)**: O DFS pode ser útil se você deseja explorar rapidamente o espaço de busca, mas não garante que encontrará o caminho mais curto. Pode levar a soluções mais longas e, em casos extremos, pode não encontrar uma solução.

2. **Breadth First Search (BFS)**: O BFS garante que encontraremos o caminho mais curto, pois explora todos os caminhos em paralelo, expandindo-se em camadas. No entanto, ele consome mais memória, já que armazena todos os nós de um nível na memória antes de explorar o próximo nível.

3. **Uniform Cost Search (UCS)**: O UCS é particularmente adequado para esse problema, pois explora os nós com menor custo acumulado primeiro, garantindo que encontraremos o caminho de menor custo. É uma escolha melhor do que o DFS e o BFS para problemas onde o custo de cada ação é relevante.

4. **Best First Search (BFS)**: Se a função de avaliação não usar informações específicas do problema, a busca pode ser considerada cega. No entanto, em problemas de roteamento, é comum usar uma função heurística que considera a distância em linha reta entre o nó atual e o destino como função de avaliação. Nesse caso, a busca Best First se torna uma busca informada e pode ser mais eficiente do que os outros métodos de busca cega.

Nesse exemplo, o Uniform Cost Search é provavelmente a melhor escolha entre os algoritmos de busca cega, pois ele garante encontrar o caminho de menor custo. No entanto, se você estiver disposto a usar uma função heurística, a busca Best First pode ser ainda mais eficiente.

Em resumo, a escolha do algoritmo de busca cega apropriado depende das características e requisitos do problema específico. É essencial analisar cuidadosamente o problema e entender as propriedades de cada algoritmo antes de tomar uma decisão.

### Busca informada

A busca informada utiliza informações específicas do problema para guiar a busca. As funções heurísticas são usadas para estimar o custo para alcançar o objetivo. Algoritmos comuns de busca informada incluem A* e Busca Gulosa.

#### Funções Heurísticas

Funções heurísticas são usadas para estimar o custo do estado atual até o objetivo. Elas fornecem uma maneira de priorizar a exploração do espaço de busca, levando em conta o conhecimento específico do problema. Um exemplo clássico é o problema do caminho mais curto, onde a distância em linha reta entre o nó atual e o destino pode ser usada como função heurística.

```python
def heuristic_function(state):
    # implementação da função heurística
```

#### A* Search

O algoritmo A* é um exemplo popular de busca informada. Ele combina os custos reais do caminho com os custos estimados pela função heurística para determinar a próxima ação a ser tomada. O A* é uma generalização da busca de custo uniforme e é ótimo quando a função heurística é admissível e consistente.

```python
import heapq

def a_star_search(problem, heuristic_function):
    def f_cost(g_cost, state):
        return g_cost + heuristic_function(state)

    frontier = [(f_cost(0, problem.initial_state()), 0, problem.initial_state())]
    explored = set()

    while frontier:
        _, g_cost, node = heapq.heappop(frontier)
        if problem.is_goal(node):
            return node
        explored.add(node)
        for child, action_cost in problem.successors_with_costs(node):
            new_g_cost = g_cost + action_cost
            if child not in explored and child not in (n for _, _, n in frontier):
                heapq.heappush(frontier, (f_cost(new_g_cost, child), new_g_cost, child))
            elif child in (n for _, _, n in frontier):
                for i, (old_f_cost, old_g_cost, old_node) in enumerate(frontier):
                    if old_node == child and new_g_cost < old_g_cost:
                        frontier[i] = (f_cost(new_g_cost, child), new_g_cost, child)
                        heapq.heapify(frontier)
    return None
```

#### Busca Gulosa

A Busca Gulosa, também conhecida como Best-First Search, é um algoritmo de busca informada que utiliza apenas a função heurística para guiar a busca. Diferentemente do A*, a Busca Gulosa não leva em consideração o custo real do caminho percorrido até o estado atual.

```python
import heapq

def greedy_best_first_search(problem, heuristic_function):
    frontier = [(heuristic_function(problem.initial_state()), problem.initial_state())]
    explored = set()

    while frontier:
        _, node = heapq.heappop(frontier)
        if problem.is_goal(node):
            return node
        explored.add(node)
        for child in problem.successors(node):
            if child not in explored and child not in (n for _, n in frontier):
                heapq.heappush(frontier, (heuristic_function(child), child))
            elif child in (n for _, n in frontier):
                for i, (old_value, old_node) in enumerate(frontier):
                    if old_node == child and heuristic_function(child) < old_value:
                        frontier[i] = (heuristic_function(child), child)
                        heapq.heapify(frontier)
    return None
```

Em resumo, a busca informada utiliza informações específicas do problema para guiar a busca, enquanto a busca cega não. Algoritmos como A* e Busca Gulosa são exemplos de busca informada que usam funções heurísticas para estimar o custo do estado atual até o objetivo. A escolha do algoritmo de busca informada apropriado depende das características e requisitos do problema específico, assim como no caso da busca cega. Analisar cuidadosamente o problema e entender as propriedades de cada algoritmo são essenciais antes de tomar uma decisão.

Ao escolher um algoritmo de busca informada, é importante levar em consideração as características específicas do problema e a qualidade da função heurística disponível. Cada algoritmo possui suas vantagens e desvantagens, e a seleção do método de busca apropriado pode ter um impacto significativo no desempenho e na qualidade da solução.

Vamos considerar um problema de planejamento de rota em um mapa 2D com obstáculos, onde o objetivo é encontrar o caminho mais curto entre dois pontos. Neste contexto, podemos analisar os dois algoritmos de busca informada mencionados e como cada um deles pode ser usado.

1. **A* Search**: O algoritmo A* é uma boa opção para esse problema, pois combina o custo real do caminho percorrido com o custo estimado pela função heurística. Ao utilizar a distância em linha reta entre o nó atual e o destino como função heurística, o A* garante que encontraremos o caminho mais curto, desde que a função heurística seja admissível e consistente. Além disso, o A* tende a ser eficiente na exploração do espaço de busca.

2. **Busca Gulosa (Greedy Best-First Search)**: A Busca Gulosa é outra opção, mas pode ser menos eficiente do que o A* para esse problema. A busca gulosa utiliza apenas a função heurística para guiar a busca, sem levar em consideração o custo real do caminho percorrido. Isso pode resultar em soluções subótimas ou até mesmo em um fracasso ao encontrar um caminho, se a função heurística não for suficientemente informativa.

Neste exemplo, o algoritmo A* é provavelmente a melhor escolha entre os algoritmos de busca informada, pois ele garante encontrar o caminho mais curto e é eficiente na exploração do espaço de busca. No entanto, a Busca Gulosa pode ser útil em casos em que a função heurística é muito informativa e a necessidade de encontrar a solução ótima não é crítica.

Em resumo, a escolha do algoritmo de busca informada apropriado depende das características e requisitos do problema específico, bem como da qualidade da função heurística disponível. É essencial analisar cuidadosamente o problema e entender as propriedades de cada algoritmo antes de tomar uma decisão.

### Busca em ambientes complexos

Ambientes complexos apresentam desafios adicionais, como informações incompletas, restrições de tempo ou espaço. Vamos explorar como as funções heurísticas e algoritmos de busca local podem ser aplicados em ambientes complexos e discutir outras abordagens para lidar com esses desafios.

#### Funções heurísticas

Funções heurísticas são usadas para estimar o custo do estado atual até o objetivo. Por exemplo, no problema do 8-puzzle, duas funções heurísticas comuns são o número de peças mal posicionadas e a soma das distâncias das peças até a posição de objetivo.

```python
def misplaced_tiles(state, goal):
    return sum(s != g for s, g in zip(state, goal) if s != 0)

def manhattan_distance(state, goal, size):
    distance = 0
    for s, g in zip(state, goal):
        if s != 0 and s != g:
            distance += abs(s // size - g // size) + abs(s % size - g % size)
    return distance
```

A utilização de uma boa função heurística admissível pode melhorar significativamente a eficiência de algoritmos de busca informada, como o A*, especialmente em ambientes complexos com um grande número de estados possíveis.

#### Busca local

Algoritmos de busca local executam a busca puramente local no espaço de estados, avaliando e modificando um ou mais estados atuais, em vez de explorar sistematicamente caminhos a partir do estado inicial. Esses algoritmos são voltados para problemas em que o que importa é o estado da solução, e não o custo do caminho para alcançá-lo.

Um exemplo de algoritmo de busca local é o Hill-climbing search, que é um loop simples que continua a mover-se na direção de incrementar o valor da função objetivo. Ele finaliza quando alcança um pico, onde nenhum vizinho tem valores maiores.

```python
def hill_climbing_search(problem, initial_state):
    current_state = initial_state
    while True:
        neighbors = problem.get_neighbors(current_state)
        next_state = max(neighbors, key=lambda x: problem.value(x))
        if problem.value(next_state) <= problem.value(current_state):
            return current_state
        current_state = next_state
```

Há variações do Hill-climbing search, como Stochastic hill climbing, First-choice hill climbing, Random-restart hill climbing e Local Beam Search, que podem ser aplicadas para lidar com os desafios específicos de ambientes complexos, como máximas locais, cumes e platôs.

Ao lidar com ambientes complexos, é crucial entender os desafios específicos do problema e adaptar ou selecionar algoritmos de busca apropriados para enfrentá-los. Funções heurísticas e algoritmos de busca local podem oferecer soluções mais eficientes e de melhor qualidade, especialmente em cenários com informações incompletas, restrições de tempo ou espaço.

### Algoritmos Genéticos

Os algoritmos genéticos (AGs) são métodos de busca inspirados nos processos de seleção natural e evolução. Eles utilizam técnicas como seleção, cruzamento (crossover) e mutação para explorar e otimizar soluções para problemas complexos. Os AGs são especialmente úteis para resolver problemas de otimização e busca global em espaços de soluções de grande dimensão.

```python
class GeneticAlgorithm:
    def __init__(self, population, fitness_function):
        self.population = population
        self.fitness_function = fitness_function

    def evolve(self):
        # implementação do algoritmo
```

#### Exemplo: Problema da Mochila

O problema da mochila é um exemplo clássico de problema de otimização combinatória. Neste problema, temos um conjunto de itens, cada um com um peso e um valor. O objetivo é selecionar um subconjunto desses itens de modo que o valor total dos itens selecionados seja maximizado, sem exceder a capacidade da mochila.

Para resolver o problema da mochila com algoritmos genéticos, podemos representar cada solução como um cromossomo, onde cada gene corresponde à presença (1) ou ausência (0) de um item na mochila. A função de aptidão (fitness) avalia a qualidade da solução, levando em consideração o valor total dos itens e a restrição de capacidade da mochila.

Suponha que temos uma mochila com capacidade para 50 kg e cinco itens disponíveis:

| Item | Peso (kg) | Valor |
|------|-----------|-------|
| A    | 10        | 60    |
| B    | 20        | 100   |
| C    | 30        | 120   |
| D    | 40        | 200   |
| E    | 50        | 300   |

Uma solução pode ser representada como um cromossomo, como `[1, 0, 1, 0, 0]`, o que indica que os itens A e C foram selecionados. A função de aptidão poderia ser a soma dos valores dos itens selecionados, desde que o peso total não exceda a capacidade da mochila.

Durante o processo de evolução, os AGs aplicam os seguintes passos:

1. **Seleção**: Escolha dos pais que participarão do cruzamento, com base em suas aptidões. Os pais com maior aptidão têm maior probabilidade de serem selecionados.

2. **Cruzamento**: Criação de novos indivíduos (descendentes) a partir dos pais selecionados, combinando seus genes. Isso pode ser feito por meio de diferentes técnicas de cruzamento, como o cruzamento de um ponto ou o cruzamento uniforme.

3. **Mutação**: Alteração aleatória de alguns genes dos descendentes, com uma probabilidade baixa. A mutação ajuda a manter a diversidade genética na população e a explorar novas soluções.

4. **Substituição**: Substituição da população atual pela nova geração de descendentes, geralmente mantendo alguns dos melhores indivíduos da geração anterior.

O processo de evolução é repetido por um número predeterminado de gerações ou até que uma solução satisfatória seja encontraria.

Neste exemplo, o algoritmo genético inicia com uma população aleatória de cromossomos. A cada geração, os melhores indivíduos têm maior chance de serem selecionados como pais. Os pais são cruzados e seus descendentes sofrem mutações, o que resulta em uma nova população.

Após várias gerações, a população tende a convergir para soluções de alta qualidade. No caso do problema da mochila, isso significa soluções que maximizam o valor total dos itens selecionados, respeitando a restrição de capacidade da mochila. Por exemplo, após a evolução, o algoritmo genético pode encontrar uma solução como `[0, 1, 1, 0, 0]`, que seleciona os itens B e C, com um valor total de 220 e um peso total de 50 kg.

Os algoritmos genéticos são uma abordagem eficaz para resolver problemas de otimização e busca em ambientes complexos, especialmente quando o espaço de soluções é grande e as soluções exatas são difíceis de encontrar. A escolha adequada da função de aptidão, dos métodos de seleção, cruzamento e mutação, e dos parâmetros do algoritmo (como tamanho da população e taxa de mutação) é essencial para garantir a eficiência e eficácia do algoritmo genético na resolução do problema específico.

## Discussões

### Algoritmos de procura não discutidos em sala

### Tabu Search

Tabu Search é um algoritmo de busca local meta-heurístico que explora o espaço de soluções utilizando movimentos locais. Ele mantém uma lista chamada "tabu" para armazenar soluções visitadas recentemente, evitando que o algoritmo retorne a elas e caia em ciclos. Essa abordagem ajuda a escapar de ótimos locais e pode encontrar soluções de melhor qualidade.

Exemplo: Em um problema de agendamento de tarefas em máquinas, o Tabu Search pode ser usado para encontrar uma solução aproximada, movendo-se localmente no espaço de soluções e evitando ciclos, através do uso da lista tabu.

### Simulated Annealing

Simulated Annealing é um algoritmo de busca global inspirado no processo físico chamado recozimento. Ele aceita soluções piores com probabilidade decrescente, permitindo escapar de ótimos locais e, eventualmente, convergir para o ótimo global. É útil para problemas com muitos ótimos locais.

Exemplo: Em um problema de otimização de layout de circuitos, o Simulated Annealing pode ser usado para encontrar uma disposição de componentes que minimize o comprimento total das conexões, permitindo temporariamente soluções piores para escapar de ótimos locais.

### Expansão do uso de algoritmos de procura

- **Jogos**: Algoritmos de busca, como minimax e alfa-beta pruning, são usados para modelar a tomada de decisões em jogos competitivos, como xadrez e Go.

- **Planejamento**: Algoritmos de busca, como Dijkstra e A*, são usados para planejar rotas em sistemas de transporte e robótica.

- **Otimização**: Algoritmos de busca, como algoritmos genéticos e simulated annealing, são usados para resolver problemas de otimização combinatorial, como o problema do caixeiro viajante e o problema da mochila.

### Expansão sobre algoritmos genéticos

- **Algoritmos genéticos com restrições**: Algoritmos genéticos podem ser adaptados para resolver problemas com restrições específicas, utilizando técnicas como penalidades ou operadores de reparo.

- **Algoritmos genéticos multiobjetivo**: Algoritmos genéticos podem ser adaptados para resolver problemas com múltiplos objetivos, utilizando conceitos como dominância de Pareto e seleção não-dominada.

### Uso de algoritmos de busca em IA

- **Navegação de robôs**: Navegação de robôs: Algoritmos de busca, como A* e D*, são usados para planejar o caminho de robôs móveis em ambientes complexos e dinâmicos. Esses algoritmos permitem que os robôs encontrem rotas ótimas ou subótimas, evitando obstáculos e minimizando o custo de movimento. Um exemplo de aplicação é o robô Shakey, que foi um dos primeiros a usar a busca A* para navegar em um ambiente desconhecido.

<div align="center">
  <img src="https://www.robot-magazine.fr/wp-content/uploads/2022/07/21434.jpg" alt="Robô Shakey" width="100%">
  <br>Fonte: <a href="https://www.robot-magazine.fr/lhomme-le-mythe-la-legende-shakey-le-robot-le-premier-robot-au-monde-base-sur-lia-1/">Robot Magazine</a>
</div>

- **Reconhecimento de padrões**: Algoritmos de busca, como k-Nearest Neighbors (k-NN) e Support Vector Machines (SVM), são usados para classificar dados em problemas de reconhecimento de padrões, como reconhecimento de dígitos escritos à mão e análise de sentimentos.

## Projetos e problemas

### Algoritmo de busca informada: Dijkstra's Algorithm

**Exemplo:** Encontrar o caminho mais curto em um grafo ponderado.

O algoritmo de Dijkstra é um algoritmo de busca informada que encontra o caminho mais curto entre um nó inicial e todos os outros nós em um grafo ponderado. Ele é garantido para encontrar a solução ótima. No exemplo a seguir, o algoritmo Dijkstra é aplicado para encontrar o menor custo de deslocamento entre dois pontos em um mapa.

```python
import heapq

def dijkstra(graph, start):
    distances = {node: float('infinity') for node in graph}
    distances[start] = 0
    pq = [(0, start)]

    while pq:
        current_distance, current_node = heapq.heappop(pq)

        if current_distance > distances[current_node]:
            continue

        for neighbor, weight in graph[current_node].items():
            distance = current_distance + weight

            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))

    return distances
```

### Algoritmo de busca cega: Iterative Deepening Depth-First Search (IDDFS)

**Exemplo:** Encontrar um objetivo em um grafo não ponderado, usando IDDFS.

O IDDFS combina DFS e BFS, realizando uma busca em profundidade limitada e aumentando o limite a cada iteração até encontrar o objetivo. No exemplo a seguir, o IDDFS é usado para encontrar o caminho mais curto entre dois pontos em um grafo não ponderado.

```python
def iddfs(graph, start, goal):
    depth = 0
    while True:
        result = dls(graph, start, goal, depth)
        if result == "found":
            return f"Goal found at depth {depth}"
        depth += 1

def dls(graph, node, goal, depth):
    if depth == 0 and node == goal:
        return "found"
    if depth > 0:
        for neighbor in graph[node]:
            result = dls(graph, neighbor, goal, depth - 1)
            if result == "found":
                return "found"
    return "not found"
```

### Exemplo de uso de algoritmos genéticos: Otimização de funções

**Exemplo:** Encontrar o mínimo global da função Rastrigin, uma função não-convexa com muitos ótimos locais.

Algoritmos genéticos podem ser aplicados para otimizar funções matemáticas. No exemplo a seguir, um algoritmo genético é usado para encontrar o mínimo global da função Rastrigin, uma função não-convexa com muitos ótimos locais.

```python
import numpy as np

def rastrigin_function(x):
    return 10 * len(x) + np.sum(x ** 2 - 10 * np.cos(2 * np.pi * x))

class FunctionOptimization(GeneticAlgorithm):
    def __init__(self, population, fitness_function):
        super().__init__(population, fitness_function)

    # Implementação de seleção, cruzamento e mutação específicos para otimização de funções

# Exemplo de uso
population = np.random.uniform(-5.12, 5.12, (50, 10))
optimizer = FunctionOptimization(population, rastrigin_function)
optimizer.evolve()
```

O algoritmo genético FunctionOptimization busca soluções no espaço de busca da função Rastrigin, selecionando indivíduos com base em sua aptidão (valor da função), realizando cruzamento e mutação para gerar novos indivíduos e, eventualmente, convergindo para uma solução próxima ao mínimo global.

## Referências
- NORVIG, P.; RUSSELL, S., Inteligência artificial: Tradução da 3a Edição, [s.l.]: Elsevier Brasil, 2014.
- [Algoritmo genético, in: Wikipédia, a enciclopédia livre, [s.l.: s.n.], 2023.](https://pt.wikipedia.org/wiki/Algoritmo_gen%C3%A9tico)
- [Problema das oito damas, in: Wikipédia, a enciclopédia livre, [s.l.: s.n.], 2023.](https://pt.wikipedia.org/wiki/Problema_das_oito_damas)
- [Algoritmos de Busca para Inteligência Artificial - Medium.](https://ricardomatsumura.medium.com/algoritmos-de-busca-para-inteligência-artificial-7cb81172396c)
- [GRANATYR, Jones, Shakey: Primeiro robô com Inteligência Artificial, IA Expert Academy](https://iaexpert.academy/2017/04/28/shakey-primeiro-robo-com-inteligencia-artificial/)
- [Reconhecimento de padrões | Introdução – Acervo Lima](https://acervolima.com/reconhecimento-de-padroes-introducao/)