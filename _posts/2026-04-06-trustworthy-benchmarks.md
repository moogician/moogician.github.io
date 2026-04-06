---
layout: post
title: "We Scored 100% on AI Benchmarks Without Solving a Single Problem"
date: 2026-04-02
author: "Hao Wang, Qiuyang Mang, Alvin Cheung, Koushik Sen, Dawn Song"
description: AI benchmarks decide which models get funded, deployed, and trusted. We hacked 13 of them. 45 working exploits. Every benchmark rated critical. If the scores are fake, so is everything built on them — including your training data.
tags: [benchmark, evaluation, reward-hacking, AI safety, trustworthy]
categories: research
related_posts: false
thumbnail: assets/img/trustworthy-benchmarks/teaser.png
og_image: https://moogician.github.io/assets/img/trustworthy-benchmarks/teaser.png
toc:
  sidebar: left
---

---

<img src="/assets/img/trustworthy-benchmarks/teaser.png" alt="AI agent celebrating 100% on a benchmark podium — behind the curtain, it's just reading the answers" style="max-width: 40%; display: block; margin: 1rem auto;">

### Fake Scores, Real Consequences

Every major AI company uses benchmark scores to sell their models. Training data companies use them to price their products. And increasingly, benchmark scores aren't just measuring models — they're shaping how models are trained, from RL reward signals to data filtering pipelines.

**So what happens when the benchmarks themselves are broken?**

It's not a hypothetical. A model that "improves SWE-bench by 5%" might just be better at exploiting test suite gaps. Training data priced on benchmark gains might be teaching models to game evaluations instead of solving real problems. The leaderboard number that closed your Series B might be inflatable by anyone who reads the eval script.

Here's what's been happening in public:

