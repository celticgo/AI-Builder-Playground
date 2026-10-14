# Agent Risk Tiering — Examples

Classify every agent before deployment. The tier sets which controls apply.
When in doubt, tier up.

## Tier 1 — Low risk

**Profile:** read-only or narrowly scoped; no access to sensitive data; no irreversible actions;
no external communication.

Examples:
- Internal docs Q&A agent (reads public/internal knowledge base, no tools beyond search)
- Code-review assistant (reads PRs, posts comments as suggestions only)
- Meeting-notes summarizer (reads transcripts, writes to a designated folder)

**Controls:** unique identity, tool allowlist, standard logging, 6-month recertification.

## Tier 2 — Medium risk

**Profile:** touches internal or confidential data; takes reversible state-changing actions;
drafts external communications; operates with human approval gates.

Examples:
- Customer-support triage agent (reads tickets, drafts replies — human sends)
- Data-analysis agent (queries scoped datasets, produces internal reports)
- IT helpdesk agent (password resets via approved workflow, with approval)

**Controls:** everything in Tier 1, plus: human approval gates for writes/sends,
enhanced logging (1-year retention), monthly owner review, no standing broad access,
documented data-handling review for any regulated data.

## Tier 3 — High risk

**Profile:** touches restricted/regulated data (PII, PHI, financial); can take irreversible
or externally visible actions; has risky tools (code execution, payments, provisioning,
production deploys); or operates with reduced human oversight.

Examples:
- Finance agent that drafts and queues payments
- DevOps agent with production deploy tooling
- Agent with shell/code-execution access to production systems
- Customer-facing agent that sends messages without per-message approval

**Controls:** everything in Tier 2, plus: max autonomy L1 (human approves each consequential
action) unless a 90-day exception is signed; per-session or per-action approval for risky tools;
kill switch tested quarterly; quarterly security review; anomaly alerting tuned and monitored.

## Quick tiering questions

1. Can it change state outside its own sandbox? (No → likely Tier 1)
2. Can it touch confidential/restricted data? (Yes → at least Tier 2)
3. Can it do something irreversible — pay, send, delete, deploy? (Yes → Tier 3)
4. Does it run without a human in the loop? (Yes → tier up one level)
