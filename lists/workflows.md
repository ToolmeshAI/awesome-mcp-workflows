# Workflow Patterns

These are pattern-level workflows designed to be useful for builders. They are not claims about external adoption unless a future contributor adds evidence and changes the label.

## Engineering and Release

### Release notes from merged work

- Status: `Pattern seed`
- Primary user: engineering lead or developer experience owner
- Problem: merged work is visible in tooling, but release messaging is still manual and inconsistent
- Inputs: merged pull requests, linked issues, labels, milestones, release dates
- Outputs: draft release notes, grouped changelog bullets, missing-context warnings
- Server mix: repository server + communication or docs server
- Human checkpoints: approve wording and remove internal-only details before publishing
- Why it matters: repetitive and easy to standardize, but still benefits from human taste

### Changelog assembly from labels and milestones

- Status: `Pattern seed`
- Primary user: open-source maintainer or internal platform team
- Problem: changes exist across issues and milestones, but the public changelog format is hard to keep consistent
- Inputs: labels, milestones, issue state, merged work, optional docs references
- Outputs: versioned changelog draft, uncategorized item list, release gaps
- Server mix: repository server + docs index
- Human checkpoints: confirm categorization and any breaking-change language
- Why it matters: converts a maintenance tax into a repeatable publishing workflow

### Pre-ship docs freshness check

- Status: `Pattern seed`
- Primary user: developer advocate, prompt designer, or docs owner
- Problem: tools and prompts are updated faster than supporting docs
- Inputs: docs corpus, release notes, changed files, prompt specs, feature flags
- Outputs: stale-page warnings, missing-doc checklist, suggested update targets
- Server mix: docs index + repository server + local workspace tools
- Human checkpoints: approve any source-of-truth changes
- Why it matters: catches a common failure mode before users do

## Knowledge and Enablement

### Internal onboarding assistant

- Status: `Pattern seed`
- Primary user: new team member or hiring manager
- Problem: onboarding information is scattered across docs, checklists, and meeting notes
- Inputs: role, team, onboarding stage, internal docs, task systems, calendar context
- Outputs: prioritized onboarding plan, briefing pack, missing-access checklist
- Server mix: docs index + calendar server + task or repository server
- Human checkpoints: manager confirms role-specific expectations
- Why it matters: useful from day one and measurable through time-to-first-task

### Meeting brief and follow-up generator

- Status: `Pattern seed`
- Primary user: manager, founder, or cross-functional lead
- Problem: context gathering and follow-up creation are repetitive and fragmented
- Inputs: meeting metadata, linked docs, task lists, recent updates
- Outputs: pre-read brief, discussion prompts, action items, follow-up draft
- Server mix: calendar server + docs index + task or communication server
- Human checkpoints: owner validates action items before sending
- Why it matters: low-friction workflow with clear time savings

## Customer and Revenue Operations

### Support ticket enrichment

- Status: `Pattern seed`
- Primary user: support agent or technical account manager
- Problem: support tickets arrive without enough product or customer context
- Inputs: ticket text, account metadata, internal docs, incident history, known issues
- Outputs: richer case summary, suggested next steps, escalation packet, safe reply draft
- Server mix: ticketing bridge + docs index + monitoring or status context
- Human checkpoints: human sends the final customer response
- Why it matters: strong fit for agent assistance because the job is context-heavy but still reviewable

### Customer onboarding QA run

- Status: `Pattern seed`
- Primary user: implementation manager or customer success team
- Problem: onboarding steps span several systems and regress easily
- Inputs: account status, checklist state, portal access, product configuration
- Outputs: completion report, missing-step alerts, evidence screenshots, follow-up tasks
- Server mix: browser bridge + CRM or ticketing bridge + communication server
- Human checkpoints: humans resolve exceptions and approve customer-facing updates
- Why it matters: combines structured checks with high visibility for teams and customers

### Account review prep

