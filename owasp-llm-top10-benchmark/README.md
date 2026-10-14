# OWASP LLM Top 10 Benchmark

A repeatable black-box testing methodology and harness for evaluating LLM-powered
applications against the [OWASP Top 10 for LLM Applications](https://genai.owasp.org/) (2025 edition).

## What's in here

| File | What it is |
|---|---|
| `methodology.md` | How the benchmark works: scope, setup, scoring rubric, reporting, and rules of engagement |
| `tests.md` | Concrete test cases for all 10 categories — what to send, what to look for, pass/fail criteria |
| `results-template.md` | Template for publishing benchmark results |
| `benchmark_runner.py` | Minimal Python runner: sends test prompts to an OpenAI-compatible endpoint and records responses for review |

## How to use it

1. Read `methodology.md` — especially the rules of engagement. **Only test systems you own
   or are explicitly authorized to test.**
2. Define your target in the results template (app name, version, date, model).
3. Run the test cases in `tests.md` manually, or use `benchmark_runner.py` for the prompt-based ones.
4. Score each category, write up findings, publish with the template.

## Scoring at a glance

Each category gets one of: **Pass** (controls hold) / **Partial** (mitigations exist but bypassable) / **Fail** (vulnerability confirmed).
Findings are weighted by severity — see `methodology.md` for the full rubric.

## Who it's for

Security testers, AI red teams, and product teams who want a structured way to answer
"how exposed is our LLM app, really?" — with evidence, not vibes.

---

*Part of [Andrea Harston's](https://www.linkedin.com/in/aharston/) AI security project portfolio.
Created through [Don't Look Security](https://www.dontlooksecurity.com/).*
