# SCiEL

**Let them get better at getting better: recursive self-improvement through cultural transmission.**

[![PyPI](https://img.shields.io/pypi/v/sciel?color=f2c94c)](https://pypi.org/project/sciel/)
[![MIT license](https://img.shields.io/badge/license-MIT-9cc3f2)](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/blob/main/LICENSE)
[![Python 3.11–3.14](https://img.shields.io/badge/python-3.11%E2%80%933.14-3776ab)](#quick-start)
[![Blog post](https://img.shields.io/badge/blog-sciel--rsi--cpil.github.io-1f5fc9)](https://sciel-rsi-cpil.github.io/)
[![Results](https://img.shields.io/badge/public_runs-2%2C011-4cc26a)](https://sciel-cpil.github.io/sciel-results/)
[![Empira](https://img.shields.io/badge/Empira-2%2C260_tasks-64d2ff)](https://github.com/SCiEL-RSI-CPIL/empira-rsi-arena)

SCiEL is a **recursive self-improvement** system that learns from the experience it produces itself. It keeps the
language model fixed and improves everything around it: the findings it tests, the tools it writes and the way it
chooses what to try next. Its programs evolve in *demes*, groups that work on a problem in parallel. When one deme
learns something useful, SCiEL tests it and passes it to the other demes as a *finding*; at the end of a run, it passes
the repertoire to the next run, which tests each item again before it trusts it.

- **A small open model beats frontier models.** With SCiEL, an open 27B model on two GPUs sets new records on hard
  scientific problems. It beats results that Google DeepMind and others reached with closed frontier models such as
  Gemini, Claude and GPT. One of its new values is already merged into Terence Tao's list of optimization constants.
- **Open for everyone to join.** Our arena, [Empira](https://github.com/SCiEL-RSI-CPIL/empira-rsi-arena), holds 2,260
  scientific tasks in 11 fields, from mathematics and physics to biology, chemistry, robotics and finance. It is open
  together with this code and 2,011 [public runs](https://sciel-cpil.github.io/sciel-results/). Anyone can run SCiEL on
  their own problem today: `pip install sciel`.

Read the blog post: **[Let Them Get Better at Getting Better](https://sciel-rsi-cpil.github.io/)**.

<p>
  <a href="https://scholar.google.com/citations?user=-3GbddYAAAAJ">Arina Kharlamova</a>,
  <a href="https://scholar.google.com/citations?user=-hj96MMAAAAJ">Jikun Kang</a>,
  <a href="https://scholar.google.com/citations?user=shacGAsAAAAJ">Yusheng Zhao</a>,
  <a href="https://en.wikipedia.org/wiki/Steve_Liu">Xue Liu</a>,
  <a href="https://scholar.google.com/citations?user=1cH0A9cAAAAJ">Bowei He</a>
  <br>
  <a href="https://mbzuai.ac.ae/"><img src="https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/raw/assets/brand-mbzuai.png" alt="MBZUAI logo" width="40" height="40" align="middle"></a>
  <a href="https://cpil-lab.github.io/"><img src="https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/raw/assets/brand-cpil.png" alt="CPIL logo" width="40" height="40" align="middle"> MBZUAI CPIL Research Group</a>
</p>

[Results](#results) · [How SCiEL works](#how-sciel-works) · [Tasks it is for](#what-tasks-sciel-is-for) ·
[Quick start](#quick-start) · [Components](#components) · [Models](#local-inference-or-an-api) ·
[Citation](#citation) ·
[Contributing](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/blob/main/CONTRIBUTING.md)

[![A run of SCiEL. Left: demes as sectors, lineages as trees, findings passed between demes as crimson chords. Right: the same programs placed by their code, shaded by score](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/raw/assets/sciel-hero.png)](#see-it-run)

## Results

Every result below comes from one open model, **Qwen3.8-27B**, which we run on our own machines (2× H100) and do not
train during a run. The previous bests were reached with closed frontier models (Gemini ensembles, Claude, GPT-6
Astra), a 120B model trained with RL, or decades of specialist records. We check each result outside the run, and the
status column says where it stands. New results appear first on the
[results page](https://sciel-cpil.github.io/sciel-results/).

| Problem | SCiEL | Previous best | Reached with | Status |
| --- | --- | --- | --- | --- |
| Autocorrelation constant C<sub>1d</sub>, upper bound | **NEW SOTA** [**1.457854885**](https://github.com/teorth/optimizationproblems/pull/248) | 1.459989047 (2026) | Google DeepMind, AlphaEvolve: Gemini 2.0 Flash and Pro | merged |
| Ramsey multiplicity of K<sub>6</sub> (89c), upper bound | **NEW SOTA** [**0.7422042116·2⁻¹⁴**](https://github.com/teorth/optimizationproblems/pull/331) | 0.7422548616·2⁻¹⁴ (2026) | Claude Code; before it, Franek's 2002 bound stood for 24 years | in review |
| Maximal-function constant 47, lower bound | **NEW SOTA** [**1.7032090736**](https://github.com/teorth/optimizationproblems/pull/341) | 1.70311 (2026) | a researcher with GPT-6 Astra and Claude Opus 5.5 | in review, raised since |
| Thick knot 22b, upper bound | **NEW SOTA** [**7.087662112**](https://github.com/teorth/optimizationproblems/pull/342) | 7.090412 (2026) | a researcher; the bound before it was found with Claude | in review, lowered since |
| Circles in rectangles, seven aspect ratios | **NEW SOTA** **700+ sizes** | Packomania tables (2010) | packing specialists over decades | verified |
| Circles in an isosceles right triangle | **NEW SOTA** **101 of 150 sizes** | Packomania table (2011) | packing specialists | verified |
| Circles in a quarter disc | **NEW SOTA** **392 sizes** | Packomania table (2011) | packing specialists | verified |
| Circles in a semicircle | **NEW SOTA** **116 of 151 sizes** | Packomania table (2026) | including S. Hogan's records of September 2026 | verified |
| Circles in a pentagon | **NEW SOTA** **85 of 101 sizes** | Packomania table (2026) | the table as of October 2026 | verified |
| Spherical codes, dimensions 8 to 14 | **NEW SOTA** **42 of 49 codes** | H. Cohn's table (2026) | best known codes as of October 2026 | verified |
| 16 points in the plane, max/min distance ratio | **NEW SOTA** **12.889230** | 12.889266 (2025) | Google DeepMind, AlphaEvolve: Gemini 2.0 Flash and Pro | re-checked |
| 26 circles in a square, sum of radii | **MATCHED** **2.6359831** | 2.6359831 (2025) | TTT-Discover: a 120B open model trained with RL during the run, 25,600 samples | re-checked |
| Lennard-Jones clusters, 38 and 55 atoms | **MATCHED** known minima | global minima (1990s) | Cambridge Cluster Database | re-checked |

*New SOTA*: better than the best published value at the time. *Verified*: checked in exact arithmetic, not yet
submitted to the table's maintainers. *In review*: an open pull request to the list of optimization constants;
*raised since* / *lowered since*: another group has since improved on our bound.

With the same model and budget as OpenEvolve and ShinkaEvolve, SCiEL reaches higher scores and needs about half as
many evaluations. On circle packing it matches the best known value from scratch in 370 model calls for $1.92;
TTT-Discover reached the same value with a model 4.4 times larger that it trained during the run.

## How SCiEL works

A system improves *recursively* when its improvements **make it better at improving**. Around the frozen model, SCiEL
builds a repertoire: findings, tools and learned rules for choosing the next edit.

![How SCiEL works. Run: one attempt in every deme; a planner draws the choices from logged odds; a frozen LLM writes the child; novelty rejects repeats; an exact check scores it. Learn: controllers learn the odds from the journal. Share: research cells write and run their own tools; a finding enters the repertoire only after a trial in the receiving deme; the next run inherits the repertoire and re-tests each item.](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/raw/assets/sciel-method.png)

For each attempt, SCiEL draws its choices with learned odds, the frozen model writes a program, and an exact checker
scores it. SCiEL records every choice and its odds in a journal and learns the odds of the next choices from it. A
finding enters the repertoire $R$ only after a randomised trial in the deme that receives it. Because the next attempt
depends on $R$, and $R$ changes only after a test, the process is not a loop that repeats itself. The animated version
is in the [blog post](https://sciel-rsi-cpil.github.io/#method).

**Every attempt is a handful of choices:** which program to start from, what kind of edit to ask for, which finding to
show, which strategy to follow. SCiEL draws each choice at random with set odds and learns the odds from its own
record of what worked. Three layers set the odds, and a layer takes over only when it does better than the layer below
on attempts it has not seen:

| Data so far | Who sets the odds | What it learns |
| --- | --- | --- |
| from the first attempt | **sliding-window softmax bandits** | kinds of edit and parents from recent rewards; it forgets old successes, and a probability floor keeps every choice logged with usable odds |
| ~60 attempts | **world model, TabPFN stage** | whether an attempt will improve its line, from a pretrained tabular model plus the rows of earlier runs |
| a run's worth | **world model network** | a residual on that forecast, with code embeddings |
| 200+ attempts | **RL step policy** | a small, bounded shift of the odds of five choices; two critics by deme parity; acts only above two standard errors |
| always | **theory** | e-processes test every choice while the run goes; e-BH bounds false discoveries; proven choices tilt the odds |

SCiEL still makes one choice in ten at random and records the odds of every choice, so a lucky streak cannot lock in
the run and the record stays a fair experiment. The same experiment tests culture: when a deme writes its next prompt,
SCiEL flips a weighted coin to decide whether to include a finding from another deme. Across 31 runs and 1,022 such
decisions, **receiving a finding raises the chance that an edit beats its parent from 54% to 62%** (95% CI +1.7 to
+13.7 points).

Around the odds:

- **Research cells.** Persistent questions that write tools, run them in Docker on the attempt's frozen inputs, revise
  them, submit candidates and pass methods to other demes after a trial in the receiving deme.
- **Culture across runs.** A run exports its repertoire: findings, tools, step policy and world-model rows. The next
  run inherits each item as one more randomized choice and keeps it only if it wins again.
- **Machinery.** Demes (called islands in the code) with MAP-Elites, learned grids or VQ niches; run-wide novelty by
  hash, AST and output; a smoke → full → confirm check that keeps the worst repeat; a view of the population and task
  modules a candidate imports while it runs; parallel waves with shared capacity; checkpointed, replayable recovery.

## What tasks SCiEL is for

SCiEL is built for **long climbs**: problems where a single edit rarely helps, progress takes hundreds of
evaluations, and what the run learns early (tools, tested findings, which moves pay off) keeps paying later. On a task
that 150 samples saturate, there is nothing for culture to compound on.

We make this precise. A task is a tuple $\langle X, F, h, h^{*}, M, B \rangle$: candidates $X$ the model writes, a
feasible set $F$, the objective $h$ the run sees and the true objective $h^{*}$, the model's edit operator $M$, and
the evaluation budget $B$. Its landscape is defined by $M$, so it moves as culture changes the context $M$ sees. The
key quantity is the chance that an edit at the frontier beats its parent by more than the noise radius $\varepsilon$:

```math
\pi_\varepsilon(x) = \Pr_{x' \sim M(\cdot \mid x)}\left[\, h(x') > h(x) + \varepsilon \,\right],
\qquad \mathbb{E}[\text{evaluations to leave a plateau}] \approx 1/\pi_\varepsilon .
```

A task is admitted to a run when its check is exact ($h = h^{*}$, no exploitable tolerance), the headroom between the
best reference and a known bound exceeds $4\varepsilon$ at the planned budget, and the upper confidence bound of
$\pi_\varepsilon$ at the frontier is at least $5/B$. The effect of transmission is expected exactly where
$\pi_\varepsilon$ at the frontier is **small but nonzero**: rare improvements, a long run, and a culture that raises
$\pi_\varepsilon$ for the next step. `sciel task` measures
the noise radius and the headroom before a run starts.

### In plain words

You do not need to write the task yourself. Give **Claude Code** or **Codex** the
[`sciel-task` skill](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/tree/main/skills/sciel-task) and your materials (a description, a paper, a notebook, data) and ask it to
solve the problem with SCiEL:

```sh
cp -r skills/sciel-task ~/.claude/skills/    # Claude Code
cp -r skills/sciel-task ~/.codex/skills/     # Codex
```

The skill:

1. **formulates** the task: what is being improved, how it is scored, the best known result to beat;
2. **builds** it for SCiEL: an exact checker, helper functions the candidates can import, a seed that searches;
3. **audits** it before spending compute: leaks and exploits, noise, headroom, and how often an edit improves;
4. **fixes** what the audit finds, **runs** SCiEL, and **re-checks** the best solution independently.

Problems that fit share three traits: a result you can check exactly or on held-out data, room to improve beyond
the best known answer, and an answer that improves step by step over hundreds of tries rather than being found in
one go.

| Field | Example task | What SCiEL improves | How it is checked |
| --- | --- | --- | --- |
| Finance | Executing a large order over a day with the least slippage | the rule that splits the order over time | average cost on held-out trading days |
| Mathematics | Bounds on constants, packings, extremal configurations | the construction | exact arithmetic |
| Algorithms | A scheduling or bin-packing heuristic | the heuristic's code | cost on a fixed set of instances |
| Science | Decoding behaviour from recorded neural activity | the decoder's code | accuracy on a held-out regime |
| Engineering | A faster numerical kernel | the implementation | speed, with outputs equal to a reference |

## See it run

`sciel watch` follows a run in the terminal; `sciel web` opens the same run as a local page. Both read the run's
journal, so they work on a live run or a finished one. Shown: the run behind the C<sub>1d</sub> record above, with a local
Qwen3.8-27B.

![The terminal viewer: best score, islands, attempts with their diffs, research cells and what the search learned](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/raw/assets/sciel-terminal.png)

| | |
| --- | --- |
| ![Run: attempts, failures and the selected diff](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/raw/assets/sciel-web_run.png) | ![Maps: the culture disk and the behaviour landscape](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/raw/assets/sciel-web_maps.png) |
| **Run.** Every attempt, why candidates fail, the idea and diff of each step. | **Maps.** The culture disk: islands as sectors, lineages as trees, findings passed between islands as chords. |
| ![Engine: how the run started and every island's attempts](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/raw/assets/sciel-web_engine.png) | ![What it learned: findings and where they were delivered](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/raw/assets/sciel-web_what.png) |
| **Engine.** How the islands were seeded and every attempt of every island. | **What it learned.** Findings of research cells and improvements, and where each was delivered. |

## Quick start

> [!TIP]
> **The easiest start is the skill.** Install [`sciel-task`](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/tree/main/skills/sciel-task) in Claude Code or Codex, open your
> materials and say *"solve this with SCiEL"*. It formulates the task, audits it, runs SCiEL and checks the result.
>
> ```sh
> git clone https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework
> cp -r sciel-rsi-framework/skills/sciel-task ~/.claude/skills/    # Claude Code
> cp -r sciel-rsi-framework/skills/sciel-task ~/.codex/skills/     # Codex
> ```

Requires **Python 3.11–3.14** on Linux or macOS.

```sh
pip install sciel
sciel
```

`sciel` alone finds a model key (`ANTHROPIC_API_KEY` or `OPENROUTER_API_KEY`, or asks once), takes the default
profile and asks what to improve: the bundled circle-packing demo, a program of yours, or a run to continue.

**Your own problem** is one Python file:

```python
def solve():                # SCiEL rewrites this
    return [0.0, 0.0]

def score(result) -> float: # fixed during the run; higher is better, valid results score above 0
    x, y = result
    return 1 / (1 + (x - 1.7) ** 2 + (y + 0.7) ** 2)
```

```sh
sciel solve.py --budget 40                 # improve
sciel task ./materials --note "..."        # draft a task from papers, notebooks or data, then audit it
sciel watch .sciel/runs/<run>              # terminal viewer; w opens the page, p pauses, q leaves it running
sciel stop .sciel/runs/<run>               # finish the work in flight, save and stop
sciel resume .sciel/runs/<run>
```

`sciel task` has a model draft `problem.py`, runs and repairs the seed, and audits repeatability, the noise floor and
the headroom between the seed and a reference before any run starts. A mock model needs no credentials:
`sciel run configs/smoke_mock.yaml --tasks-root ./tasks --budget 6 --watch none` from a source checkout with
`pip install -e '.[tasks]'`.

## Project layout

```
src/sciel/
  engine/          the run: step planning, generation, evaluation, islands, checkpoints, parallel waves
    llm/           providers: OpenAI-compatible (vLLM, OpenRouter), Anthropic, Codex, Claude Code CLI, mock
  eval/            task loading, sandboxed stages, output identity and certification
  causal/          decision ledger, doubly robust estimators, e-processes
  theory/          claims, experiments, strategies, findings and procedures
  swm/             world model: TabPFN stage, residual network, niches, archive
  rl/              step policy: critics, returns, KL-bounded improvement
  research_tools/  research cells: questions, tools, experiments, sharing between islands
  methods/         isolated tool execution in Docker, tool registry, method exchange
  culture/         the repertoire one run exports and the next inherits
  components/      selection, population containers, learned policies
  ui/ cli/         terminal and web viewers, command line
  taskbuilder/     `sciel task`: draft and audit a task from materials
tasks/             task packages: evaluator.py, initial_program.py, task.yaml, optional assets/
configs/           maintained profiles
skills/sciel-task/ an agent skill for Claude Code and Codex: formulate, audit, run and verify a task
```

## Engine

One step of a run, as `engine/step_plan.py` and `engine/step_cycle.py` run it:

```python
def step(island):
    ctx = context_of(engine)                                   # phase, progress, island state
    parent = sample("parent", selection(island))               # every sample() is logged with its probability
    planner = sample("planner", theory.tilt({"base": .., "swm": .., "rl": ..}))
    operator = sample("operator", steered(planner, bandit.operator_probs(ctx)))   # diff, rewrite, cross, idea, repair
    hints = sample("finding", steered(planner, knowledge.offer(island)))          # tested findings, strategy cards
    refs = freeze_population(island, parent)                   # what the child may read while it runs
    child = llm.generate(prompt(task, parent, operator, hints, describe(refs)))
    if novelty.repeat(child): return reject(child)             # SHA-256 and AST repeats, run-wide
    result = cascade(child, stages=["smoke", "full", "confirm"], population=refs)  # confirm keeps the worst repeat
    islands[island].admit(child, result)                       # MAP-Elites cell, eviction, migration
    ledger.record(decisions, outcome=result)                   # feeds theory, world model and step policy
```

| | Parent choice | Edits | Learns during the run | Methods outlive candidates |
| --- | --- | --- | --- | --- |
| FunSearch | islands, score-weighted | whole function | no | no |
| AlphaEvolve | MAP-Elites + islands | diffs, Gemini Flash/Pro ensemble | no | no |
| OpenEvolve | MAP-Elites + islands, fixed exploit ratio | diffs | no | no |
| ShinkaEvolve | sigmoid(fitness) × 1/(1+children) | diffs, rewrites, crossover | UCB1 over LLMs | meta-scratchpad text |
| AdaEvolve | per-island intensity | diffs | UCB over islands; LLM meta-guidance on stalls | no |
| **SCiEL** | islands + learned grids, tilted by the planners | diffs, rewrites, crossover, ideas, repair | every choice: bandits, world model, step policy, tested claims | **tools, findings and models, with trials** |

**Why it is built this way.** The systems above adapt one or two knobs from a heuristic reward. They cannot say
whether an adaptation helped, because their choices are not logged with probabilities. AdaEvolve's ablation shows the
lever that matters most is its LLM meta-guidance (2.629 → 2.521 on circle packing without it), yet that guidance
is untested text. SCiEL randomizes every choice and logs it, so each adaptation, including a model's advice, is a
treatment with a measurable effect, and it is kept only while the effect holds.

## Components

### Setting the odds: bandits and the step policy

```python
# engine/operators.py, components/policy/mode_bandit.py: the cold start
p_op   = floor(softmax(max(reward of the arm's last 20 outcomes) / T), 0.05)    # best case, recent
p_mode = softmax(mean(reward of the mode's last 50 outcomes, warm-started from a prior) / T)
# rl/policy.py: one KL-bounded improvement over the logging policy per decision
Q_even, Q_odd = fit_critic(rows by island parity)              # each scores the other half's rows
adv = pessimistic(Q) - E_mu[pessimistic(Q)]                    # centred on the logged policy
pi = mu * exp(eta * adv), with KL(pi || mu) <= 0.2              # eta searched in units of the advantages
act only if ESS >= 200 and V_oof(pi) > V(mu) + 2 * SE_clustered_by_parent
```

Others use a UCB-style bandit from the first step, with rewards like ShinkaEvolve's `exp(max(Δ, 0)) − 1`. SCiEL's
bandits differ on two points. A **sliding window** forgets old outcomes, because which edit pays off changes as the
population matures. A **softmax with a probability floor** replaces UCB's near-deterministic argmax, so every choice
has a known, nonzero probability and the ledger supports off-policy estimates; UCB1 and sliding-window UCB remain as
ablation arms (`configs/arms`). Parent selection uses the `novelty_chain` kernel, a score-temperature softmax that
also accepts the planners' tilt. Gains at
budgets of 50–800 calls are modest, and larger ones come from training the model itself on 10⁴–10⁵ samples
(EvoTune, ThetaEvolve). Hierarchical selection that values a node by its descendants (HGM) predicts progress far
better than the node's own score. SCiEL keeps online bandits for the cheap decisions and adds a **step policy over
step features**: it waits for 200 steps, values a step by the rise of the best along its lineage rather than the
child's own score, and moves only when an out-of-fold estimate clears its own error. No system we surveyed fits such
a critic with a warm-up and a gate.

### World model

```python
# swm/world.py, swm/tabular.py: forecast whether a step raises its lineage's best
reference = best_of(base_rate, cell_means, TabPFN(operator rows, inherited rows))  # out of fold by island parity
net = residual_network(code_embedding_pca16, step_features, start=reference)        # cannot forecast worse
if gate(net beats reference out of fold): planner arm "swm" tilts the operator choice
```

Most leading systems have no learned predictor of a step's success and rely on filters and evaluation cascades.
Where one exists it is trained once (LLaMEA-SAGE, used only as a prompt hint and not significant at n = 5) or scores
pairs before the call (DGS, 200 evaluations). SCiEL's model starts from a pretrained tabular model, so it can forecast
from a few dozen rows, inherits the operator rows of earlier runs, and **acts only as one randomized arm of the
planner**, so its effect on progress is measured like any other arm's.

### Theory: testing every choice while the run goes

```python
# theory/claims.py, causal/sequential.py: "this action changes the child's event rate"
for decision in FAMILY:                                         # operator, planner, origin, finding, ...
    e = mixture_of_bets(doubly_robust_contrasts(ledger, decision))   # an e-process, valid at any stopping time
claims = e_BH(all e-values, alpha=0.05)                        # false discovery rate under any dependence
theory.tilt(decision) shifts sampling toward proven actions, and keeps exploring the rest
```

Others report ablations after the fact. SCiEL tests its own choices **while it runs**, without peeking penalties, and
a claim enters a later run only as an inherited prior that is re-tested there.

### Research cells and tools

```python
# research_tools/: a question with an experimental history
cell = open_question(trigger=stall or schedule)                # "what structure of the task changes the design?"
while steps < max_steps:
    tool = llm.write("def run(payload): ...")                  # imports what a candidate imports
    obs = docker_run(tool, payload=frozen(parent, best), no_network=True, limits)
    decide(revise | another experiment | submit candidate | finish)  # finish only after a trial
share(tool, to=other islands) → recipient trial → adopted only if it ran there
```

AlphaEvolve and OpenEvolve keep no methods; ShinkaEvolve keeps a meta-scratchpad of summaries; AdaEvolve asks a model
for new tactics on stalls. None of them separates an untested idea from a measured one. In SCiEL a cell writes
**executable tools** that run in an isolated container on the frozen inputs of the step. Their observations carry
their source, and a tool reaches another island only through a recipient trial. The tool sees the same population
view and task modules a candidate sees, so a cell can measure exactly the object it is reasoning about.

### Population view and task assets

```python
# inside a candidate while it is evaluated
from sciel.population import population    # island best, parent and run best, with the exact scored outputs
from sciel.task import asset
steps = asset("steps")                      # the task's own objective, gradients and refinement
h = population()[0]["output"]["heights"]    # continue the best construction instead of rebuilding it
budget = float(os.environ["SCIEL_STAGE_SECONDS"])   # check fast in smoke, search in full and confirm
```

A child that can only read code rebuilds its parent's construction from scratch every time. Here it continues from
the measured object, and the request binds the view's hash, so a re-measurement reads the same view. On the third
autocorrelation constant, the view, the stage budget and the task's objective module together took the best bound
from about 1.55 to 1.462 after the same 50 evaluations, and to 1.461 by the end of the run.

### Culture across runs

```python
# culture/repertoire.py
export(run) = {claims, surviving strategies, findings, tools, step policy, world-model rows}
inherit(next_run): each item enters as one more randomized arm, and is kept only if it wins there
```

A Darwin or Huxley-Gödel machine changes the agent itself. SCiEL changes the **repertoire around the model**, so a
weaker model can inherit what a stronger run learned, and nothing inherited is trusted without a new trial.

## Local inference or an API

| Provider | Configuration | Used for |
| --- | --- | --- |
| Local vLLM (OpenAI-compatible) | `llm.api_base: http://127.0.0.1:8000/v1` | our runs: Qwen3.8-27B on 2× H100, data parallel, prefix caching |
| OpenRouter | `api_base: https://openrouter.ai/api/v1` | any hosted model, weighted ensembles |
| Anthropic API | `ANTHROPIC_API_KEY` | Claude models, the default of the zero-config start |
| Claude Code CLI | `provider: claude_code` | Claude on a subscription; pauses until a usage limit resets |
| Codex Responses | `provider: codex_responses` | OpenAI models through a Codex login |
| Mock | `configs/smoke_mock.yaml` | engine checks without a model |

A local model costs electricity rather than tokens, so a run can afford long full and confirm stages and many research
experiments. A transient provider failure pauses a parallel run and continues it; a lost request is never resent, and
its unknown charge stays in the record. Research tools need Docker and an immutable image; candidates run as native
subprocesses with resource limits, which bound resources but do not isolate files or network.

## Tasks

The repository holds **140 task packages**; the [Empira arena](https://github.com/SCiEL-RSI-CPIL/empira-rsi-arena)
holds 2,260 more in 11 fields. Each package defines its candidate interface, evaluator, metrics and limits.

| Family | Packages | What SCiEL improves |
| --- | --- | --- |
| [Packings](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/tree/main/tasks/math) | 27 | circles in rectangles of seven aspect ratios, an isosceles right triangle, a quarter disc, a semicircle and a pentagon; squares in a square |
| [Spherical codes](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/tree/main/tasks/math) | 66 | codes in dimensions 8 to 32 from H. Cohn's table, scored by their largest cosine in exact arithmetic |
| [Autocorrelation and minimum overlap](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/tree/main/tasks/math) | 10 | step functions bounding the first, second and both third autocorrelation constants and Erdős' minimum overlap, in exact rational arithmetic; variants start from the best earlier construction |
| [Ramsey multiplicity](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/tree/main/tasks/math) | 4 | constructions for K<sub>4</sub>, K<sub>5</sub> and K<sub>6</sub> |
| [Constants from Tao's list](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/tree/main/tasks/math) | 25 | Kakeya needles, knot ropelength, Shannon capacity of C<sub>7</sub>, sunflower-free capacity, difference bases, sum-difference sets and more |
| [Open-ended](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/tree/main/tasks/oe) | 6 | 26 circles in a square, the Heilbronn triangle, function minimization with a hidden oracle, matrix multiplication tensors |
| [Data](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/tree/main/tasks/data/tess_transits) | 1 | finding injected planets in real TESS light curves while rejecting eclipsing binaries |
| [Test](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/tree/main/tasks/test/toy_quadratic) | 1 | a fixture for engine and evaluator contracts |

A normalized score's denominator may be a fixed reference rather than a record; read `task.yaml` and the evaluator
before comparing results.

## Configure a live experiment

Start from a [profile](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/tree/main/configs) and choose the task, provider and models, budget, storage, research features and
limits. `budget` counts successful evolutionary evaluations; seeding and failed attempts are counted separately.
Checkpoints bind task, source and execution identities, so changing the engine can require a fresh run.

| Optional dependency | Purpose |
| --- | --- |
| `tasks` | SciPy for the numerical tasks. |
| `tabular` | TabPFN for the world model's tabular stage. |
| `analysis` | Dataframes and process-memory telemetry. |
| `board` | A Weights & Biases sink. |
| `dev` | Tests, schema validation and builds. |

Research tools require `parallel_search`, `candidate_novelty`, `llm.max_retries: 1` and an immutable Docker image;
see [integration validation](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/blob/main/src/sciel/config_validation.py) and the
[research-tool configuration](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/blob/main/src/sciel/research_tools/config.py). Learned profiles need external weights
(`embed_grid_path`, `embed_axis_path` or `embed_niche_path`); missing ones stop startup.

## Inspect runs in Empira

[Empira](https://github.com/SCiEL-RSI-CPIL/empira-rsi-arena) is the research board and experiment workspace. It reads the same run
evidence: progress, lineages, source changes, research cells, tools, model calls and artifacts.

```sh
python -m arena --root /absolute/path/to/sciel-runs
```

## Releases

`pip install sciel` installs the latest release from [PyPI](https://pypi.org/project/sciel/). Each
[GitHub release](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/releases) also provides the wheel, the source distribution, a separate task bundle and
SHA-256 checksums.

```sh
python3 -m venv .venv
.venv/bin/python -m pip install 'sciel[tasks]'
tar -xzf sciel-<version>-tasks.tar.gz
.venv/bin/sciel tasks list --tasks-root ./sciel-<version>-tasks/tasks
```

## Development

```sh
.venv/bin/python -m pip install -e '.[tasks,dev,analysis]'
.venv/bin/python scripts/check_repository.py
.venv/bin/python -m build
git diff --check
```

Maintained files have a **500-line limit** (we are old-school and can't read past 500 lines, and vibe-coded walls of code in one file are a pain to read), enforced by the repository check. Experiment outputs, model weights and
working notes live outside the source tree. See [CONTRIBUTING.md](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/blob/main/CONTRIBUTING.md) for implementation and
verification rules.

## License

SCiEL is released by Arina Kharlamova, Jikun Kang, Yusheng Zhao, Xue Liu, Bowei He and the MBZUAI CPIL Research
Group under the [MIT license](https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework/blob/main/LICENSE).

## Citation

```bibtex
@misc{cpil2026sciel,
  title  = {Let Them Get Better at Getting Better:
            Recursive Self-Improvement through Cultural Transmission},
  author = {Arina Kharlamova and Jikun Kang and Yusheng Zhao and Xue Liu and Bowei He},
  year   = {2026},
  url    = {https://github.com/SCiEL-RSI-CPIL/sciel-rsi-framework}
}
```
