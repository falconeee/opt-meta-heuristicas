# Roteiro da apresentação: Uma introdução à Evolução Diferencial

Apresentação: `Uma_introducao_a_Evolucao_Diferencial.pptx` (23 slides) · duração-alvo: **≈ 38 min**, dentro da previsão de até 40 min

| Bloco | Slides | Tempo |
|---|---|---|
| Abertura | 1 | ≈ 1 min |
| 1. Introdução | 2–5 | ≈ 6,5 min |
| 2. Definição | 6–15 | ≈ 14 min |
| 3. Aplicação | 16–20 | ≈ 12 min |
| 4. Conclusões | 21–23 | ≈ 4,5 min |

Como usar este roteiro: o texto em **O que falar** não é para ler palavra por palavra. Ele mostra a ideia de cada frase. Os tempos são aproximados; se atrasar, encurte os slides 5, 11 e 19.

Os números abaixo seguem a ordem real dos slides no arquivo. O número impresso no rodapé ainda é o antigo: nos slides 2 a 22 ele aparece com uma unidade a mais (o slide 2 mostra "3"), e o slide 23 mostra "25".

---

## Parte I — Roteiro slide a slide

### Slide 1 — Capa · ≈ 1 min
**Objetivo:** apresentar você, o tema e as quatro partes da fala. Como não há slide de roteiro, as partes são ditas aqui.

**O que falar:**
"Boa tarde, meu nome é Gabriel Falcone. Vou apresentar a Evolução Diferencial, ou ED, um método para encontrar o mínimo de funções difíceis. Ela foi criada por Rainer Storn e Kenneth Price e publicada em 1997. A ideia central é simples: usar a diferença entre soluções para decidir para onde andar."

"São quatro partes, na mesma ordem do artigo. Primeiro, por que o problema é difícil e de onde veio a ED. Depois, como o algoritmo funciona, passo a passo, que é a parte mais longa. Em seguida, a ED funcionando na função de Rastrigin. E, por fim, os pontos fortes, os pontos fracos e como o algoritmo evoluiu até hoje."

**Transição:** "Vamos começar pelo problema."

### Slide 2 — Por que a otimização global é difícil · ≈ 2 min
**Objetivo:** mostrar por que métodos simples ficam presos.

**O que falar:**
- "Nosso objetivo é encontrar o menor valor de uma função f que depende de D variáveis contínuas, ou seja, números reais."
- "O problema é que muitas funções reais são difíceis: não são lineares, às vezes nem têm derivada, e têm muitos vales. Chamamos isso de função **multimodal**."
- "Olhem a figura: é a função de Rastrigin. Cada 'buraquinho' é um mínimo local, um vale que parece o fundo mas não é. Só existe um mínimo global, no centro."
- "Métodos como o gradiente só dão passos que melhoram. Se começarem perto de um vale qualquer, descem até o fundo dele e param ali."
- "As meta-heurísticas de população fazem diferente: mantêm várias soluções ao mesmo tempo. Assim, uma solução pode ajudar as outras a sair de um vale ruim."

**Transição:** "Com esse problema em mente, Storn e Price listaram o que queriam de um bom otimizador."

### Slide 3 — O que Storn e Price queriam de um otimizador · ≈ 1,5 min
**Objetivo:** apresentar os quatro requisitos que guiaram o projeto da ED.

**O que falar:**
"Eram quatro desejos. Primeiro, funcionar em funções difíceis: sem derivada, não lineares e com muitos vales. Segundo, permitir avaliar várias soluções ao mesmo tempo, o que importa quando cada avaliação é cara. Terceiro, ser fácil de usar, com poucos parâmetros. E quarto, achar o mínimo global de forma confiável, mesmo repetindo o teste várias vezes. A ED foi desenhada para cumprir os quatro. Nos testes do artigo, ela foi o único método que achou todos os mínimos globais."

**Transição:** "Vamos ver como a ED surgiu e para onde ela foi."

