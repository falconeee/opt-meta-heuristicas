# Otimização e Meta-heurísticas — PPGI/UFES

Repositório com os trabalhos desenvolvidos na disciplina de **Meta-heurísticas (2026/2)** do
**Programa de Pós-Graduação em Informática (PPGI)** da **Universidade Federal do Espírito Santo (UFES)**,
como parte do mestrado em Informática.

Cada pasta corresponde a uma atividade da disciplina e reúne o enunciado, o código (em notebooks
Jupyter), os dados, os resultados e o material de apresentação.

| Pasta | Atividade | Tema | Técnicas |
|---|---|---|---|
| [`Seminario/`](Seminario/) | Atividade S — Seminário | *Uma introdução à Evolução Diferencial* | Evolução Diferencial (DE/rand/1/bin) na função de Rastrigin |
| [`T1/`](T1/) | Trabalho 1 | Problema *Splitwise* | Construtivos guloso e randomizado, busca local *first/best-improvement* |
| [`T2/`](T2/) | Trabalho 2 | — | *em andamento* |

---

## Estrutura do repositório

```
.
├── Seminario/                         # Atividade S — Seminário sobre Evolução Diferencial
│   ├── enunciado/AtividadeS.pdf       # especificação da atividade
│   ├── artigo/                        # artigo (LaTeX, modelo SBC) + figuras + PDF
│   ├── implementacao/                 # notebook da DE na Rastrigin 2D (projeto uv)
│   │   ├── de_rastrigin.ipynb
│   │   ├── figuras/                   # figuras e GIF gerados pelo notebook
│   │   ├── resultados.json            # números usados no artigo e nos slides
│   │   └── pyproject.toml, uv.lock
│   ├── slides/                        # apresentação (.pptx), deck HTML e imagens
│   └── referencias/                   # material de referência (ED.pdf)
│
├── T1/                                # Trabalho 1 — Splitwise
│   ├── enunciado/                     # especificação da atividade
│   ├── T1_Splitwise_Gabriel_Falcone.ipynb
│   ├── instancias/                    # 32 instâncias instancia_splitwise_*.txt
│   ├── resultados/                    # tabela consolidada (CSV) e gráfico
│   └── apresentacao/                  # slides da apresentação
│
└── T2/                                # Trabalho 2 (em andamento)
```

---

## Seminário — Uma introdução à Evolução Diferencial

Seminário (slides + artigo de 4 a 6 páginas no modelo SBC) sobre a **Evolução Diferencial (ED/DE)**,
meta-heurística populacional para otimização contínua proposta por Storn e Price (1995/1997).

- **Conteúdo:** histórico e estado da arte, aplicações, o algoritmo clássico **DE/rand/1/bin**
  (mutação diferencial, cruzamento binomial e seleção gulosa), pseudocódigo, variantes
  (`best/1`, `rand/1`, `best/2`, `current-to-best/1`), papel dos parâmetros $NP$, $F$ e $CR$, e o
  equilíbrio entre diversificação e intensificação.
- **Exemplo de aplicação:** minimização da **função de Rastrigin em 2D** (121 mínimos locais no
  domínio $[-5{,}12;\ 5{,}12]^2$) com a biblioteca [pymoo](https://pymoo.org), configurada para
  reproduzir fielmente o DE original ($NP=20$, $F=0{,}5$, $CR=0{,}9$, sem mutação polinomial extra).
- **Resultado:** o DE encontra o mínimo global $f(0,0)=0$ (erro $\approx 10^{-15}$ em 100 gerações,
  $f<10^{-6}$ a partir da geração 61). O notebook também gera um exemplo numérico passo a passo de
  uma geração, a evolução da população sobre a paisagem da função, curvas de convergência e uma
  animação (GIF) usada nos slides.

### Como executar

O ambiente é gerenciado com [uv](https://docs.astral.sh/uv/) (Python ≥ 3.12):

```bash
cd Seminario/implementacao
uv sync
uv run jupyter lab de_rastrigin.ipynb
```

O notebook regrava `figuras/` e `resultados.json`. O artigo pode ser compilado a partir de
`Seminario/artigo/main.tex` (por exemplo, no Overleaf ou com `pdflatex` + `bibtex`).

---

## T1 — Problema *Splitwise*: construtivos e busca local

Dado um grupo de pessoas com saldos líquidos (soma zero), o problema *Splitwise* consiste em
quitar todas as dívidas usando o **menor número possível de transações**.

- **Modelagem:** o valor ótimo é $f^* = n' - k_{\max}$, em que $k_{\max}$ é o maior número de
  grupos de soma zero em que as pessoas podem ser particionadas. A solução é representada por uma
  **permutação** decodificada como cadeia de repasses — toda permutação é viável e toda partição
  é representável.
- **Algoritmos construtivos:**
  - **ACG** (guloso) — escolhe o candidato que deixa o resíduo do grupo mais próximo de zero;
  - **ACR** (randomizado) — mesma função gulosa com lista restrita de candidatos (LRC, $k=3$).
- **Busca local:** vizinhança de **troca (swap)** com avaliação **incremental** em
  $O(\log n')$ por vizinho, nas estratégias **first-improvement (BL_FI)** e
  **best-improvement (BL_BI)**, partindo do ACG e do ACR.
- **Experimentos:** 32 instâncias (de 20 a 1500 pessoas). A busca local reduz em média 6,5% o
  número de transações a partir do ACG, chegando a ~27% em instâncias com grupos grandes de
  valores pequenos. Resultados completos em
  [`T1/resultados/tabela_resultados.csv`](T1/resultados/tabela_resultados.csv) e no gráfico
  [`grafico_construtivo_vs_bl.png`](T1/resultados/grafico_construtivo_vs_bl.png).

### Como executar

Dependências: `numpy`, `pandas`, `matplotlib`. Execute o notebook a partir da pasta `T1/`
(ele lê `instancias/` e grava em `resultados/`):

```bash
cd T1
pip install numpy pandas matplotlib jupyter
jupyter lab T1_Splitwise_Gabriel_Falcone.ipynb
```

O notebook também funciona no Google Colab (há uma célula para enviar `instancias.zip`).

---

## Autor

**Gabriel Falcone** — Mestrado em Informática, PPGI/UFES.
