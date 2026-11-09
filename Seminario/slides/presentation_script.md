# Presentation script: An introduction to Differential Evolution

Presentation: `Uma_introducao_a_Evolucao_Diferencial.pptx` (23 slides) · target length: **≈ 38 min**, within the allowed 40 min

| Block | Slides | Time |
|---|---|---|
| Opening | 1 | ≈ 1 min |
| 1. Introduction | 2–5 | ≈ 6.5 min |
| 2. Definition | 6–15 | ≈ 14 min |
| 3. Application | 16–20 | ≈ 12 min |
| 4. Conclusions | 21–23 | ≈ 4.5 min |

How to use this script: the text under **What to say** is not meant to be read word for word. It gives the idea of each sentence. The times are approximate; if you run late, shorten slides 5, 11 and 19.

The numbers below follow the actual slide order in the file. The number printed in the footer is still the old one: on slides 2 to 22 it shows one more than the real number (slide 2 shows "3"), and slide 23 shows "25".

> The slides themselves are in Portuguese; the slide titles below are translated.

---

## Part I — Slide-by-slide script

### Slide 1 — Title · ≈ 1 min
**Goal:** introduce yourself, the topic and the four parts of the talk. Since there is no outline slide, the parts are stated here.

**What to say:**
"Good afternoon, my name is Gabriel Falcone. I am going to present Differential Evolution, or DE, a method for finding the minimum of hard functions. It was created by Rainer Storn and Kenneth Price and published in 1997. The core idea is simple: use the difference between solutions to decide where to move."

"There are four parts, in the same order as the paper. First, why the problem is hard and where DE came from. Then, how the algorithm works, step by step, which is the longest part. Next, DE running on the Rastrigin function. And finally, its strengths, its weaknesses and how the algorithm has evolved up to today."

**Transition:** "Let's start with the problem."

### Slide 2 — Why global optimization is hard · ≈ 2 min
**Goal:** show why simple methods get stuck.

**What to say:**
- "Our goal is to find the smallest value of a function f that depends on D continuous variables, that is, real numbers."
- "The problem is that many real functions are hard: they are not linear, sometimes they do not even have a derivative, and they have many valleys. We call this a **multimodal** function."
- "Look at the figure: it is the Rastrigin function. Each 'little hole' is a local minimum, a valley that looks like the bottom but is not. There is only one global minimum, in the center."
- "Methods like gradient descent only take steps that improve. If they start near any valley, they go down to its bottom and stop there."
- "Population-based meta-heuristics do it differently: they keep several solutions at the same time. That way, one solution can help the others escape a bad valley."

**Transition:** "With this problem in mind, Storn and Price listed what they wanted from a good optimizer."

### Slide 3 — What Storn and Price wanted from an optimizer · ≈ 1.5 min
**Goal:** present the four requirements that guided DE's design.

**What to say:**
"There were four wishes. First, to work on hard functions: no derivative, non-linear and with many valleys. Second, to allow evaluating several solutions at the same time, which matters when each evaluation is expensive. Third, to be easy to use, with few parameters. And fourth, to find the global minimum reliably, even when the test is repeated several times. DE was designed to meet all four. In the paper's tests, it was the only method that found every global minimum."

**Transition:** "Let's see how DE came about and where it went."

### Slide 4 — From the proposal to the state of the art · ≈ 2 min
**Goal:** give the timeline.

**What to say:**
- "1995: the first technical report. DE was born while Price was trying to solve a polynomial-fitting problem posed by Storn."
- "1996: at the first international contest on evolutionary optimization, it was the fastest evolutionary algorithm."
- "1997: the paper in the Journal of Global Optimization, which is the basis of this seminar."
- "2005: the authors' book, which brings theory and practice together."
- "Between 2006 and 2009, versions appear in which the parameters tune themselves: jDE, SaDE and JADE."
- "In 2013 and 2014 come SHADE and L-SHADE. L-SHADE won the IEEE CEC 2014 competition, one of the main competitions in the field."
- "Today, a large part of the research tries to make the algorithm tune F, CR and the mutation scheme by itself, during the run. We will come back to this at the end."

**Transition:** "And where is DE used in practice?"

### Slide 5 — Where DE is applied · ≈ 1 min
**Goal:** show that DE is useful beyond theory.

