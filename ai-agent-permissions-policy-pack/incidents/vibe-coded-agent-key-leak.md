# Incident Case Study: Vibe-coded agent leaks API key, burns ~$600k in tokens (2026)

*Added 2026-10-15 as a worked example of the policy controls in practice.*

## What happened (per reporting)

Per SANS reporting on a METR case study, a vibe-coded agentic application leaked
an API key and burned roughly $600,000 in tokens before anyone noticed. No
attacker required — the agent did it to itself through normal operation:
credentials exposed where the agent could reach them, no spending controls,
and no monitoring to catch the burn.

This is the "boring" incident in the set, and arguably the most likely one for
any team deploying agents today. Not a sandbox escape or a police tip — just
money and secrets evaporating through missing basics.

## Why it maps to this policy pack

- **Secrets management.** Agent credentials live in a secrets manager or
  workload-identity system — never in code, prompts, repos, or chat logs.
  An agent that can read its own credentials (or anyone else's) is a finding.
- **Unbounded consumption controls.** The benchmark project's LLM10 category
  covers this: per-identity metering, spending caps, and alerts on anomalous
  consumption. $600k should have paged someone at $6k.
- **Least-privilege tool scoping.** The agent had access to credentials it didn't
  need for its task. Scope tools to the minimum actions and data required.
- **Owner review cadence.** Monthly owner reviews of Tier 2/3 agent activity
  exist precisely to catch slow-burn failures like this before they compound.

## The pattern

Not every agent incident is dramatic. The unglamorous controls — secrets hygiene,
spending caps, scoped permissions, regular reviews — prevent the incidents most
teams are actually going to hit first.

## Source (as reported)

- SANS blog, "Zero Trust for AI Agents Starts With Fixing Zero Visibility":
  https://www.sans.org/blog/zero-trust-ai-agents-starts-fixing-zero-visibility
