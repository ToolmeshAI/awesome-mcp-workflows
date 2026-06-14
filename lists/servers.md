# Server Patterns

This list focuses on reusable MCP server shapes rather than pretending every entry is a fully verified product.

## Reading Guide

- `Pattern seed` means the entry describes a credible reusable shape.
- Trust levels are about blast radius if the server is misused or poorly scoped.
- Use the labels from [../docs/taxonomy.md](../docs/taxonomy.md) when adding entries.

## Knowledge and Context

### Internal docs and knowledge index

- Status: `Pattern seed`
- Primary job: expose internal docs, runbooks, policies, and notes to an agent
- Typical inputs: search queries, entity names, product areas, document identifiers
- Typical outputs: grounded answers, citations, extracted snippets, freshness warnings
- Trust level: `Medium`
- Why it matters: one of the fastest ways to make an agent useful without giving it write access
- Good matches: onboarding copilot, support enrichment, policy review, docs freshness checks
- Review questions: where does freshness come from, and how do you prevent stale or conflicting sources from looking authoritative?

### Data warehouse or BI query server

- Status: `Pattern seed`
- Primary job: let an agent retrieve metrics or answer scoped analytical questions
- Typical inputs: metric names, date windows, dimensions, approved query templates
- Typical outputs: aggregates, trend summaries, query artifacts, charts or chart-ready data
- Trust level: `Medium`
- Why it matters: makes weekly reporting, launch checks, and operations reviews much faster
- Good matches: KPI briefings, incident analysis, revenue reviews, launch retrospectives
- Review questions: can users tell the difference between raw data, derived metrics, and natural-language interpretation?

## SaaS and System Connectors

### Single-SaaS connector

- Status: `Pattern seed`
- Primary job: connect one authenticated SaaS surface to agent workflows
- Typical inputs: object ids, search queries, filters, approved write intents
- Typical outputs: search results, record updates, comments, tasks, tickets, documents
- Trust level: `Medium`
- Why it matters: easiest way to build focused product value around one team's existing system of record
- Good matches: project hygiene, CRM updates, support workflows, content operations
- Review questions: are write actions scoped tightly enough to avoid turning a convenience layer into a footgun?

### Social data and publishing workflow server

- Status: `Documented example`
- Primary job: expose authenticated X data, monitoring, webhooks, extraction jobs, compose flows, and approved write actions to agent workflows
- Typical inputs: search queries, account names, tweet ids, monitor definitions, webhook targets, compose briefs, approved publish intents
- Typical outputs: X data results, extraction job records, monitor events, webhook delivery state, draft posts, write action status
- Trust level: `High`
- Why it matters: social operations often mix research, monitoring, and publishing, so read-only discovery and approved actions need a clear boundary
- Good matches: social listening, campaign reporting, influencer research, giveaway operations, support escalation, approval-before-write publishing
- Review questions: which endpoints are read-only, which actions need explicit approval, and how are API keys kept out of prompts and logs?
- Evidence note: Xquik documents a remote MCP server at https://docs.xquik.com/mcp/overview and publishes its MCP manifest at https://xquik.com/.well-known/mcp.json.

### Repository and issue operations server

- Status: `Pattern seed`
- Primary job: read and update source-control artifacts such as issues, pull requests, releases, and labels
- Typical inputs: repository refs, issue ids, branch names, labels, milestone filters
- Typical outputs: summaries, triage actions, release notes, status comments, review queues
- Trust level: `Medium`
- Why it matters: engineering workflows have clear artifacts and recurring chores, which makes them ideal for agent assistance
- Good matches: release note drafting, changelog generation, triage queues, documentation backfills
- Review questions: what human approval step exists before merge, release, or destructive repo actions?

### Ticketing, CRM, or helpdesk bridge