### Slide 4 — Da proposta ao estado da arte · ≈ 2 min
**Objetivo:** dar a linha do tempo.

**O que falar:**
- "1995: o primeiro relatório técnico. A ED nasceu quando Price tentava resolver um problema de ajuste de polinômios proposto por Storn."
- "1996: na primeira competição internacional de otimização evolutiva, ela foi o algoritmo evolutivo mais rápido."
- "1997: o artigo no Journal of Global Optimization, que é a base deste seminário."
- "2005: o livro dos autores, que junta teoria e prática."
- "Entre 2006 e 2009 surgem versões em que os parâmetros se ajustam sozinhos: jDE, SaDE e JADE."
- "Em 2013 e 2014 vêm o SHADE e o L-SHADE. O L-SHADE venceu a competição do IEEE CEC 2014, uma das principais competições da área."
- "Hoje, boa parte da pesquisa tenta fazer o algoritmo ajustar F, CR e a forma de mutação sozinho, durante a execução. Voltamos a isso no fim."

**Transição:** "E onde a ED é usada na prática?"

### Slide 5 — Onde a ED é aplicada · ≈ 1 min
**Objetivo:** mostrar que a ED é útil fora da teoria.

**O que falar:**
"Uma das primeiras aplicações foi o projeto de filtros digitais, feito pelo próprio Storn. Depois vieram antenas e micro-ondas, sistemas de energia, projeto de peças mecânicas, engenharia química e aprendizado de máquina, por exemplo para ajustar hiperparâmetros. O que esses problemas têm em comum: variáveis contínuas e funções caras ou difíceis de avaliar. A ED também já vem pronta em bibliotecas como SciPy e pymoo."

**Transição:** "Agora vamos abrir o algoritmo."

### Slide 6 — O ciclo da Evolução Diferencial · ≈ 1 min
**Objetivo:** dar a visão geral antes dos detalhes.

**O que falar:**
"O ciclo tem quatro passos. Começamos sorteando uma população de NP soluções. Depois, a cada geração, cada solução passa por três operações: **mutação**, que cria um vetor novo; **cruzamento**, que mistura esse vetor com a solução atual; e **seleção**, que fica com a melhor das duas. Isso se repete até atingir o número máximo de gerações ou um valor de f bom o suficiente."

**Transição:** "Antes dos operadores, alguns nomes que vamos usar."

### Slide 7 — População e inicialização · ≈ 1 min
**Objetivo:** fixar o vocabulário.

**O que falar:**
- "Cada solução é só uma lista de D números reais. Não há código binário, como nos algoritmos genéticos clássicos."
- "A população tem NP soluções, e esse número não muda."
- "Na geração zero, cada coordenada é sorteada de forma uniforme dentro dos limites, para cobrir todo o espaço."
- "Três nomes importantes: o **vetor-alvo** é a solução que estamos tentando melhorar; o **vetor mutante (v)** sai da mutação; e o **vetor experimental (u)** sai do cruzamento. Em inglês ele é chamado de *trial vector*."

**Transição:** "Agora o coração do método: a mutação."

### Slide 8 — Mutação: somar uma diferença ponderada · ≈ 2 min
**Objetivo:** explicar a ideia do vetor diferença. **Este é o slide mais importante.**

**O que falar:**
- "Para cada alvo, sorteamos três outras soluções: r1, r2 e r3. Elas precisam ser diferentes entre si e diferentes do alvo; por isso a população precisa ter pelo menos 4 indivíduos."
- "Fazemos a diferença entre r2 e r3. Isso dá uma seta, uma direção. Multiplicamos essa seta por F, que encolhe ou estica o passo. Depois somamos o resultado ao ponto r1."
- Aponte para a figura: "A seta azul é a diferença; a laranja é a mesma seta multiplicada por F = 0,5, saindo de r1."
- "O ponto-chave é este: a direção e o tamanho do passo vêm da própria população. Se as soluções estão espalhadas, o passo é grande. Se estão juntas, o passo é pequeno. O algoritmo ajusta o passo sozinho."

