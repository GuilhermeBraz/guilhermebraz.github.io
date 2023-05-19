---
title: Portfolio 3 - Inteligência Artificial
date: 2023-05-18 17:30:00 -0400 
categories: [portfolio - inteligência artificial, portfolio 3]
tags: [ai, portfolio, Problemas de Satisfação de Condições]
---

## 1. Representação Atômica vs. Fatorada

A **representação atômica** e a **representação fatorada** são duas abordagens fundamentais para lidar com Problemas de Satisfação de Condições (CSPs).

Na representação atômica, cada estado é tratado como uma caixa preta indivisível, sem nenhuma estrutura interna reconhecível. Este tipo de representação é simples, mas pode não capturar todas as nuances de um estado.

Em contraste, na representação fatorada, os estados são quebrados em variáveis ou "fatores". Cada fator tem seu próprio conjunto de valores possíveis, permitindo uma visão mais granular do estado. Essa abordagem permite uma representação mais expressiva e pode tornar a resolução do CSP mais eficiente, já que a solução pode ser encontrada por meio da resolução de subproblemas menores.

## 2. Definindo Problemas de Satisfação de Condições (CSPs)

Os Problemas de Satisfação de Condições (CSPs) são um tipo de problema computacional em que se procura encontrar estados que satisfaçam uma série de restrições. Cada CSP é composto por um conjunto de variáveis, um conjunto de domínios (um para cada variável) e um conjunto de restrições.

## 3. Tipos de Condições

Existem diferentes tipos de condições ou restrições em CSPs:

- **Restrições Unárias**: Envolve uma única variável.
- **Restrições Binárias**: Envolve duas variáveis.
- **Restrições n-árias**: Envolve n variáveis.

Por vezes, para simplificar a resolução do problema, restrições n-árias podem ser transformadas em um conjunto de restrições binárias através do uso de variáveis auxiliares.

## 4. Consistência

A consistência é avaliada em diferentes níveis para verificar a solvabilidade de um CSP:

- **Consistência de Nó**: Verifica se cada variável pode satisfazer suas restrições individualmente.
- **Consistência de Arco**: Verifica se cada par de variáveis pode satisfazer suas restrições conjuntamente.
- **Consistência de Trajeto**: Estende a consistência de arco para sequências de variáveis.
- **Consistência k**: Generaliza a consistência de arco para um conjunto de k variáveis.
- **Consistência Global**: Verifica se o CSP completo pode satisfazer todas as suas restrições.

É importante notar que a consistência de arco é fundamental para a resolução de CSPs, e várias técnicas, como o algoritmo AC-3, são usadas para alcançar essa consistência.

## 5. Algoritmos

A escolha do algoritmo para resolver um CSP pode variar dependendo do problema. Aqui estão três dos algoritmos mais comumente utilizados:

- **Backtracking**: Este é um método de busca em profundidade que tenta resolver o problema atribuindo valores às variáveis uma a uma, voltando atrás (daí o nome) quando uma atribuição não leva a uma solução. É um algoritmo recursivo que pode ser aprimorado com heurísticas de seleção de variáveis e ordenação de valores.

- **Forward Checking**: Este método envolve verificar restrições à medida que atribuímos valores às variáveis, reduzindo assim o espaço de busca. Quando atribuímos um valor a uma variável, olhamos adiante e removemos do domínio das outras variáveis quaisquer valores que causem uma inconsistência.

- **Propagação de Restrições (AC-3)**: Este algoritmo usa a ideia de consistência de arco para reduzir o tamanho do domínio de cada variável. Em cada passo, verifica todas as restrições e remove valores do domínio de cada variável que não possam participar de uma solução. O AC-3 pode ser combinado com o algoritmo de backtracking para melhorar a eficiência da resolução.

## 6. Estrutura de Problemas

Ao resolver problemas de satisfação de restrições (CSPs), a estrutura do problema é representada comumente como um grafo, onde os nós são as variáveis e as arestas são as restrições entre as variáveis.