**What to say:**
"One of the first applications was digital filter design, done by Storn himself. Then came antennas and microwaves, power systems, mechanical part design, chemical engineering and machine learning, for example to tune hyperparameters. What these problems have in common: continuous variables and functions that are expensive or hard to evaluate. DE also comes ready-made in libraries such as SciPy and pymoo."

**Transition:** "Now let's open up the algorithm."

### Slide 6 — The Differential Evolution cycle · ≈ 1 min
**Goal:** give the big picture before the details.

**What to say:**
"The cycle has four steps. We start by sampling a population of NP solutions. Then, in each generation, every solution goes through three operations: **mutation**, which creates a new vector; **crossover**, which mixes that vector with the current solution; and **selection**, which keeps the better of the two. This repeats until the maximum number of generations or a good-enough value of f is reached."

**Transition:** "Before the operators, a few names we will use."

### Slide 7 — Population and initialization · ≈ 1 min
**Goal:** fix the vocabulary.

**What to say:**
- "Each solution is just a list of D real numbers. There is no binary encoding, as in classic genetic algorithms."
- "The population has NP solutions, and that number does not change."
- "In generation zero, each coordinate is sampled uniformly within the bounds, to cover the whole space."
- "Three important names: the **target vector** is the solution we are trying to improve; the **mutant vector (v)** comes out of mutation; and the **trial vector (u)** comes out of crossover."

**Transition:** "Now the heart of the method: mutation."

### Slide 8 — Mutation: adding a weighted difference · ≈ 2 min
**Goal:** explain the idea of the difference vector. **This is the most important slide.**

**What to say:**
- "For each target, we sample three other solutions: r1, r2 and r3. They must be different from each other and from the target; that is why the population needs at least 4 individuals."
- "We take the difference between r2 and r3. That gives an arrow, a direction. We multiply that arrow by F, which shrinks or stretches the step. Then we add the result to the point r1."
- Point at the figure: "The blue arrow is the difference; the orange one is the same arrow multiplied by F = 0.5, starting from r1."
- "The key point is this: the direction and the size of the step come from the population itself. If the solutions are spread out, the step is large. If they are close together, the step is small. The algorithm adjusts the step by itself."

**Transition:** "The mutant does not go straight into the population: first, it is mixed with the target."

### Slide 9 — Binomial crossover: mixing mutant and target · ≈ 1.5 min
**Goal:** explain how u is built.

**What to say:**
- "Crossover decides, coordinate by coordinate, where each value of u comes from: the mutant or the target."
- "For each coordinate, we draw a number between 0 and 1. If it is less than or equal to CR, the value comes from the mutant. Otherwise, it comes from the target."
- "There is one randomly chosen coordinate, j_rand, that always comes from the mutant. This guarantees that u is never a copy of the target."
- On the figure: "With CR = 0.4, positions 2 and 5 passed the test; position 4 came from the mutant because it is j_rand."
- "Each coordinate is an independent draw, as in a binomial distribution. Hence the name 'bin'. There is also exponential crossover, which copies a contiguous block of coordinates."

**Transition:** "Now we have u. It remains to decide whether it enters the population."

### Slide 10 — Greedy one-to-one selection · ≈ 1 min
**Goal:** show the survival rule.

**What to say:**
- "Selection is the simplest part. u competes for the slot only with its own target. It is a duel, one against one."
- "It is **greedy**: u only enters if it is better. That is why the value of f at each position in the population never gets worse."
- "Since each position only competes with itself, a very good solution does not take over the population all at once. This helps keep diversity."

**Transition:** "Putting it all together, we have the complete algorithm."

### Slide 11 — The DE/rand/1/bin algorithm · ≈ 1.5 min
**Goal:** show that the algorithm is short.

**What to say:**
"There are two loops: one over generations and one over individuals. For each individual: we sample the indices, do the mutation, the crossover and the selection. The original paper says DE's core fits in fewer than 30 lines of C. One detail: the new population is built separately and only replaces the current one at the end of the generation, as in the paper."

**Transition:** "What we have seen is called DE/rand/1/bin. That name means something."

### Slide 12 — Variants: the DE/x/y/z notation · ≈ 1.5 min
**Goal:** explain the names of the variants.