**Transição:** "O mutante não entra direto na população: antes, ele é misturado com o alvo."

### Slide 9 — Cruzamento binomial: misturar mutante e alvo · ≈ 1,5 min
**Objetivo:** explicar como u é montado.

**O que falar:**
- "O cruzamento decide, coordenada por coordenada, de onde vem cada valor de u: do mutante ou do alvo."
- "Para cada coordenada, sorteamos um número entre 0 e 1. Se ele for menor ou igual a CR, o valor vem do mutante. Se não, vem do alvo."
- "Existe uma coordenada sorteada, o j_rand, que sempre vem do mutante. Isso garante que u nunca seja uma cópia do alvo."
- Na figura: "Com CR = 0,4, as posições 2 e 5 passaram no teste; a posição 4 veio do mutante por ser o j_rand."
- "Cada coordenada é um sorteio independente, como numa distribuição binomial. Por isso o nome 'bin'. Existe também o cruzamento exponencial, que copia um bloco seguido de coordenadas."

**Transição:** "Agora temos u. Falta decidir se ele entra na população."

### Slide 10 — Seleção gulosa, um contra um · ≈ 1 min
**Objetivo:** mostrar a regra de sobrevivência.

**O que falar:**
- "A seleção é a parte mais simples. u disputa a vaga só com o seu próprio alvo. É um duelo, um contra um."
- "Ela é **gulosa**: u só entra se for melhor. Por isso, o valor de f em cada posição da população nunca piora."
- "Como cada posição só compete consigo mesma, uma solução muito boa não toma conta da população de uma vez. Isso ajuda a manter a variedade."

**Transição:** "Juntando tudo, temos o algoritmo completo."

### Slide 11 — Algoritmo DE/rand/1/bin · ≈ 1,5 min
**Objetivo:** mostrar que o algoritmo é curto.

**O que falar:**
"São dois laços: um para as gerações e outro para os indivíduos. Para cada indivíduo: sorteamos os índices, fazemos a mutação, o cruzamento e a seleção. O artigo original diz que o núcleo da ED cabe em menos de 30 linhas de C. Um detalhe: a nova população é montada à parte e só substitui a atual no fim da geração, como no artigo."

**Transição:** "O que vimos se chama DE/rand/1/bin. Esse nome tem um significado."

### Slide 12 — Variantes: a notação DE/x/y/z · ≈ 1,5 min
**Objetivo:** explicar o nome das variantes.

**O que falar:**
- "O nome segue o padrão DE/x/y/z. O x diz qual é o ponto de partida da mutação, o vetor-base: aleatório (rand) ou o melhor da população (best). O y diz quantas diferenças usamos. O z diz o tipo de cruzamento: bin ou exp."
- "DE/rand/1 é a clássica, que usamos: como a base é aleatória, explora mais."
- "DE/best/1 parte do melhor: é mais rápida, mas pode parar cedo num mínimo local."
- "DE/best/2 usa duas diferenças, o que dá mais variedade quando a população é grande."
- "DE/current-to-best/1 parte do próprio alvo e o puxa em direção ao melhor. Ela é a base do JADE e do SHADE, que aparecem no fim."

**Transição:** "Além da variante, temos que escolher três números."

### Slide 13 — Só três parâmetros de controle · ≈ 1,5 min
**Objetivo:** dar as regras práticas do artigo.

**O que falar:**
- "**NP**, o tamanho da população: de 5 a 10 vezes o número de variáveis, e no mínimo 4. Mais indivíduos dão mais variedade, mas custam mais avaliações."
- "**F**, o tamanho do passo: 0,5 é um bom começo. Abaixo de 0,4 ou acima de 1 raramente funciona."
- "**CR**, a taxa de cruzamento: 0,1 é a escolha segura; 0,9 ou 1,0 serve para testar se dá para convergir rápido. CR baixo muda poucas coordenadas por vez, o que funciona bem quando as variáveis são independentes umas das outras."
- "E uma dica do artigo: se a população se juntou cedo demais, aumente F ou NP."
- "No nosso exemplo: NP = 20, F = 0,5 e CR = 0,9."

