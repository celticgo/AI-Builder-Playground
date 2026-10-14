# Benchmark Results Template

Copy this file per target. One file per benchmark run.

---

## Target

- **Application:**
- **Version/build:**
- **URL / access method:**
- **Model(s):**
- **Test date:**
- **Tester:**
- **Scope notes:** (any categories skipped, with justification)

## Baseline behavior

(3–5 benign prompts and observed normal behavior)

## Results

| Category | Rating (Pass/Partial/Fail) | Notes |
|---|---|---|
| LLM01 Prompt Injection | | |
| LLM02 Sensitive Information Disclosure | | |
| LLM03 Supply Chain | | |
| LLM04 Data and Model Poisoning | | |
| LLM05 Improper Output Handling | | |
| LLM06 Excessive Agency | | |
| LLM07 System Prompt Leakage | | |
| LLM08 Vector/Embedding Weaknesses | | |
| LLM09 Misinformation | | |
| LLM10 Unbounded Consumption | | |

**Overall grade:** A / B / C / D (per methodology rubric)

## Findings

### [F-01] Title
- **Category:** LLM0X
- **Severity:** Critical / High / Medium
- **Test case:** (which test from tests.md)
- **Evidence:** (redacted prompt/response excerpt)
- **Impact:**
- **Remediation:**

*(repeat per finding)*

## Remediation re-test

| Finding | Fix applied | Re-test date | New rating |
|---|---|---|---|
| | | | |

## Disclaimer

Results describe the tested configuration on the test date. They are not a security
certification and do not guarantee the absence of other vulnerabilities.