**What to say:**
- "The name follows the pattern DE/x/y/z. The x says which point the mutation starts from, the base vector: random (rand) or the best of the population (best). The y says how many differences we use. The z says the crossover type: bin or exp."
- "DE/rand/1 is the classic one, which we use: since the base is random, it explores more."
- "DE/best/1 starts from the best: it is faster, but it may stop early at a local minimum."
- "DE/best/2 uses two differences, which gives more diversity when the population is large."
- "DE/current-to-best/1 starts from the target itself and pulls it toward the best. It is the basis of JADE and SHADE, which appear at the end."

**Transition:** "Besides the variant, we have to choose three numbers."

### Slide 13 — Only three control parameters · ≈ 1.5 min
**Goal:** give the paper's rules of thumb.

**What to say:**
- "**NP**, the population size: 5 to 10 times the number of variables, and at least 4. More individuals give more diversity, but cost more evaluations."
- "**F**, the step size: 0.5 is a good start. Below 0.4 or above 1 it rarely works."
- "**CR**, the crossover rate: 0.1 is the safe choice; 0.9 or 1.0 is for testing whether fast convergence is possible. A low CR changes few coordinates at a time, which works well when the variables are independent of each other."
- "And a tip from the paper: if the population converged too early, increase F or NP."
- "In our example: NP = 20, F = 0.5 and CR = 0.9."

**Transition:** "These parameters help understand how DE balances exploration and refinement."

### Slide 14 — Diversification and intensification · ≈ 1.5 min
**Goal:** answer the assignment's question.

**What to say:**
- "**Diversification** is exploring new regions. **Intensification** is refining the good regions. Every algorithm needs to balance the two."
- "In DE there is no separate knob for each one. The balance comes from the structure itself."
- "At the start, the solutions are spread out, the differences are large and so are the jumps: DE explores."
- "Over the generations, selection discards the worst ones, the population concentrates in the best valleys, the differences get short and so do the steps: DE refines."
- "In flat regions the opposite happens: the differences grow and the search speeds up. The step adjusts itself."

**Transition:** "Let's look at one generation with real numbers."


### Slide 16 — The problem: the Rastrigin function · ≈ 1.5 min
**Goal:** present the test problem.

**What to say:**
"We use Rastrigin with two variables. It is a parabola with ripples on top. The ripples create 121 local minima, one near each point with integer coordinates. The only global minimum is 0 and sits at (0, 0). It is also part of the 1997 paper's test set, function f13. There the domain was much larger; here we use −5.12 to 5.12 so that we can see the population on the function."

**Transition:** "To run it, we used the pymoo library."

### Slide 17 — Implementation with pymoo · ≈ 2.5 min
**Goal:** show the code and the precautions.

**What to say:**
- "pymoo already has DE built in. But its defaults are not the paper's, so we changed four things:"
- "The variant: pymoo's default is DE/best/1/bin; we switched to DE/rand/1/bin."
- "CR: the default is 0.2; we used 0.9."
- "prob_mut = 0: turns off an extra mutation that pymoo applies after crossover and that does not exist in the original algorithm."
- "n_gen = 101: pymoo counts the initial population as generation 1, so that is 100 DE generations and 2020 function evaluations."
- "We also tested other libraries: pygmo has no package for ARM Macs, and mealpy does not use j_rand in its crossover."

**Transition:** "Let's see what happened to the population."

### Slide 18 — The population over the generations · ≈ 3 min
**Goal:** show the shift from exploring to refining.

**What to say:**
- "The figure shows the population at four moments. In generation 0, the 20 individuals are spread across 19 different valleys."
- "The table counts how many valleys, which we call basins, the population occupies. That number drops from 19 to 14, 12, 9, 3 and reaches 1 at generation 42."
- "From generation 42 on, everyone is in the valley of the global minimum. From then on, DE only refines."
- "The best value of f drops from 25 to 3.55 × 10⁻¹⁵, practically zero."
- "This is diversification turning into intensification, as we saw on slide 14."

**Transition:** "This is even clearer in motion."

### Slide 19 — Differential Evolution in motion · ≈ 2 min
**Goal:** let the animation speak.

**What to say:**
"I'll let the animation run once; it takes about 12 seconds. Each orange dot is a solution. Watch three phases: up to generation 20, long jumps explore the whole space; between 20 and 42, the population leaves the neighboring valleys and moves to the central one; after that, the steps get smaller and smaller and the population becomes almost a single point at the origin."