**Transição:** "Esses parâmetros ajudam a entender como a ED equilibra exploração e refinamento."

### Slide 14 — Diversificação e intensificação · ≈ 1,5 min
**Objetivo:** responder à pergunta do enunciado.

**O que falar:**
- "**Diversificação** é explorar regiões novas. **Intensificação** é refinar as regiões boas. Todo algoritmo precisa equilibrar as duas."
- "Na ED não há um botão separado para cada uma. O equilíbrio vem da própria estrutura."
- "No começo, as soluções estão espalhadas, as diferenças são grandes e os saltos também: a ED explora."
- "Com as gerações, a seleção descarta as piores, a população se concentra nos melhores vales, as diferenças ficam curtas e os passos também: a ED refina."
- "Em regiões planas acontece o contrário: as diferenças crescem e a busca acelera. O passo se ajusta sozinho."

**Transição:** "Vamos ver uma geração com números de verdade."


### Slide 16 — O problema: função de Rastrigin · ≈ 1,5 min
**Objetivo:** apresentar o problema-teste.

**O que falar:**
"Usamos a Rastrigin com duas variáveis. Ela é uma parábola com ondulações por cima. As ondulações criam 121 mínimos locais, um perto de cada ponto de coordenadas inteiras. O único mínimo global vale 0 e fica em (0, 0). Ela também faz parte do conjunto de testes do artigo de 1997, a função f13. Lá o domínio era bem maior; aqui usamos de −5,12 a 5,12 para conseguir ver a população sobre a função."

**Transição:** "Para rodar, usamos a biblioteca pymoo."

### Slide 17 — Implementação com o pymoo · ≈ 2,5 min
**Objetivo:** mostrar o código e os cuidados.

**O que falar:**
- "O pymoo já tem a ED pronta. Mas os valores padrão dele não são os do artigo, então ajustamos quatro coisas:"
- "A variante: o padrão do pymoo é DE/best/1/bin; trocamos para DE/rand/1/bin."
- "O CR: o padrão é 0,2; usamos 0,9."
- "prob_mut = 0: desliga uma mutação extra que o pymoo aplica depois do cruzamento e que não existe no algoritmo original."
- "n_gen = 101: o pymoo conta a população inicial como geração 1, então são 100 gerações da ED e 2020 avaliações da função."
- "Também testamos outras bibliotecas: o pygmo não tem pacote para Mac com chip ARM, e o mealpy não usa o j_rand no cruzamento."

**Transição:** "Vamos ver o que aconteceu com a população."

### Slide 18 — A população ao longo das gerações · ≈ 3 min
**Objetivo:** mostrar a passagem de explorar para refinar.

**O que falar:**
- "A figura mostra a população em quatro momentos. Na geração 0, os 20 indivíduos estão espalhados por 19 vales diferentes."
- "A tabela conta em quantos vales, que chamamos de bacias, a população está. Esse número cai de 19 para 14, 12, 9, 3 e chega a 1 na geração 42."
- "A partir da geração 42, todos estão no vale do mínimo global. Daí em diante, a ED só refina."
- "O melhor valor de f cai de 25 para 3,55 × 10⁻¹⁵, praticamente zero."
- "Essa é a diversificação virando intensificação, como vimos no slide 14."

**Transição:** "Isso fica ainda mais claro em movimento."

### Slide 19 — A Evolução Diferencial em movimento · ≈ 2 min
**Objetivo:** deixar a animação falar.

