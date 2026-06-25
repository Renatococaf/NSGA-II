# NSGA-II

Jupyter Notebook explorando o algoritmo **NSGA-II** (Non-dominated Sorting Genetic Algorithm II) aplicado ao **Problema do Caixeiro Viajante Multi-Objetivo (MOTSP)**, usando a biblioteca [PyMOO](https://pymoo.org).

## Contexto

Em problemas de otimização multi-objetivo, não existe uma única solução ótima — existe um conjunto de soluções de compromisso chamado **Frente de Pareto**. O NSGA-II é um algoritmo evolutivo clássico que encontra e mantém esse conjunto ao longo das gerações, equilibrando dois mecanismos:

- **Non-dominated sorting:** classifica os indivíduos em frentes de dominância
- **Crowding distance:** mantém diversidade dentro de cada frente, preferindo soluções mais espaçadas

## Problema: MOTSP

O MOTSP é o TSP com múltiplos objetivos simultâneos (ex: tempo de viagem e custo de viagem). O objetivo é minimizar todos ao mesmo tempo, encontrando a frente de Pareto de rotas que representam os melhores compromissos possíveis.

- **Cidades:** 20
- **Objetivos:** 2 (configurável)
- **Restrição:** cada cidade visitada exatamente uma vez

## Algoritmo e Operadores

| Componente | Configuração |
|-----------|-------------|
| Algoritmo | NSGA2 (PyMOO) |
| Tamanho da população | 100 |
| Crossover | Edge Recombination (ERX) |
| Mutação | Inversion Mutation |
| Amostragem | Permutation Random Sampling |
| Eliminação de duplicatas | Ativada |

## Convergência

O notebook demonstra duas métricas de convergência:

- **Curva de convergência por objetivo:** evolução do fitness do primeiro indivíduo da frente de Pareto ao longo das gerações
- **Running Metric:** métrica proposta por Blank & Deb (2020) que avalia o deslocamento da frente de Pareto entre intervalos de gerações, normalizando pelo conjunto final

## Exercícios e Respostas

### Exercício 1 — Visualização da população e de soluções individuais

> Veja as opções de [visualização](http://pymoo.org/visualization/index.html) no PyMOO e visualize a população. Qual gráfico faz mais sentido para apresentar a população completa? E para uma solução individual? E quando o número de objetivos aumenta para 5?

**Para a população completa com 2 objetivos:** o **Scatter plot** é a escolha mais natural. Ele posiciona cada solução no espaço objetivo e torna a frente de Pareto imediatamente visível como uma curva de compromisso — soluções à esquerda minimizam o objetivo 1, soluções abaixo minimizam o objetivo 2.

**Para uma solução individual:** um **gráfico de barras** com os valores de cada objetivo é direto e claro. Para contexto, pode-se destacar essa solução no scatter da frente de Pareto.

**Quando o número de objetivos aumenta para 5:** o scatter plot torna-se impraticável (não é possível visualizar 5 dimensões simultaneamente). As alternativas mais adequadas são:
- **Parallel Coordinates Plot:** cada eixo vertical representa um objetivo e cada linha representa uma solução — permite identificar padrões de trade-off entre muitos objetivos
- **Radar/Petal diagram:** útil para comparar uma única solução ou um pequeno grupo delas com referências, mostrando o "perfil" de cada solução em todos os objetivos
- **Heatmap:** representa as soluções como linhas e os objetivos como colunas, usando cor para o valor — adequado quando há muitas soluções e muitos objetivos

---

### Exercício 2 — Impacto do número de objetivos na convergência do NSGA-II

> Aumente o número de objetivos e observe a convergência do NSGA-II. Aproximadamente, quanto cada objetivo adicional muda a velocidade de convergência?

Cada objetivo adicional degrada significativamente a convergência do NSGA-II por dois motivos principais:

1. **Dominância de Pareto perde poder discriminativo:** com mais objetivos, é muito mais difícil uma solução dominar outra (seria necessário ser melhor em todos os objetivos simultaneamente). Com 5+ objetivos, quase toda a população fica na primeira frente não-dominada, tornando o non-dominated sorting ineficaz como mecanismo de seleção.

2. **Espaço de busca cresce exponencialmente:** a frente de Pareto em si se torna muito mais extensa, exigindo populações e gerações muito maiores para cobri-la adequadamente.

Na prática, o NSGA-II mantém bom desempenho para **2–3 objetivos**. Com **4–5 objetivos**, a convergência deteriora visivelmente — o número de gerações necessárias pode crescer na ordem de 3–10× por objetivo adicional. Para **6+ objetivos** (problemas "many-objective"), o NSGA-II é geralmente inadequado e algoritmos especializados como **NSGA-III**, **MOEA/D** ou **SPEA2** são preferidos, pois substituem o crowding distance por mecanismos mais eficazes em alta dimensão.

## Dependências

```bash
pip install pymoo numpy matplotlib
```

## Como executar

```bash
jupyter notebook moo/NSGA-II.ipynb
```

## Estrutura do repositório

```
NSGA-II/
└── moo/
    └── NSGA-II.ipynb
```

## Referências

- Wilson, D. G. — [Evolutionary Algorithms Course](https://d9w.github.io/evolution/)
- Blank, J., & Deb, K. (2020). *A running performance metric and termination criterion for evaluating evolutionary multi- and many-objective optimization algorithms.* WCCI.
- Deb, K. et al. (2002). *A fast and elitist multiobjective genetic algorithm: NSGA-II.* IEEE Transactions on Evolutionary Computation.
- [PyMOO Documentation](https://pymoo.org)