**Transition:** "Finally, the experiment's numbers."

### Slide 20 — Convergence and final numbers · ≈ 3 min
**Goal:** close the experiment with data.

**What to say:**
- "The best value found was 3.55 × 10⁻¹⁵ at generation 100. For practical purposes, it is zero."
- "Reaching f below 10⁻⁶ took 1240 evaluations, at generation 61, out of 2020 in total."
- "In the left plot, the best value drops in steps, because it only changes when someone better appears. The mean drops continuously."
- "The right plot is the most interesting one: it shows how spread out the population is. That spread is also the size of the mutation step. It drops 100 million times, from 4 to almost 2 × 10⁻⁸, without any manual tuning."
- "The sharp drop near generation 40 is the moment the last individuals leave the neighboring valleys."

**Transition:** "With that, on to the conclusions."

### Slide 21 — Strengths and limitations · ≈ 1.5 min
**Goal:** give an honest assessment.

**What to say:**
- "Strengths: it is simple, with few lines of code and three parameters; it does not need a derivative and handles many local minima well; the NP solutions can be evaluated in parallel; and the step adjusts itself."
- "Weaknesses: the result depends on F, CR and the chosen variant; the population can stagnate or converge too early; problems with integer variables or constraints need adaptations; and there is no guarantee of finding the global optimum. The *no free lunch* theorem says no method is the best on every problem."
- "Many of these limitations became research topics, and that is what the next slide shows."

**Transition:** straight to slide 22.

### Slide 22 — How DE has evolved since 1997 · ≈ 2.5 min
**Goal:** show the improvements and their gains (source: the survey by Reyes-Davila et al., 2025).

**What to say:**
- "A survey published in 2025 gathered 113 works that modified DE. They organized everything by the four steps we just saw."
- "**Initialization:** instead of sampling the population at random, start with it better spread out, using chaotic maps, opposite points or clustering. Gain: convergence gets faster in most proposals."
- "**Mutation**, where the best-known improvement is: instead of fixing F and CR by hand, the algorithm learns these values from the attempts that worked. In addition, the guide is drawn from among the best individuals. These are JADE and SHADE. Gain: no more trial and error, and L-SHADE, which also shrinks the population during the run, won the CEC 2014 competition."
- "**Crossover:** CR starts changing during the search, and there are versions that swap coordinates along rotated axes. Gain: it works better when the variables depend on each other."
- "**Selection:** besides the one-to-one duel, there are elitist selections, others that look at the population's diversity, and others that reuse discarded solutions. Gain: less stagnation and faster convergence."
- "The trend the survey points out: mutation and crossover have already been studied a lot. Initialization and selection are only 20% of the works, and the authors point to them as the path for future research."

**Transition:** "Those were the improvements. To close, the references."

### Slide 23 — References · ≈ 0.5 min
**Goal:** end the talk and open for questions. There is no closing slide, so this one stays on screen during the questions.

**What to say:**
"These are the references I used, including the 2025 survey. To close, the main idea: DE turns differences between solutions into search steps, long when the population is spread out and short when it comes together. The full experiment is in the assignment's notebook. Thank you, I'm happy to take questions."

Likely answers are in Part IV.

---

## Part II — Explaining the algorithm (for study)

### The idea in one sentence
DE moves through the search space using **the difference between two solutions in the population** as its step. Since the solutions get closer over time, the step shrinks by itself.

### The elements
- **Solution (individual):** a vector x = (x₁, …, x_D) of real numbers.
- **Population:** NP vectors. The size stays fixed in classic DE.
- **Objective function f:** what we want to minimize. DE only needs to compute f(x); it does not need a derivative.

### Step 0 — Initialization
Each coordinate is sampled uniformly between the lower and upper bound:

x_{i,j} = min_j + rand(0,1) · (max_j − min_j)

### Step 1 — Mutation (DE/rand/1)
For each target xᵢ, sample three indices that are different from each other and from i:

v = x_{r1} + F · (x_{r2} − x_{r3})

- (x_{r2} − x_{r3}) is the **difference vector**: it gives the direction and size of the step.
- F (usually between 0.4 and 1) stretches or shrinks that step.
- x_{r1} is the **base vector**, the starting point.