**O que falar:**
"Vou deixar a animação rodar uma vez, leva uns 12 segundos. Cada ponto laranja é uma solução. Observem três fases: até a geração 20, saltos longos exploram todo o espaço; entre 20 e 42, a população abandona os vales vizinhos e vai para o central; depois, os passos ficam cada vez menores e a população vira quase um ponto só na origem."

**Transição:** "Por fim, os números do experimento."

### Slide 20 — Convergência e números finais · ≈ 3 min
**Objetivo:** fechar o experimento com dados.

**O que falar:**
- "O melhor valor encontrado foi 3,55 × 10⁻¹⁵ na geração 100. Para efeitos práticos, é zero."
- "Para chegar a f menor que 10⁻⁶ foram 1240 avaliações, na geração 61, de 2020 no total."
- "No gráfico da esquerda, o melhor valor cai em degraus, porque só muda quando aparece alguém melhor. A média cai de forma contínua."
- "O gráfico da direita é o mais interessante: mostra o quanto a população está espalhada. Esse espalhamento é também o tamanho do passo da mutação. Ele cai 100 milhões de vezes, de 4 para quase 2 × 10⁻⁸, sem nenhum ajuste manual."
- "A queda forte perto da geração 40 é o momento em que os últimos indivíduos saem dos vales vizinhos."

**Transição:** "Com isso, vamos às conclusões."

### Slide 21 — Potencialidades e limitações · ≈ 1,5 min
**Objetivo:** fazer um balanço honesto.

**O que falar:**
- "Pontos fortes: é simples, com poucas linhas de código e três parâmetros; não precisa de derivada e lida bem com muitos mínimos locais; as NP soluções podem ser avaliadas em paralelo; e o passo se ajusta sozinho."
- "Pontos fracos: o resultado depende de F, CR e da variante escolhida; a população pode estagnar ou se juntar cedo demais; problemas com variáveis inteiras ou com restrições precisam de adaptações; e não há garantia de achar o ótimo global. O teorema *no free lunch* diz que nenhum método é o melhor em todos os problemas."
- "Muitas dessas limitações viraram tema de pesquisa, e é isso que o próximo slide mostra."

**Transição:** direto para o slide 22.

### Slide 22 — Como a ED evoluiu desde 1997 · ≈ 2,5 min
**Objetivo:** mostrar as evoluções e os ganhos (fonte: survey de Reyes-Davila et al., 2025).

**O que falar:**
- "Um survey publicado em 2025 reuniu 113 trabalhos que modificaram a ED. Eles organizaram tudo pelas quatro etapas que acabamos de ver."
- "**Inicialização:** em vez de sortear a população ao acaso, começar com ela mais bem espalhada, usando mapas caóticos, pontos opostos ou agrupamento. Ganho: a convergência fica mais rápida na maioria das propostas."
- "**Mutação**, onde está a evolução mais conhecida: em vez de fixar F e CR na mão, o algoritmo aprende esses valores com as tentativas que deram certo. Além disso, o guia é sorteado entre os melhores indivíduos. São o JADE e o SHADE. Ganho: acaba a tentativa e erro, e o L-SHADE, que também diminui a população ao longo da execução, venceu a competição CEC 2014."
- "**Cruzamento:** o CR passa a mudar durante a busca, e há versões que trocam coordenadas em eixos girados. Ganho: funciona melhor quando as variáveis dependem umas das outras."
- "**Seleção:** além do duelo um contra um, surgem seleções elitistas, outras que olham a variedade da população e outras que reaproveitam soluções descartadas. Ganho: menos estagnação e convergência mais rápida."
- "A tendência apontada pelo survey: mutação e cruzamento já foram muito estudados. Inicialização e seleção são só 20% dos trabalhos, e os autores as apontam como o caminho para pesquisas futuras."

**Transição:** "Essas foram as evoluções. Para encerrar, as referências."

### Slide 23 — Referências · ≈ 0,5 min
**Objetivo:** encerrar a fala e abrir para perguntas. Não há slide de encerramento, então este fica na tela durante as perguntas.

