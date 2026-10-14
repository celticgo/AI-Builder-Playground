# AI Agent IAM & Permissions Policy

**Version:** 1.0 — example baseline. Adapt before adopting.
**Applies to:** any AI agent (LLM-powered system that takes actions through tools, APIs, or other systems)
deployed or piloted by the organization.

---

## 1. Purpose & Scope

AI agents act. Unlike a chatbot that only produces text, an agent can read files, query databases,
send messages, move money, and change configurations. Every one of those actions needs an identity,
a permission, and a record — the same as any human or service account, plus agent-specific controls.

This policy defines how agent identities are created, what agents are allowed to do, when a human
must approve, and how agent activity is monitored and reviewed.

**Out of scope:** model training, model evaluation, and pure text-generation use cases with no tool access.
Those follow the organization's general AI use policy.

## 2. Guiding Principles

1. **Deny by default.** An agent can do nothing until explicitly granted. Allowlists, not denylists.
2. **Least privilege, per tool.** Each tool grant is scoped to the minimum actions and data the agent needs.
3. **One identity per agent.** No shared agent credentials. If two agents need the same access, each gets its own identity.
4. **Humans approve irreversible actions.** Anything that can't be undone (payments, external sends, deletions, production changes) needs a human gate.
5. **Every action is attributable.** Logs tie each action to the agent identity, the invoking user or trigger, and the policy version in effect.
6. **Permissions expire.** Grants are time-bound and re-reviewed. Standing broad access is not permitted for Tier 2/3 agents.

## 3. Agent Identity

- Every agent gets a **unique, non-human identity** in the identity provider (e.g., `agent-support-triage-01`), never a shared or personal account.
- The identity record must state: owner (human accountable), purpose, risk tier (see `examples/agent-risk-tiering.md`), environment, and data classification it may touch.
- Agents authenticate to tools with **short-lived credentials** (scoped tokens, workload identity) — never long-lived API keys baked into prompts, code, or config files.
- Agent identities are **non-interactive**: no console login, no password-based auth.
- When an agent acts on behalf of a user (delegation), the action is logged under **both** identities: the agent and the delegating user.

## 4. Tool Permissioning

Tools are the agent's hands. Control them like production access.

- Maintain a **tool permission matrix** per agent: tool → allowed actions → data scope → approval requirement.
  See `examples/tool-permission-matrix.yaml` for the format.
- **Allowlist model:** the agent's tool manifest lists exactly what it may call. Anything not listed is denied at the enforcement layer, not just in the prompt.
- **Scope tools narrowly.** Prefer `tickets:read:queue-billing` over `tickets:read`. If the tool doesn't support fine scopes, put a policy proxy in front of it.
- **Separate read from write.** Read-only tools and state-changing tools are granted independently. A research agent doesn't get write tools "just in case."
- **No privilege inheritance by chaining.** If Agent A can call Agent B, B's permissions do not expand A's. Each hop is authorized independently.
- **Risky tool categories** (code execution, shell access, email/send-as, payment, identity/provisioning, production deploy) require Tier 3 controls: human approval per action or per session, plus enhanced logging.

## 5. Data Access Boundaries

- An agent's data access must match the **data classification** it's approved for (public / internal / confidential / restricted). The matrix records this per tool.
- Agents do not get broad warehouse or file-share access to "find what they need." Scope to specific datasets, tables, or paths.
- **PII/PHI/financial data:** any agent touching regulated data needs documented legal basis, data-handling review, and Tier 2+ controls at minimum.
- Outputs inherit the classification of the most sensitive input. An agent summarizing restricted data produces restricted output — enforce handling accordingly.
- Cross-boundary flows (e.g., internal data → external SaaS tool → public output) are prohibited unless explicitly approved and logged.

## 6. Human Oversight & Approval Gates

Define the agent's **autonomy level** at deployment and record it in the identity record:

| Level | Meaning | Example |
|---|---|---|
| L0 | Human approves every action | Agent drafts, human clicks send |
| L1 | Agent acts on low-risk actions; human approves the rest | Reads data freely; writes need approval |
| L2 | Agent acts within guardrails; human reviews asynchronously | Batch jobs with next-day review |
| L3 | Agent acts autonomously within scope; human on exception | Only with strong detective controls |

- **Tier 3 agents may not operate above L1** without a documented exception signed by security leadership.
- Approval gates are enforced in the tool layer (the API requires an approval token), not requested in the prompt.
- The approver must see: what the agent wants to do, on what data, and why (the agent's stated rationale) — enough to make an informed decision.

## 7. Authentication & Secrets Management

- Agent credentials live in a secrets manager or workload-identity system. Nothing in prompts, repos, tickets, or chat logs.
- Rotate agent credentials on schedule and on personnel/role change of the agent owner.
- Agents must not be able to read their own credentials or request broader scopes than granted (no self-elevation).
- Third-party agent platforms: verify how the platform stores credentials and whether its data handling meets your requirements before connecting internal tools.

## 8. Logging, Monitoring & Audit

Log every tool call with, at minimum:

- timestamp, agent identity, invoking user/trigger
- tool name, action, parameters (redact secrets/PII)
- approval reference (if gated), policy version
- result (success/failure) and data classification touched

Requirements:

- Logs are append-only and retained per the organization's retention schedule (minimum 1 year for Tier 2/3).
- Alert on: permission denials (possible prompt-injection probing), first-time tool use, after-hours activity spikes, data-volume anomalies, and approval-bypass attempts.
- The agent owner reviews Tier 2/3 agent activity **monthly**; security reviews **quarterly**.
- Keep a prompt/behavior version history so an incident can be traced to the exact agent version.

## 9. Lifecycle Management

- **Provisioning:** new agents need owner, purpose, tier, and tool matrix before deployment. No tier, no launch.
- **Change control:** new tools, broader scopes, or tier changes go through the same approval as initial provisioning.
- **Periodic review:** re-certify every agent's permissions at least every 6 months (quarterly for Tier 3). Remove what isn't used.
- **Deprovisioning:** disable the identity, revoke credentials, and archive logs when the agent is retired. Orphaned agent identities are a finding.

## 10. Incident Response

- Every Tier 2/3 agent has a **kill switch**: a single control that revokes its credentials and halts its tool access immediately.
- Suspected agent misbehavior (unexpected actions, data exfiltration, privilege-escalation attempts) follows the standard incident response process, with the agent owner and security engaged.
- Preserve agent logs, prompts, and version history before remediation — they're evidence.
- Post-incident: determine whether the cause was prompt injection, tool misconfiguration, excessive permissions, or model behavior, and fix the class, not just the instance.

## 11. Roles & Responsibilities

| Role | Owns |
|---|---|
| Agent owner (business) | Purpose, tiering, approving what the agent should do, monthly reviews |
| Security | Policy, tier criteria, approval of Tier 2/3 deployments, quarterly reviews, incident response |
| Platform/engineering | Enforcement (identity, scopes, gates, logging plumbing), kill switch |
| Data owner | Approving data access for their datasets |
| Internal audit / GRC | Periodic attestation that the policy is followed |

## 12. Exceptions

Exceptions (e.g., a Tier 3 agent needing L2 autonomy) require: business justification, compensating controls,
an expiry date (max 90 days), and sign-off from security leadership. All exceptions are logged and reviewed at expiry.