- Status: `Pattern seed`
- Primary user: customer success manager or account executive
- Problem: preparing for renewals or executive reviews requires stitching together multiple systems
- Inputs: CRM records, support history, product usage summaries, meeting notes
- Outputs: account brief, risk flags, upsell or renewal talking points, open actions
- Server mix: CRM bridge + docs index + BI server + communication tools
- Human checkpoints: owner removes speculation and validates sensitive details
- Why it matters: compresses prep work without requiring fully autonomous decision-making

## Operations and Governance

### Browser-assisted QA walk

- Status: `Pattern seed`
- Primary user: QA lead, ops analyst, or product manager
- Problem: critical admin flows still require manual, repetitive checks in web UIs
- Inputs: test steps, authenticated browser session, expected UI states
- Outputs: pass-fail checklist, screenshots, issue notes, rerun-ready scripts
- Server mix: browser bridge + communication or repository server
- Human checkpoints: human decides whether results block launch
- Why it matters: useful even when no proper end-to-end automation exists yet

### Incident update assistant

- Status: `Pattern seed`
- Primary user: incident commander or on-call engineer
- Problem: responders need to coordinate facts, status updates, and stakeholder communication under time pressure
- Inputs: active alerts, timeline events, dashboard context, previous updates
- Outputs: incident brief, stakeholder update draft, responder checklist, postmortem skeleton
- Server mix: monitoring server + communication server + docs index
- Human checkpoints: incident commander approves every external or broad internal message
- Why it matters: improves clarity during stressful workflows without pretending the agent should own the incident

### Policy or prompt review gate

- Status: `Pattern seed`
- Primary user: platform owner, compliance lead, or AI safety reviewer
- Problem: prompts, policies, and workflow configs change faster than review habits
- Inputs: proposed prompt or policy diff, existing guidelines, risk checklist, affected systems
- Outputs: review summary, missing-control warnings, approval checklist, escalation notes
- Server mix: local workspace tools + docs index + communication or approval server
- Human checkpoints: explicit approval before rollout
- Why it matters: creates a repeatable governance layer for fast-moving teams

### Payment-owner handoff before signing

- Type: `workflow pattern`
- Status: `Documented example`
- Summary: an agent prepares a bounded payment action, then hands off the exact foreground terminal command when the wallet owner must approve/sign it
- Primary user: developer building an agent that consumes paid APIs
- Problem solved: preparing a transaction and authorizing its signature must remain distinct steps
- Trigger: an owner-requested direct transfer or EVM payment reaches a foreground-approval boundary
- Inputs: owner intent, explicit asset/chain/recipient or resource, amount/fee bounds, prepared operation reference; no private keys in model context
- Outputs: foreground command for owner review, plus operation/receipt state after a separately authorized action
- Server mix: a local payment MCP runtime such as [Agent Payment Node](https://github.com/nuanu-ai/agent-payment-node), using its documented `apn_foreground_handoff` tool; optional application tools consume only the required result fields
- Trust boundary: high; a local payment runtime can affect funds and use secret-backed signing. Review untrusted offers and returned commands. A generated command is not authorization and must not be executed automatically by the agent
- Human checkpoint: the owner checks recipient/resource, asset, chain, amount and fees, and decides whether to run the command in the foreground terminal; stop on ambiguity or changed terms
- Success signal: the recorded operation state and receipt match the separately approved intent; a prepared operation or successful tool call alone does not establish payment completion
- Follow-up artifact: a redacted operation reference and outcome in the calling application's approved record; keep keys and unnecessary wallet data out of shared logs
- Why it matters: makes the approval handoff visible to both the developer and owner, without granting the model general signing authority
- Evidence note: APN's public README documents the foreground handoff and owner controls. This example covers direct transfers/EVM actions; eligible Solana x402 execution has a distinct owner-admitted policy. APN currently requires Apple Silicon macOS/Node.js 24. No real payment, production adoption, client-specific integration or end-to-end workflow result is asserted here
