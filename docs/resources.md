# Research Resources for LLM Agent Workflows

[← Back to SWIFT](../README.md)

A curated starting point for researchers studying **workflow synthesis, agent architecture search, reusable experience, and evaluation**. The relevance notes are our reading guide, not claims that these projects integrate with SWIFT. This is a focused selection, not an exhaustive survey or ranking.

**Last reviewed: September 9, 2026.** Links point to original papers, author repositories, or official documentation. Code links were inspected; external implementations have not been run or validated by this repository.

## Suggested reading path

1. **Start with AFlow and ADAS** to understand searching over code-represented workflows and agent designs.
2. **Read MaAS and AgentSquare** to compare query-dependent architecture selection with modular agent search.
3. **Read Agent Workflow Memory and SWIFT** to study how prior experience can be reused: retrieved routines for agent behavior versus demonstrations and distilled priors for workflow synthesis.
4. **Explore DSPy and TextGrad** for complementary approaches to optimizing the instructions and modules within an LM program.
5. **Choose an evaluation setting** from the benchmark resources below, then make the source/target split and cost accounting explicit before comparing methods.

## Workflow and agent architecture search

| Resource | Paper / code | Why read it? |
| :--- | :--- | :--- |
| **AFlow** | [Paper](https://arxiv.org/abs/2410.10762) · [Code](https://github.com/FoundationAgents/AFlow) | Monte Carlo tree search over code-represented workflows using execution feedback. SWIFT's primary search-based comparison; useful for understanding operators, workflow evaluation, and source search trajectories. |
| **ADAS** | [Paper](https://arxiv.org/abs/2408.08435) · [Code](https://github.com/ShengranHu/ADAS) | Meta Agent Search programs new agents using an archive of earlier discoveries. Useful for studying open-ended code-level design and transfer of discovered agents. |
| **MaAS** | [Paper](https://arxiv.org/abs/2502.04180) · [Code](https://github.com/bingreeky/MaAS) | Learns a probabilistic agentic supernet and samples query-dependent architectures. Useful for comparing per-query resource allocation with a synthesized task-level workflow. |
| **AgentSquare** | [Paper](https://arxiv.org/abs/2410.06153) · [Code](https://github.com/tsinghua-fib-lab/AgentSquare) | Searches a modular agent design space. Its module pools and interface descriptions are useful references for reusable planning, reasoning, tool-use, and memory components. |

## Reusing workflows and experience

| Resource | Paper / code | Why read it? |
| :--- | :--- | :--- |
| **Agent Workflow Memory (AWM)** | [Paper](https://arxiv.org/abs/2409.07429) · [Code](https://github.com/zorazrw/agent-workflow-memory) | Induces reusable routines from experience and provides them to agents on subsequent tasks, including web navigation. Useful for studying offline and online workflow memory. |
| **SWIFT** | [Paper](https://arxiv.org/abs/2604.25012) · [Project overview](../README.md) | Distills source trajectories into heuristics and output contracts, then synthesizes a target workflow from cross-task demonstrations in one generation pass. The implementation release is in preparation. |

**A useful comparison axis:** identify what is reused—an executable program, a retrieved routine, a demonstration, a prompt, or a distilled rule—and whether the target task requires additional search or feedback. These choices affect both transfer and cost.

## LM program optimization and interfaces

| Resource | Paper / code / docs | Why read it? |
| :--- | :--- | :--- |
| **DSPy** | [Paper](https://arxiv.org/abs/2310.03714) · [Code](https://github.com/stanfordnlp/dspy) · [Docs](https://dspy.ai/) | Structured signatures, composable LM modules, and optimization against task metrics. Useful for studying program interfaces and optimization within an LM pipeline. |
| **TextGrad** | [Paper](https://arxiv.org/abs/2406.07496) · [Code](https://github.com/zou-group/textgrad) | Uses textual feedback as an optimization signal through computation graphs. Useful for comparing instruction or component refinement with workflow structure synthesis. |

When comparing these approaches, specify **what is optimized**: topology, operator selection, instructions, demonstrations, or model parameters. Holding some of these fixed can make an ablation much easier to interpret.

## Benchmarks and evaluation tools

| Resource | Official resource | Research use |
| :--- | :--- | :--- |
| **GSM8K** | [Dataset repository](https://github.com/openai/grade-school-math) | Arithmetic word problems; a source-pool benchmark in SWIFT. Useful for studying reasoning, verification, and final-answer extraction. |
| **MATH** | [Dataset repository](https://github.com/hendrycks/math) | Competition-style mathematics; a source-pool benchmark and SWIFT's main efficiency case study. |
| **EvalPlus** | [Evaluation toolkit](https://github.com/evalplus/evalplus) | Expanded tests for HumanEval and MBPP. Useful for evaluating functional correctness beyond the original tests; EvalPlus results are not interchangeable with the paper's reported benchmark protocol. |
| **BigCodeBench** | [Benchmark and evaluation code](https://github.com/bigcode-project/bigcodebench) | Code generation involving library use and more complex execution requirements. A held-out benchmark in SWIFT; record dependencies and execution failures when interpreting results. |
| **WebArena** | [Environment repository](https://github.com/web-arena-x/webarena) | Interactive web tasks for exploring workflow reuse beyond static inputs. An extension direction, not an established SWIFT result. |

Use the upstream setup instructions and record the exact dataset version, split, evaluator, and environment. The resources above are not a substitute for SWIFT's forthcoming reproduction configurations.

## Designing an informative comparison

The following are suggested reporting practices for new experiments, not additional claims from the SWIFT paper.

| Research question | What to hold fixed or report |
| :--- | :--- |
| Does structure transfer to a new task? | Source-task membership; target exclusions from trajectories, demonstrations, heuristics, and error logs; task-family overlap. |
| Does synthesis outperform simple reuse? | Compare direct reuse or retrieval of a source workflow, synthesis without demonstrations, and demonstration-conditioned synthesis under matched execution settings. |
| Which input makes synthesis work? | Ablate demonstrations, heuristics, and output contracts; report accuracy, valid-program rate, and parsing/interface failures. |
| Is the method cheaper? | Separate source-bank construction, distillation, target synthesis/search, and execution costs. Report tokens, model prices and dates, retries, caching, and wall time. |
| Does the workflow transfer across models? | Hold workflow code and operator instructions fixed while changing the execution model; report generator and executor separately. |
| Are improvements reliable? | Repeat synthesis and evaluation where feasible; report variation, sample counts, model versions, and evaluator details. |
| Why does execution fail? | Distinguish incorrect reasoning, incompatible outputs, invalid generated code, missing dependencies, and environment failures. |

## Contributing a resource

Open an [issue](https://github.com/Kingsford-Group/swift/issues) or submit a pull request with:

- The paper title and a stable primary-source link.
- The official code or documentation link, if available.
- One sentence explaining its relevance to workflow synthesis, search, reuse, or evaluation.
- The appropriate category and the date you checked the links.

Prefer focused, actionable entries. Label unavailable code and distinguish proposed applications from published evaluations. When updating an existing link, preserve the paper reference so readers can still identify the original work.