**O que falar:**
"Estas são as referências que usei, incluindo o survey de 2025. Para fechar, a ideia principal: a ED transforma diferenças entre soluções em passos de busca, longos quando a população está espalhada e curtos quando ela se junta. O experimento completo está no notebook do trabalho. Obrigado, fico à disposição para perguntas."

Respostas prováveis estão na Parte IV.

---

## Parte II — Explicação do algoritmo (para estudar)

### A ideia em uma frase
A ED anda pelo espaço de busca usando **a diferença entre duas soluções da população** como passo. Como as soluções se aproximam ao longo do tempo, o passo encolhe sozinho.

### Os elementos
- **Solução (indivíduo):** um vetor x = (x₁, …, x_D) de números reais.
- **População:** NP vetores. O tamanho fica fixo na ED clássica.
- **Função objetivo f:** o que queremos minimizar. A ED só precisa calcular f(x), não precisa de derivada.

### Passo 0 — Inicialização
Cada coordenada é sorteada de forma uniforme entre o limite inferior e o superior:

x_{i,j} = min_j + rand(0,1) · (max_j − min_j)

### Passo 1 — Mutação (DE/rand/1)
Para cada alvo xᵢ, sorteie três índices diferentes entre si e diferentes de i:

v = x_{r1} + F · (x_{r2} − x_{r3})

- (x_{r2} − x_{r3}) é o **vetor diferença**: dá a direção e o tamanho do passo.
- F (geralmente entre 0,4 e 1) estica ou encolhe esse passo.
- x_{r1} é o **vetor-base**, o ponto de partida.

**Por que funciona:** com a população espalhada, as diferenças são grandes, e o algoritmo explora. Com a população concentrada, as diferenças são pequenas, e o algoritmo refina. Não é preciso escolher manualmente o tamanho do passo.

### Passo 2 — Cruzamento binomial
Para cada coordenada j:

u_j = v_j se rand_j ≤ CR ou j = j_rand; senão u_j = x_{i,j}

- CR é a chance de cada coordenada vir do mutante.
- j_rand garante que pelo menos uma coordenada venha do mutante.

### Passo 3 — Seleção
xᵢ na próxima geração = u se f(u) < f(xᵢ); senão, continua xᵢ.

É uma disputa individual e gulosa: a qualidade de cada posição nunca piora, e nenhuma solução domina a população de uma vez.

### Os três parâmetros
| Parâmetro | O que controla | Valor inicial sugerido |
|---|---|---|
| NP | quantas soluções existem (variedade × custo) | 5D a 10D, no mínimo 4 |
| F | tamanho do passo | 0,5 |
| CR | quantas coordenadas mudam por vez | 0,1 (seguro) ou 0,9 (rápido) |

Regra prática: se a população se juntou cedo demais, aumente F ou NP.

### Exemplo numérico do slide 15, passo a passo
1. Alvo: (2,805; −0,626), f = 31,887.
2. Diferença r2 − r3 = (−2,796 − 2,643; 1,739 − (−1,490)) = (−5,439; 3,229).
3. F · diferença = 0,5 · (−5,439; 3,229) = (−2,720; 1,615).
4. Mutante v = base r1 + passo = (0,559 − 2,720; −4,467 + 1,615) = (−2,161; −2,852).
5. Cruzamento (CR = 0,9): rand₁ = 0,374 ≤ 0,9 → x₁ vem de v; rand₂ = 0,987 > 0,9 e j_rand = 1 → x₂ vem do alvo.
6. u = (−2,161; −0,626), f = 26,770.
7. Seleção: 26,770 < 31,887 → u substitui o alvo.

### Diversificação × intensificação
| Fase | O que acontece | Por quê |
|---|---|---|
| Início | saltos grandes por todo o espaço | soluções espalhadas → diferenças longas |
| Meio | população abandona vales ruins | seleção gulosa descarta os piores |
| Fim | passos minúsculos perto do ótimo | soluções juntas → diferenças curtas |