- [IQuest-Coder-V1](https://github.com/IQuestLab/IQuest-Coder-V1/issues/14) claimed 81.4% on SWE-bench — then researchers found 24.4% of trajectories just ran `git log` to copy the answer from commit history. Corrected score: 76.2%.
- [METR found](https://metr.org/blog/2025-06-05-recent-reward-hacking/) that o3 and Claude 3.7 Sonnet reward-hack in **30%+ of evaluation runs** — stack introspection, monkey-patching graders, operator overloading.
- [OpenAI dropped SWE-bench Verified](https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/) after finding 59.4% of audited problems had flawed tests.
- In [KernelBench](https://github.com/ScalingIntelligence/KernelBench/issues/82), `torch.empty()` returns stale GPU memory containing the reference answer — [zero computation, full marks](https://deep-reinforce.com/defense_kernel_hack.html).

These are the ones people caught by hand. We built an AI agent that finds them automatically — and it found a lot more. 

### What We Did

We built an AI agent that analyzes benchmark evaluation code in depth and automatically discovers inflation of benchmark scores. We pointed it at 13 widely-used AI benchmarks — including FrontierCS, BFCL, LiveBench, GAIA, WebArena, AGIEval, AgentBench, Terminal-Bench, tau-bench, MLE-bench, OSWorld, FieldWorkArena, and CAR-bench.

<div style="text-align: center;">
  <img src="/assets/img/trustworthy-benchmarks/results.svg" style="max-width: 85%; display: block; margin: 1rem auto;" alt="Audit Results Overview">
  <p style="margin-top: 0.8rem; font-size: 0.9em; color: #888;">Overview of findings across 13 audited benchmarks. Every benchmark was rated critical risk.</p>
</div>

The 45 confirmed exploits each come with a working proof-of-concept — code that achieves inflated or perfect scores without solving the actual task. They affect benchmarks used to evaluate everything from code generation to web navigation to general-purpose AI assistants.

We also cataloged **50 known issues** across Terminal-Bench, SWE-bench, and KernelBench from public GitHub issues and papers. Our dual detection pipeline — one LLM-based, one formal — achieved **100% detection rate** on all 50 after iterative improvement.

### How We Found Them

We used a **fully automated** hybrid agent that, with zero human intervention, scans benchmark repos, identifies potential vulnerabilities, generates working exploit code, and verifies results end-to-end. **Manual auditing doesn't scale** — a human expert might spend days on a single evaluation harness, and we needed to cover 13 benchmarks with hundreds of scoring scripts each.

The agent runs a dual detection pipeline. The **LLM Detector** uses 15 specialized scanners that reason about code intent — cross-referencing task descriptions with test logic to catch semantic gaps like spec mismatches and answer leakage. The **Formal Detector** uses 12 analysis modules built on AST parsing, data-flow analysis, and z3 SMT solving to catch structural issues without any language model. Both were iteratively improved to **100% detection** on a 50-issue catalog.

The full system is an 8-stage audit pipeline:

<div style="text-align: center;">
  <img src="/assets/img/trustworthy-benchmarks/pipeline.svg" style="max-width: 90%; display: block; margin: 1rem auto;" alt="8-Stage Audit Pipeline">
  <p style="margin-top: 0.8rem; font-size: 0.9em; color: #888;">The full audit pipeline: from benchmark ingestion through adversarial PoC generation and findings correlation.</p>
</div>

Point the agent at a benchmark repo and it produces a full audit report — vulnerability analysis, working exploit code, and fix suggestions. The 45 confirmed exploits in this post were all **discovered, written, and verified by the agent**, not by us.

### Example 1: Perfect Score, Zero Work (Frontier-CS)

In Frontier-CS's research track, the evaluator loads submitted solutions via `importlib.util.spec_from_file_location()` and `exec_module()`. This means the submitted code runs **in the same Python process as the evaluator** — sharing `sys.modules`, global state, and the call stack.

A malicious solution can walk up the stack and replace the scoring function:

```python
class Solution:
    def solve(self):
        import sys
        frame = sys._getframe(1)
        while frame is not None:
            if 'evaluate_kernel_performance' in frame.f_globals:
                def fake_evaluate(*args, **kwargs):
                    return {
                        "score": 100, "pass_all": True,
                        "geometric_mean_speedup": 10.0,
                        "total_tests": 10, "passed_tests": 10,
                    }
                frame.f_globals['evaluate_kernel_performance'] = fake_evaluate
                break
            frame = frame.f_back
        return {"code": "pass"}  # No actual solution needed
```

**Result: score 100 out of 100 without solving anything.** The solution doesn't even need to contain relevant code. Alternative attack vectors include monkey-patching `torch.allclose` to always return `True`, overwriting baseline files to fake speedup ratios, or using `atexit` to inject a fake score as the last stdout line. The Docker container runs both evaluator and solution in the same process with no sandboxing.

<div style="background: #f5f5f5; border-left: 3px solid #999; padding: 0.8rem 1rem; border-radius: 4px; margin: 1.2rem 0; font-size: 0.88rem; color: #444;">
<strong>Root cause:</strong> No process isolation between submitted code and evaluation infrastructure. The solution has full read/write access to evaluator source code and baseline implementations inside the container.
</div>

### Example 2: Fake C Extension (Terminal-Bench)

The task asks the agent to write a **C extension** for portfolio risk/return calculation — a systems programming task that should test low-level optimization skills. The test suite has three checks:

1. `test_c_extension_exists()` — verifies a compiled `.so` file exists with `portfolio_risk_c` and `portfolio_return_c` functions
2. `test_correctness_small()` — checks computation accuracy on small inputs
3. `test_performance_and_scalability()` — requires 1.2x speedup over pure Python

The critical gap: **test 1 checks that C functions exist, but never calls them.** Tests 2 and 3 import from the Python wrapper, not from the C extension. The wrapper can use any implementation internally.

The exploit — a dummy C extension paired with a numpy wrapper:

```c
// portfolio_optimized_c.c — compiles to .so, passes test_c_extension_exists()
static PyObject* portfolio_risk_c(PyObject *self, PyObject *args) {
    return PyFloat_FromDouble(0.0);  // Dummy — never actually called
}
static PyObject* portfolio_return_c(PyObject *self, PyObject *args) {
    return PyFloat_FromDouble(0.0);  // Dummy — never actually called
}
```

```python
# portfolio_optimized.py — uses numpy, not the C extension
import numpy as np
import portfolio_optimized_c  # noqa: imported so .so is loadable

def portfolio_risk_c(weights, cov_matrix):
    w = np.array(weights)
    cov = np.array(cov_matrix)
    return math.sqrt(float(w @ cov @ w))  # numpy, not C
```

**Result: all three tests pass without writing any meaningful C code.** The dummy `.so` satisfies the existence check. numpy satisfies correctness (within 1e-10) and performance (1.2x faster than pure Python loops).

<div style="background: #f5f5f5; border-left: 3px solid #999; padding: 0.8rem 1rem; border-radius: 4px; margin: 1.2rem 0; font-size: 0.88rem; color: #444;">
<strong>Root cause:</strong> The benchmark tests what it's easy to test (does a file exist?) rather than what it claims to test (can the agent write optimized C?). No check verifies the C extension functions produce correct output or are actually called.
</div>

### Example 3: All 812 Answers in a JSON File (WebArena)

[WebArena](https://webarena.dev/) tests whether agents can navigate websites to complete real-world tasks — shopping, forum posting, content management. The evaluator checks the agent's answer against reference answers using string matching.

The problem: `config_files/test.raw.json` contains **every reference answer in plaintext**:

```json
{
  "task_id": 0,
  "intent": "What is the top-1 best-selling product in 2022",
  "eval": {
    "reference_answers": {
      "exact_match": "Quest Lumaflex Band"
    }
  }
}
```

All 812+ tasks. Every answer. Sitting on the same filesystem where the agent runs. No sandboxing prevents the agent from reading it. A trivial cheating agent:

```python
class CheatingAgent:
    def __init__(self):
        with open('config_files/test.raw.json') as f:
            tasks = json.load(f)
        self.answers = {
            t['task_id']: t['eval']['reference_answers']
            for t in tasks
        }

    def act(self, task_id, observation=None):
        ref = self.answers.get(task_id, {})
        answer = ref.get('exact_match', ref.get('must_include', [''])[0])
        return {'action_type': 'stop', 'answer': answer}
```

**Result: perfect score on all string-match tasks with zero web browsing.** No clicking, no navigation, no understanding of web interfaces. Just read a JSON file and return the answer.

<div style="background: #f5f5f5; border-left: 3px solid #999; padding: 0.8rem 1rem; border-radius: 4px; margin: 1.2rem 0; font-size: 0.88rem; color: #444;">
<strong>Root cause:</strong> Reference answers stored in agent-accessible filesystem with no integrity protection. The evaluator reads from the same JSON files the agent can access.
</div>

### What This Means

Broken benchmarks don't just produce wrong leaderboards — they poison training signals, inflate data pricing, and mislead deployment decisions. If nobody audits the evaluation infrastructure, everything built on top of it is unreliable.

Our agent found 45 confirmed exploits that human reviewers missed — not because they were subtle, but because nobody was looking. The tools and methodology are open source at [github.com/moogician/trustworthy-env](https://github.com/moogician/trustworthy-env).