**Why it works:** with the population spread out, the differences are large and the algorithm explores. With the population concentrated, the differences are small and the algorithm refines. There is no need to pick the step size by hand.

### Step 2 — Binomial crossover
For each coordinate j:

u_j = v_j if rand_j ≤ CR or j = j_rand; otherwise u_j = x_{i,j}

- CR is the chance of each coordinate coming from the mutant.
- j_rand guarantees that at least one coordinate comes from the mutant.

### Step 3 — Selection
xᵢ in the next generation = u if f(u) < f(xᵢ); otherwise, xᵢ stays.

It is an individual, greedy contest: the quality of each position never gets worse, and no solution takes over the population at once.

### The three parameters
| Parameter | What it controls | Suggested starting value |
|---|---|---|
| NP | how many solutions exist (diversity × cost) | 5D to 10D, at least 4 |
| F | step size | 0.5 |
| CR | how many coordinates change at a time | 0.1 (safe) or 0.9 (fast) |

Rule of thumb: if the population converged too early, increase F or NP.

### Numerical example from slide 15, step by step
1. Target: (2.805; −0.626), f = 31.887.
2. Difference r2 − r3 = (−2.796 − 2.643; 1.739 − (−1.490)) = (−5.439; 3.229).
3. F · difference = 0.5 · (−5.439; 3.229) = (−2.720; 1.615).
4. Mutant v = base r1 + step = (0.559 − 2.720; −4.467 + 1.615) = (−2.161; −2.852).
5. Crossover (CR = 0.9): rand₁ = 0.374 ≤ 0.9 → x₁ comes from v; rand₂ = 0.987 > 0.9 and j_rand = 1 → x₂ comes from the target.
6. u = (−2.161; −0.626), f = 26.770.
7. Selection: 26.770 < 31.887 → u replaces the target.

### Diversification × intensification
| Phase | What happens | Why |
|---|---|---|
| Start | large jumps across the whole space | spread-out solutions → long differences |
| Middle | population leaves the bad valleys | greedy selection discards the worst |
| End | tiny steps near the optimum | clustered solutions → short differences |

In the experiment: the occupied valleys drop from 19 to 1 by generation 42, and the population's spread drops 10⁸ times by generation 100.

---

## Part III — How DE evolved (for study)

### Timeline of the best-known variants
| Year | Variant | What it introduced | Gain |
|---|---|---|---|
| 1997 | DE/rand/1/bin | original algorithm | simple and robust; beat other methods in the paper's tests |
| 2006 | **jDE** (Brest et al.) | each individual carries its own F and CR, which are occasionally resampled; values that produce good offspring survive | no need to tune F and CR by hand |
| 2009 | **SaDE** (Qin et al.) | keeps several mutation strategies and chooses the one that has been most successful recently | the algorithm chooses the strategy by itself |
| 2009 | **JADE** (Zhang and Sanderson) | *current-to-pbest/1* mutation: the guide is drawn from the top p%, not just the best; archive of discarded solutions; F and CR learned from successes | converges fast without losing diversity |
| 2013 | **SHADE** (Tanabe and Fukunaga) | a "memory" of the F and CR values that worked in recent generations | more stable adaptation than JADE's |
| 2014 | **L-SHADE** | SHADE + a population that shrinks linearly during the run | won the IEEE CEC 2014 competition |

**Summary of the evolution:** the biggest weakness of the original DE is its dependence on F, CR and the choice of variant. The most successful versions attacked exactly that: the algorithm **learns** the parameters while it runs.

### What the 2025 survey says (Reyes-Davila et al.)
The paper analyzes 113 works (63 direct research papers and 50 related ones), organized by DE's four steps. To compare the proposals, it uses three efficiency criteria (from Beyer): **convergence speed**, **ability to find the optimum across different problems** and **computational cost**.

**1. Initialization** (about 15% of the proposals)
- Ideas: start with a better-distributed population. Examples: opposite points (ODE and variants), chaotic maps (CIDE), Latin hypercube (SLADE-SLHD), k-means clustering (CBPI, DEc), a population split and adjusted during the search.
- Gain: almost all of them speed up convergence; about half reduce the computational cost.
- Highlights: **Cumu-DE** and **CODE** (opposition + chaos) were the only ones that met all three criteria.