- Status: `Pattern seed`
- Primary job: move customer context into agent workflows without forcing humans to swivel between tools
- Typical inputs: account ids, ticket ids, requester metadata, product area tags
- Typical outputs: enriched cases, suggested responses, escalation packets, next-step tasks
- Trust level: `Medium`
- Why it matters: this is where workflow speed and better context can compound quickly for support or account teams
- Good matches: support enrichment, onboarding follow-up, renewal risk prep, escalation handling
- Review questions: what data should remain read-only, and how are sensitive fields masked or excluded?

### Calendar and scheduling server

- Status: `Pattern seed`
- Primary job: let an agent reason about meetings, deadlines, and follow-up obligations
- Typical inputs: attendees, date ranges, meeting titles, agenda context, constraints
- Typical outputs: meeting briefs, scheduling suggestions, reminders, follow-up tasks
- Trust level: `Medium`
- Why it matters: scheduling is repetitive, structured, and easy to combine with docs or task systems
- Good matches: meeting prep, onboarding checklists, leadership reviews, cross-team coordination
- Review questions: how do you prevent accidental over-sharing when event metadata spans multiple teams or confidentiality levels?

## Execution Surfaces

### Browser automation bridge

- Status: `Pattern seed`
- Primary job: let an agent inspect or operate authenticated web applications through a browser session
- Typical inputs: URLs, page state, DOM targets, form data, navigation goals
- Typical outputs: captured evidence, screenshots, filled forms, QA notes, verified state transitions
- Trust level: `High`
- Why it matters: unlocks workflows that do not expose clean APIs, especially for QA, operations, and internal admin tools
- Good matches: onboarding checks, regression walks, portal audits, manual back-office procedures
- Review questions: how are approvals, authentication boundaries, and anti-bot or legal constraints handled?

### Local workspace tools server

- Status: `Pattern seed`
- Primary job: give an agent bounded access to local files, commands, and generated artifacts
- Typical inputs: file paths, search queries, shell arguments, patch requests
- Typical outputs: edits, diffs, generated files, summaries, reports
- Trust level: `High`
- Why it matters: often the most powerful server type for builders, but also the one that demands the clearest safety model
- Good matches: codebase refactors, content pipelines, migration prep, local QA automation
- Review questions: what is the allowed write scope, and how are destructive operations prevented or audited?

### Secrets-aware execution wrapper

- Status: `Pattern seed`
- Primary job: allow approved tools to run with access to scoped credentials without spraying secrets into prompts or logs
- Typical inputs: named capabilities, secret aliases, execution intents, environment rules
- Typical outputs: task results, structured logs, approval records, redacted traces
- Trust level: `High`
- Why it matters: many serious workflows die unless secrets are handled deliberately rather than bolted on later
- Good matches: deployment checks, back-office tasks, scheduled operations, approval-gated workflows
- Review questions: are credentials least-privilege, auditable, and revocable?

## Operations and Monitoring

### Monitoring and incident operations server

- Status: `Pattern seed`
- Primary job: expose alerts, incident timelines, dashboards, and response runbooks
- Typical inputs: alert ids, service names, severity filters, recent deployments
- Typical outputs: incident briefs, status updates, responder checklists, correlation hints
- Trust level: `High`
- Why it matters: incidents are structured enough for automation support but risky enough to demand strong guardrails
- Good matches: incident coordination, postmortem drafting, rollout reviews, service health checks
- Review questions: which actions are informative only, and which ones can page, mute, restart, or escalate production systems?

### Communication and approvals server

- Status: `Pattern seed`
- Primary job: move workflow state through chat, email, or approval queues
- Typical inputs: summary drafts, recipients, approval requests, routing rules
- Typical outputs: notifications, approvals, follow-ups, audit trails
- Trust level: `Medium`
- Why it matters: many useful agent workflows fail unless they end in a channel where a human can approve or act
- Good matches: release approvals, escalation loops, executive summaries, compliance handoffs
- Review questions: do the outbound messages preserve the original evidence, or do they collapse nuanced state into false certainty?
