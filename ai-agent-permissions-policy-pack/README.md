# AI Agent IAM & Permissions Policy Pack

Example policies for managing AI agent identity, tool permissions, and access boundaries.
A starter pack for teams deploying AI agents — adapt it to your environment, risk appetite, and compliance obligations.

## What's in here

| File | What it is |
|---|---|
| `agent-iam-permissions-policy.md` | The core policy: identity, tool permissioning, data boundaries, human oversight, logging, lifecycle, incident response |
| `examples/tool-permission-matrix.yaml` | A worked example: tool permissions for a customer-support agent, in a machine-readable format |
| `examples/agent-risk-tiering.md` | Risk tier definitions (Tier 1–3) with example agents in each tier |

## How to use it

1. **Start with the risk tiers.** Classify each agent you're deploying as Tier 1, 2, or 3.
2. **Apply the policy sections** to each tier — higher tiers get more controls, not fewer.
3. **Fill in the permission matrix** for your own agents using the YAML example as a template.
4. **Get sign-off** from security and the business owner before a Tier 2 or 3 agent goes live.

## Who it's for

Security teams, platform teams, and anyone being asked "so how do we control what the AI agent can do?"
It maps to concepts you'll recognize from NIST AI RMF (Govern/Map/Manage functions) and the OWASP
guidance on agentic AI threats (excessive agency, tool misuse, privilege escalation).

## A note on what this is

This is a practitioner starter pack, not legal advice and not a compliance certification.
Treat it as a baseline to adapt — your regulators, contracts, and data types set the real requirements.

---

*Created by [Andrea Harston](https://www.linkedin.com/in/aharston/) — AI security consulting through
[Don't Look Security](https://www.dontlooksecurity.com/).*