No experimento: os vales ocupados caem de 19 para 1 até a geração 42, e o espalhamento da população cai 10⁸ vezes até a geração 100.

---

## Parte III — As evoluções da ED (para estudar)

### Linha do tempo das variantes mais conhecidas
| Ano | Variante | O que trouxe | Ganho |
|---|---|---|---|
| 1997 | DE/rand/1/bin | algoritmo original | simples e robusto; venceu outros métodos nos testes do artigo |
| 2006 | **jDE** (Brest et al.) | cada indivíduo carrega seu próprio F e CR, que de vez em quando são sorteados de novo; os valores que geram bons filhos sobrevivem | dispensa ajustar F e CR na mão |
| 2009 | **SaDE** (Qin et al.) | mantém várias estratégias de mutação e escolhe a que teve mais sucesso recentemente | o algoritmo escolhe a estratégia sozinho |
| 2009 | **JADE** (Zhang e Sanderson) | mutação *current-to-pbest/1*: o guia é sorteado entre os p% melhores, não é só o melhor; arquivo com soluções descartadas; F e CR aprendidos com os sucessos | converge rápido sem perder variedade |
| 2013 | **SHADE** (Tanabe e Fukunaga) | uma "memória" com os valores de F e CR que deram certo nas últimas gerações | adaptação mais estável que a do JADE |
| 2014 | **L-SHADE** | SHADE + população que diminui em linha reta ao longo da execução | venceu a competição IEEE CEC 2014 |

**Resumo da evolução:** o maior ponto fraco da ED original é depender de F, CR e da escolha da variante. As versões mais bem-sucedidas atacaram exatamente isso: o algoritmo passa a **aprender** os parâmetros enquanto roda.

### O que diz o survey de 2025 (Reyes-Davila et al.)
O artigo analisa 113 trabalhos (63 de pesquisa direta e 50 relacionados), organizados pelas quatro etapas da ED. Para comparar as propostas, usa três critérios de eficiência (de Beyer): **velocidade de convergência**, **capacidade de achar o ótimo em problemas diferentes** e **custo computacional**.

**1. Inicialização** (cerca de 15% das propostas)
- Ideias: começar com a população mais bem distribuída. Exemplos: pontos opostos (ODE e variantes), mapas caóticos (CIDE), hipercubo latino (SLADE-SLHD), agrupamento com k-means (CBPI, DEc), população dividida e ajustada durante a busca.
- Ganho: quase todas aceleram a convergência; cerca de metade reduz o custo computacional.
- Destaques: **Cumu-DE** e **CODE** (oposição + caos) foram as únicas que atenderam aos três critérios.

**2. Mutação** (a etapa mais estudada)
- O survey divide as estratégias em dois grupos:
  - **Aleatórias** (DE/rand/1, DE/rand/2, current-to-rand): exploram mais e ajudam a sair de mínimos locais, mas podem ser lentas.
  - **Guiadas** (DE/best, current-to-best, current-to-pbest, rand-to-best, mutação triangular, histórica e outras): andam em direção às melhores soluções. São mais rápidas, mas podem parar cedo se o guia estiver num mínimo local.
- Tendência forte: **combinar várias estratégias**, seja escolhendo a que tem dado mais certo (SaDE, EPSDE), seja dividindo a população em grupos com estratégias diferentes.
- Lembrete do survey: nenhuma estratégia de mutação é a melhor em todos os problemas (*no free lunch*). Um mesmo operador pode ir mal num algoritmo e muito bem em outro, como aconteceu com o current-to-pbest no L-SHADE.