**2. Mutation** (the most studied step)
- The survey splits the strategies into two groups:
  - **Random** (DE/rand/1, DE/rand/2, current-to-rand): explore more and help escape local minima, but can be slow.
  - **Guided** (DE/best, current-to-best, current-to-pbest, rand-to-best, triangular mutation, historical mutation and others): move toward the best solutions. They are faster, but can stop early if the guide is in a local minimum.
- Strong trend: **combining several strategies**, either by picking the one that has been working best (SaDE, EPSDE) or by splitting the population into groups with different strategies.
- A reminder from the survey: no mutation strategy is the best on every problem (*no free lunch*). The same operator can do badly in one algorithm and very well in another, as happened with current-to-pbest in L-SHADE.

**3. Crossover**
- The classic ones are **binomial** (coordinates drawn one by one) and **exponential** (a contiguous block of coordinates). Exponential does well on problems with many variables.
- Limitation of binomial: it depends on the axes. If the function is "rotated", that is, if the variables depend on each other, it loses efficiency.
- New ideas: CR adjusted or repaired during the search, **eigenvector** crossover (swaps coordinates along axes aligned with the population, which solves the rotated-function problem), orthogonal crossover, and crossover that picks among options according to the population's diversity.
- Gain: more robustness on problems where the variables interact.

**4. Selection** (only about 2% of the proposals)
- Greedy one-to-one selection barely changed for years, because DE's big contribution was its mutation.
- New ideas: elitist selection (merges parents and offspring and picks the best), selection that considers the population's diversity (**IDEI**), subset-based selection (STS), and reusing discarded vectors when the search stagnates.
- Gain: less stagnation and faster convergence. **IDEI** was the only proposal that did well on all three criteria.

**The survey's conclusion and trend**
- The number of new mutation and crossover schemes is falling; interest in **initialization and selection** is growing.
- These two steps account for only **about 20%** of the works, but they have been shown to improve DE. The authors point to them as the main path for future research.
- The literature lacks a comparison of all proposals on the same benchmark; each author uses their own, which makes it hard to say which is best.

---

## Part IV — Glossary and likely questions

### Quick glossary
| Term | Plain meaning |
|---|---|
| Local minimum | a valley that looks like the bottom but is not the lowest |
| Global minimum | the lowest point of the whole function |
| Multimodal function | a function with many valleys (many local minima) |
| Meta-heuristic | a general search strategy that does not guarantee the optimum but usually finds good solutions |
| Target vector | the solution we are trying to improve |
| Mutant vector (v) | the result of mutation |
| Trial vector (u) | the result of crossover; competes with the target for its slot |
| Greedy | only accepts changes that improve |
| Diversification | exploring new regions of the space |
| Intensification | refining the regions that already look good |
| Basin (of attraction) | the region around a minimum; whoever starts in it tends to descend to it |
| Separable function | the variables can be optimized one at a time, independently |
| *No free lunch* | theorem: no method is the best on every problem |
| CEC | IEEE's yearly evolutionary algorithms competition |

### Likely questions
- **Why not use gradient descent?** Rastrigin has 121 local minima. A method that only goes downhill stops in the valley closest to its starting point.
- **Why CR = 0.9 if Rastrigin is separable?** We followed the paper's suggestion of testing 0.9 for fast convergence. A low CR would also work well here.
- **How does DE handle constraints?** With penalties in the objective function or with rules that prefer feasible solutions, as pymoo does.
- **How do you choose F and CR for another problem?** Start from the rules on slide 13 or use an adaptive variant, such as JADE or SHADE, which learns these values by itself.
- **What is the difference between DE and a genetic algorithm?** In DE, the solution is a vector of real numbers, and mutation uses the difference between solutions in the population. In the classic GA, the solution is usually binary and mutation is a fixed random flip. Also, in DE mutation comes before crossover, and selection is a one-to-one duel.
- **Which variant is the best today?** There is no single best for everything (*no free lunch*). In competitions, the SHADE/L-SHADE family usually does very well.
- **What has changed since 1997?** Mainly the automatic adaptation of F, CR and the strategy (jDE, SaDE, JADE, SHADE, L-SHADE). According to the 2025 survey, the next frontier is initialization and selection.