### 6.1. Estrutura de Árvore

Problemas com estrutura de árvore são geralmente mais simples de resolver. Uma sequência de variáveis se dispõe de tal forma que cada variável, excluindo a primeira, tem exatamente uma restrição a outra variável precedente na sequência. A solução para tais problemas pode ser obtida em tempo linear, relacionando-se diretamente ao número de variáveis.

Exemplo:

```
    A
   / \
  B   C
 / \ / \
D   E   F
```

### 6.2. Estrutura de Grafo

Problemas com uma estrutura de grafo mais complexa exigem estratégias de resolução mais elaboradas. As variáveis podem ter múltiplas restrições entre si, o que, por vezes, exige algoritmos de busca como Backtracking ou Local Search.

Exemplo:

```
    A - B
   / \ / \
  C - D - E
   \ / \ /
    F - G
```

### 6.3. Grafo Bipartido

Nessa estrutura, as variáveis podem ser divididas em dois conjuntos com todas as restrições sendo entre variáveis de conjuntos diferentes.

Exemplo:

```
  A - 1
  B - 2
  C - 3
```

### 6.4. Grafo em Grade

Essa estrutura é comum em muitos problemas práticos. Por exemplo, no problema Sudoku, as células do tabuleiro formam uma grade, e as restrições são entre células que estão na mesma linha, na mesma coluna, ou na mesma região de 3x3.

Exemplo:

```
  A - B - C
  |   |   |
  D - E - F
  |   |   |
  G - H - I
```

## 7. Discussões:

### Algoritmos de Satisfação de Condições não discutidos em sala

#### Simulated Annealing

Simulated Annealing (SA) é um algoritmo de busca local probabilístico inspirado no processo de recozimento em metalurgia. Na metalurgia, o recozimento é um processo de resfriamento lento usado para reduzir defeitos em um material e alcançar um estado de menor energia.

No contexto de satisfação de restrições, podemos usar o SA para minimizar uma função de custo que representa o número de restrições violadas. Inicializamos o algoritmo com uma solução aleatória e tentamos realizar pequenas mudanças que reduzem o número de restrições violadas.

Aqui está um exemplo simples de como implementar o SA em Python:

```python
import random
import math
import sys

def simulated_annealing(problem, schedule):
    current = problem.random_initial_state()
    for t in range(sys.maxsize):
        T = schedule(t)
        if T == 0:
            return current
        neighbors = problem.get_neighbors(current)
        next_state = random.choice(neighbors)
        delta_E = problem.value(next_state) - problem.value(current)
        if delta_E > 0 or random.uniform(0, 1) < math.exp(delta_E / T):
            current = next_state
    return current
```

Nesse código, `problem` representa o CSP que estamos tentando resolver, `schedule` é uma função que determina a taxa de resfriamento (geralmente uma função exponencial decrescente), `problem.random_initial_state()` retorna uma solução inicial aleatória, `problem.get_neighbors(current)` retorna todas as soluções vizinhas do estado atual, e `problem.value(state)` retorna a função de custo para um estado específico.

### Expansão do uso de algoritmos de satisfação de condições

O campo de CSPs está em rápida expansão. As empresas estão utilizando CSPs para alocar recursos de maneira eficiente, para agendar tarefas complexas e para otimizar rotas de logística. Em bioinformática, os CSPs estão sendo usados para modelar e resolver problemas relacionados à estrutura de proteínas e ao sequenciamento de DNA.

### Exemplos Práticos: Problema do Sudoku

Um exemplo prático de CSP é o problema do Sudoku. As variáveis são as células não preenchidas, cada variável tem um domínio de 1 a 9 e as restrições são que nenhum número pode ser repetido em uma linha, coluna ou sub-grade de 3x3. Usando o algoritmo de propagação de restrições AC-3 mencionado anteriormente, é possível criar um programa Python que resolva qualquer quebra-cabeça de Sudoku.