**3. Cruzamento**
- Os clássicos são o **binomial** (coordenadas sorteadas uma a uma) e o **exponencial** (um bloco seguido de coordenadas). O exponencial se sai bem em problemas com muitas variáveis.
- Limitação do binomial: ele depende dos eixos. Se a função estiver "girada", isto é, se as variáveis dependem umas das outras, ele perde eficiência.
- Ideias novas: CR ajustado ou reparado durante a busca, cruzamento por **autovetores** (troca coordenadas em eixos alinhados com a população, o que resolve o problema das funções giradas), cruzamento ortogonal, e cruzamento que escolhe entre opções conforme a variedade da população.
- Ganho: mais robustez em problemas em que as variáveis interagem.

**4. Seleção** (só cerca de 2% das propostas)
- A seleção gulosa um contra um quase não mudou por anos, porque a grande contribuição da ED era a mutação.
- Ideias novas: seleção elitista (junta pais e filhos e escolhe os melhores), seleção que considera a variedade da população (**IDEI**), seleção por subconjuntos (STS), e reaproveitar vetores descartados quando a busca estagna.
- Ganho: menos estagnação e convergência mais rápida. O **IDEI** foi a única proposta boa nos três critérios.

**Conclusão e tendência do survey**
- O número de novas mutações e cruzamentos está caindo; o interesse por **inicialização e seleção** está crescendo.
- Essas duas etapas somam só **cerca de 20%** dos trabalhos, mas mostraram que melhoram a ED. Os autores as indicam como o principal caminho para pesquisas futuras.
- Falta na literatura comparar todas as propostas no mesmo conjunto de testes; cada autor usa o seu, o que dificulta dizer qual é a melhor.

---

## Parte IV — Glossário e perguntas prováveis

### Glossário rápido
| Termo | Significado simples |
|---|---|
| Mínimo local | um vale que parece o fundo, mas não é o mais baixo |
| Mínimo global | o ponto mais baixo de toda a função |
| Função multimodal | função com muitos vales (muitos mínimos locais) |
| Meta-heurística | estratégia geral de busca que não garante o ótimo, mas costuma achar boas soluções |
| Vetor-alvo | a solução que estamos tentando melhorar |
| Vetor mutante (v) | resultado da mutação |
| Vetor experimental (u) | resultado do cruzamento; disputa a vaga com o alvo |
| Guloso | só aceita mudanças que melhoram |
| Diversificação | explorar regiões novas do espaço |
| Intensificação | refinar as regiões que já parecem boas |
| Bacia (de atração) | região em volta de um mínimo; quem começa nela tende a descer até ele |
| Função separável | as variáveis podem ser otimizadas uma de cada vez, de forma independente |
| *No free lunch* | teorema: nenhum método é o melhor em todos os problemas |
| CEC | competição anual de algoritmos evolutivos do IEEE |

### Perguntas prováveis
- **Por que não usar gradiente?** A Rastrigin tem 121 mínimos locais. Um método que só desce para no vale mais próximo do ponto de partida.
- **Por que CR = 0,9 se a Rastrigin é separável?** Seguimos a sugestão do artigo de testar 0,9 para convergência rápida. CR baixo também funcionaria bem aqui.
- **Como a ED trata restrições?** Com penalidades na função objetivo ou com regras que preferem soluções viáveis, como faz o pymoo.
- **Como escolher F e CR em outro problema?** Comece pelas regras do slide 13 ou use uma variante adaptativa, como JADE ou SHADE, que aprende esses valores sozinha.
- **Qual a diferença da ED para um algoritmo genético?** Na ED, a solução é um vetor de números reais, e a mutação usa a diferença entre soluções da população. No AG clássico, a solução costuma ser binária e a mutação é um sorteio fixo. Além disso, na ED a mutação vem antes do cruzamento, e a seleção é um duelo um contra um.
- **Qual é a melhor variante hoje?** Não existe uma melhor para tudo (*no free lunch*). Em competições, as da família SHADE/L-SHADE costumam ir muito bem.
- **O que mudou desde 1997?** Principalmente a adaptação automática de F, CR e da estratégia (jDE, SaDE, JADE, SHADE, L-SHADE). Segundo o survey de 2025, a próxima fronteira são a inicialização e a seleção.
