<div align="center">

# SWIFT: Synthesizing Workflows via Few-shot Transfer

**Can Workflow Search Be Amortized?**<br>
**Demonstration-Guided Synthesis of LLM Agent Workflows**

[![arXiv](https://img.shields.io/badge/arXiv-2604.25012-b31b1b.svg)](https://arxiv.org/abs/2604.25012)
[![Status](https://img.shields.io/badge/Code-release%20in%20preparation-blue.svg)](#getting-started)

[📄 Paper](https://arxiv.org/abs/2604.25012) · [🔎 Overview](#overview) · [🏆 Results](#results) · [🚀 Getting Started](#getting-started) · [📚 Resources](#resources) · [📌 Roadmap](#roadmap) · [📝 Citation](#citation)

**Reuse prior workflow search. Synthesize a workflow for a new task in one generation pass.**

</div>

<a id="news"></a>

## 📰 Updates & News

- 🎉 **SWIFT has been accepted to the EMNLP 2026 Main Conference!**
- **[2026-04-27]** The [SWIFT preprint](https://arxiv.org/abs/2604.25012) is available on arXiv.

<a id="tldr"></a>

## ⚡ TL;DR

- **The idea:** reuse workflow structures discovered on source tasks to synthesize a workflow for a new task in **one generation pass**.
- **The ingredients:** executable workflow demonstrations, distilled design heuristics, and output-interface contracts.
- **The result:** **85.34%** average accuracy on five main benchmarks with GPT-4o-mini, with substantially lower marginal optimization cost given an existing source bank.

> 🚧 **Code release in preparation.** The paper overview and results are available now. Implementation, workflow assets, and runnable examples will follow.

<details>
<summary><b>📑 Table of Contents</b></summary>

- [Overview](#overview)
- [How It Works](#how-it-works)
- [Getting Started](#getting-started)
- [Benchmarks & Tasks](#benchmarks-and-tasks)
- [Results](#results)
- [Scope & Limitations](#scope-and-limitations)
- [Research Resources](#resources)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [Citation](#citation)

</details>

<a id="overview"></a>

## 🔎 Overview

Designing an LLM agent workflow often means searching over many combinations of reasoning, verification, ensembling, and code execution for every new task. **SWIFT reuses what previous searches have already discovered.**

Given source-task search trajectories and workflow demonstrations, SWIFT distills reusable design heuristics and output-interface contracts. A single LLM generation call then combines these priors with a new task description to synthesize an executable workflow, without iterative target-task optimization.

<p align="center">
  <img src="assets/swift-overview.png" alt="SWIFT pipeline: source-task search trajectories are distilled into heuristics, output contracts, and workflow demonstrations; a single online LLM call uses these assets and a new task description to synthesize an executable workflow." width="100%">
</p>

*Method overview from Figure 2 of the camera-ready paper.*

**Workflow synthesis** produces the program; **workflow execution** runs that program and may use multiple operator and LLM calls. The reported cost advantage concerns optimization with an existing source bank.

<a id="how-it-works"></a>

## 🧩 How It Works

### 1. Distill reusable priors offline

Compare successful and unsuccessful source-task workflows and their execution or parsing failures to extract:

- **Compositional heuristics:** design rules for choosing and connecting operators.
- **Output contracts:** intermediate and final output-format constraints that help operators exchange compatible results.
- **Workflow demonstrations:** source-task descriptions, executable workflow code, and design rationales.

### 2. Synthesize a workflow for a new task

Build a meta-prompt containing the operator library, distilled heuristics, output contracts, source demonstrations, and the target-task description. The generator produces one executable candidate workflow in a single pass.

### 3. Execute the synthesized workflow

Run the generated program on task inputs using the execution model. Workflow synthesis and workflow execution are separate stages: the synthesized structure can coordinate reasoning, verification, ensembling, or execution-feedback repair.

**Evaluation protocol.** Source-pool tasks use task-level leave-one-out: the target task's examples, search trajectories, error logs, and optimized workflows are excluded from the synthesis inputs. Held-out benchmarks are absent from both trajectory distillation and demonstration construction.

<a id="getting-started"></a>

## 🚀 Getting Started

Start with the [paper](https://arxiv.org/abs/2604.25012) and [method overview](#how-it-works). This repository does not yet include an installable package or runnable implementation.

| Resource | Availability |
| :--- | :--- |
| Paper | [Read on arXiv](https://arxiv.org/abs/2604.25012) |
| Method overview & reported results | Available in this README |
| Synthesis & execution implementation | Planned for the code release |
| Demonstrations, heuristics & output contracts | Planned for the code release |
| Installation & minimal example | Planned alongside the implementation |
| Custom-task guide & reproduction configs | Planned after the end-to-end example |

<a id="benchmarks-and-tasks"></a>

## 🧪 Benchmarks & Tasks

The camera-ready evaluation covers math reasoning, code generation, and question answering. Inclusion in this table describes the paper's experiments, not currently available dataset loaders.

| Evaluation group | Benchmarks | Role |
| :--- | :--- | :--- |
| Source pool | GSM8K, MATH, HumanEval, MBPP | Task-level leave-one-out synthesis |
| Held-out transfer | MultiArith, SVAMP, AIME 2024–2025, AQuA, BigCodeBench | Never included in the source demonstration pool |
| Execution-backbone portability | GSM8K, MATH, HumanEval, MBPP, DROP, HotpotQA | Fixed workflows executed with Grok, Qwen, and Gemma |

<a id="results"></a>

## 🏆 Results

Results below are reported in the camera-ready paper; they have not yet been reproduced from this repository.

### Main benchmarks

Test accuracy (%) with GPT-4o-mini. Selected comparisons from **Table 1**; the paper includes the full baseline set.

| Method | GSM8K | MATH | HumanEval | MBPP | MultiArith¹ | Average |
| :--- | ---: | ---: | ---: | ---: | ---: | ---: |
| Vanilla | 87.45 | 46.29 | 87.08 | 71.83 | 96.85 | 77.90 |
| AFlow | 91.16 | 51.28 | 90.93 | 81.67 | 96.22 | 82.25 |
| MaAS | 92.30 | 51.82 | 92.85 | 82.17 | **98.80** | 83.59 |
| **SWIFT** | **94.31** | **55.14** | **95.42** | **83.28** | 98.54 | **85.34** |

¹ MultiArith is not part of the source demonstration pool. SWIFT improves over AFlow on all five main benchmarks and has the highest average accuracy among the methods compared in the paper.

<details>
<summary><b>🌍 Transfer to additional held-out benchmarks</b></summary>

Accuracy (%) from **Table 2**. None of these benchmarks is included in the source demonstration pool.

| Benchmark | Vanilla | SWIFT | Gain (percentage points) |
| :--- | ---: | ---: | ---: |
| SVAMP | 81.0 | 95.8 | +14.8 |
| AIME 2024–2025 | 8.3 | 14.6 | +6.3 |
| AQuA | 52.9 | 79.9 | +27.0 |
| BigCodeBench | 31.6 | 34.3 | +2.7 |

</details>

<details>
<summary><b>⏱️ Optimization efficiency and source-bank accounting</b></summary>

MATH with GPT-4o-mini, from **Table 3**. These columns describe workflow optimization, excluding execution on the test set.

| Method | Optimization cost (USD) | Optimization time | Accuracy (%) |
| :--- | ---: | ---: | ---: |
| AFlow | $22.50 | 184 min | 51.28 |
| MaAS | $3.38 | 53 min | 51.82 |
| **SWIFT** | **$0.004** | **0.13 min** | **55.14** |

SWIFT's reported optimization cost includes contrastive distillation over available source trajectories and single-pass synthesis. It excludes the search needed to create those source trajectories. Under the paper's cold-start accounting, the upstream source bank costs approximately **$112.50**, with a break-even point of about **five downstream tasks** relative to per-task search (Section 4.2 and Appendix G). These are experimental measurements, not current API price estimates or guarantees for arbitrary tasks.

</details>

<details>
<summary><b>🔄 Execution-model portability</b></summary>

Holding the synthesized workflows and operator instructions fixed, the paper evaluates Grok-4.1-fast, Qwen3-8B, and Gemma-3-12B-IT across six benchmarks. SWIFT improves accuracy on **16 of 18** benchmark–model pairs (**Table 5**). Gains vary by model and task; for example, Gemma's MATH accuracy decreases, illustrating that transferred workflows depend on the executor's instruction-following ability.

</details>

<a id="scope-and-limitations"></a>

## Scope & Limitations

SWIFT is most relevant when source workflows or search traces already exist, or when their construction cost can be shared across multiple related tasks. The paper primarily evaluates static reasoning and code-generation benchmarks with automatic correctness signals.

Transfer is sensitive to task-family alignment, output-interface compatibility, execution dependencies, and the capabilities of the execution model. The reported results do not establish effectiveness in general stochastic, interactive, or irreversible environments.

<a id="resources"></a>

## 📚 Research Resources

Explore the **[research resource guide](docs/resources.md)** for an annotated reading path, official implementations, benchmark tools, and suggestions for designing informative comparisons.

| Direction | Starting points |
| :--- | :--- |
| Workflow & architecture search | AFlow, ADAS, MaAS, AgentSquare |
| Workflow & experience reuse | Agent Workflow Memory, SWIFT |
| LM program optimization | DSPy, TextGrad |
| Benchmarks & evaluation | GSM8K, MATH, EvalPlus, BigCodeBench, WebArena |

Each entry explains its research relevance and links to primary sources. Contributions of relevant papers, tools, and corrected links are welcome.

<a id="roadmap"></a>

## 📌 Roadmap

- [x] Paper overview, method figure, and main results.
- [x] arXiv citation and paper link.
- [ ] Release workflow synthesis and execution code.
- [ ] Release source demonstrations, distilled heuristics, and output contracts.
- [ ] Add verified installation instructions and a minimal end-to-end example.
- [ ] Document how to adapt SWIFT to a custom task.
- [ ] Add evaluation configurations and reproduction instructions.
- [ ] Establish automated checks and document tested model configurations.

<a id="contributing"></a>

## 🤝 Contributing

We welcome feedback on the paper, documentation improvements, and suggestions for tasks where workflow reuse would be useful.

- **Questions or ideas:** [open an issue](https://github.com/Kingsford-Group/swift/issues).
- **Documentation fixes:** submit a focused pull request describing the change.
- **Future code contributions:** task adapters, workflow demonstrations, model integrations, and reproducibility improvements are useful directions once the implementation is available.

When reporting an issue with a paper result, include the relevant table or section and enough detail to understand the discrepancy. Development and testing instructions will be added with the code release.

<a id="citation"></a>

## 📝 Citation

If you use SWIFT in your research, please cite the [arXiv paper](https://arxiv.org/abs/2604.25012). The arXiv version is titled *Why Search When You Can Transfer? Amortized Agentic Workflow Design from Structural Priors*.

```bibtex
@misc{du2026swift,
  title = {Why Search When You Can Transfer? Amortized Agentic Workflow Design from Structural Priors},
  author = {Du, Shiyi and Liu, Jiayuan and Du, Weihua and Huang, Yue and Li, Jiayi and Luo, Yingtao and Zhang, Xiangliang and Conitzer, Vincent and Kingsford, Carl},
  year = {2026},
  eprint = {2604.25012},
  archivePrefix = {arXiv},
  primaryClass = {cs.LG},
  doi = {10.48550/arXiv.2604.25012},
  url = {https://arxiv.org/abs/2604.25012}
}
```