O código Python que resolve o problema do Sudoku usando o algoritmo AC-3 pode ser representado da seguinte maneira:

```python
import sys

def ac3(csp):
    queue = [(Xi, Xj) for Xi in csp.variables for Xj in csp.neighbors[Xi]]
    while queue:
        (Xi, Xj) = queue.pop(0)
        if revise(csp, Xi, Xj):
            if not csp.domains[Xi]:
                return False
            for Xk in csp.neighbors[Xi]:
                if Xk != Xj:
                    queue.append((Xk, Xi))
    return True

def revise(csp, Xi, Xj):
    revised = False
    for x in csp.domains[Xi]:
        if not any(csp.is_consistent(Xi, x, Xj, y) for y in csp.domains[Xj]):
            csp.domains[Xi].remove(x)
            revised = True
    return revised
```

Aqui, `ac3` é a função principal do algoritmo, que aceita um CSP como entrada. `revise` é uma função auxiliar que tenta reduzir o domínio da variável `Xi` removendo valores que não são consistentes com a variável `Xj`.

## 8. Projetos e Problemas

### Implementação em código Python de algoritmos e/ou problemas não discutidos em sala

Para um problema mais exótico e menos típico, vamos considerar o problema da Colmeia de Abelhas.

#### Problema da Colmeia de Abelhas

O problema da Colmeia de Abelhas é um CSP em que a colmeia é representada como um gra

fo e cada abelha é uma variável que deve ser atribuída a um nó no grafo. A restrição é que duas abelhas não podem ser atribuídas ao mesmo nó. Este problema pode ser resolvido por vários algoritmos de CSP, incluindo Backtracking e Local Search.

Aqui está um exemplo de como implementar o algoritmo Backtracking para resolver este problema em Python:

```python
def backtrack(assignment, csp):
    if len(assignment) == len(csp.variables):
        return assignment
    var = select_unassigned_variable(assignment, csp)
    for value in order_domain_values(var, assignment, csp):
        if csp.is_consistent(var, value, assignment):
            csp.assign(var, value, assignment)
            result = backtrack(assignment, csp)
            if result is not None:
                return result
            csp.unassign(var, assignment)
    return None

def select_unassigned_variable(assignment, csp):
    for var in csp.variables:
        if var not in assignment:
            return var
    return None

def order_domain_values(var, assignment, csp):
    return csp.domains[var]
```

Neste código, `backtrack` é a função principal que aceita uma atribuição atual e um CSP como entrada e retorna uma solução para o problema ou `None` se nenhuma solução for encontrada. `select_unassigned_variable` é uma função que seleciona a próxima variável a ser atribuída, e `order_domain_values` simplesmente retorna os possíveis valores para uma determinada variável (neste caso, não estamos implementando qualquer heurística específica para ordenar os valores, mas em uma implementação mais avançada, você pode querer fazer isso).

Para experimentar esses algoritmos, você precisará definir suas próprias classes CSP que representem os problemas do Sudoku e da Colmeia de Abelhas, incluindo a definição das variáveis, domínios e restrições, além de definir o que significa uma atribuição ser consistente.

Lembrando sempre, para obter uma compreensão mais profunda dos CSPs, é aconselhável experimentar a implementação de diferentes problemas e algoritmos, além de continuar lendo pesquisas recentes no campo para se manter atualizado sobre os últimos avanços e técnicas.

## Referências
- NORVIG, P.; RUSSELL, S., Inteligência artificial: Tradução da 3a Edição, [s.l.]: Elsevier Brasil, 2014.
- ROSSI, F., VAN BEEK, P., & WALSH, T. (2006). Handbook of Constraint Programming (Foundations of Artificial Intelligence). Elsevier Science Inc.
- DECHTER, Rina. Constraint Processing. Morgan Kaufmann, 2003.
- KUMAR, V. Algorithms for Constraint-Satisfaction Problems: A Survey. AI Magazine, [s.l.], v. 13, n. 1, p.32, 1992.
