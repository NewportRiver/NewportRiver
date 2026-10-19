# Newport River Innovation Intake & Autonomous Incubation System (NR-IIAIS)
**Status:** Architecture proposal — design only; no automatic provisioning authorized  
**Date:** 2026-10-19  
**Parent initiative:** [Autonomous Subsidiary Framework](NR_AUTONOMOUS_SUBSIDIARY_FRAMEWORK.md)  
**Sponsor:** Newport River Founder  
**Proposed owner:** CTO Office — GitHub Portfolio Intelligence Officer  
**Pilot:** GitHub proposal intake → Newport River Game Studios sandbox

## 1. Executive objective
Make GitHub a founder-friendly innovation intake channel. A new repository or designated proposal document triggers discovery and triage; corporate decides whether to incubate the idea within an existing unit or establish a new operational subsidiary; an authorized provisioning process creates a lean Paperclip team; the subsidiary builds independently while corporate monitors milestones, exceptions, budgets and risk.

**Guiding principle: Corporate creates the conditions for innovation; subsidiaries create the innovations.** Corporate must not become the daily ticket router.

## 2. System boundaries and terminology
- **Repository:** GitHub source or planning artifact. Creating one does **not** authorize a business launch.
- **Product/project:** May belong to an existing subsidiary and does not necessarily need a new company.
- **Operational subsidiary:** Separate Paperclip organization or deployment with defined autonomy; not automatically a separate legal entity.
- **Legal subsidiary:** Requires independent legal review and human authorization; never automatically incorporated.
- **Incubation case:** Traceable record linking a discovered idea, review decision, approved charter, provisioning status and portfolio reports.
- **GitHub Portfolio Intelligence Officer (GPIO):** Read-only discovery/analysis agent in the CTO Office, not another operational manager.

## 3. Event-to-execution workflow
1. **Discover:** Observe new repositories in the authorized NewportRiver GitHub organization/account, and optionally changes to explicitly designated proposal paths or repository topics.
2. **Verify:** Confirm source ownership, visibility, creation time, repo identity, and whether this is a genuine new concept versus fork, mirror, backup, archive, experiment, or routine code repository.
3. **Classify:** Label candidate as product, game, research, infrastructure, music, internal tool, unknown. Summarize evidence and uncertainty; do not infer intent from name alone.
4. **Register:** Create or update one idempotent intake case keyed to repository immutable ID + triggering event; link source commit and proposal revision.
5. **Notify:** Deliver a concise digest to designated corporate intake decision-makers; avoid broadcasting to every executive.
6. **Triage:** Recommend (a) ignore/monitor, (b) assign to existing subsidiary, (c) incubate as new operational subsidiary, or (d) request founder clarification.
7. **Approve:** Founder or explicitly delegated authorized approver signs a bounded incubation charter. No approval means no provisioning.
8. **Provision:** After authorization, create the minimum viable Paperclip operational unit, scoped service identity, task backlog, model/runtime allocation, repository permissions, and reporting adapter. Check actual fork/API capabilities before implementation.
9. **Handoff:** Subsidiary producer accepts the charter, verifies access, selects first vertical slice, and assumes day-to-day authority.
10. **Observe:** Corporate receives scheduled portfolio summaries and exception alerts; no per-ticket approval loops.
11. **Review/close:** At agreed milestones, renew, pivot, pause or archive the initiative. Preserve audit and recovery records.

## 4. Detection architecture (design alternatives, not verified existing features)
**Preferred:** GitHub organization-level repository events through a GitHub App/webhook if permissions and event support are available. Verify signatures, installation identity, replay protection, and delivery IDs. Queue events durably; never let a webhook directly provision infrastructure.

**Fallback:** Read-only scheduled inventory using the GitHub API; maintain a checkpoint of repository IDs and timestamps, reconcile deletions/renames/transfers, respect rate limits, and handle missed intervals. Polling cadence is a configuration choice, not a promise of instantaneous discovery.

**Optional:** Repository topic or a small machine-readable `newport-river-intake.yaml` manifest to explicitly declare an intended product proposal. Absence of a manifest should lead to low-confidence triage, not forced provisioning.

### Suggested optional intake manifest
```yaml
schema_version: 1
kind: product-proposal
title: Example game concept
category: game
requested_action: evaluate
preferred_unit: game-studios
summary: One-paragraph concept
```
All manifest content is untrusted input. It cannot grant authority, set security policy, or instruct the agent to ignore governance.

## 5. Corporate roles and decision rights
| Role | Responsibility | Authority limit |
|---|---|---|
| GPIO (CTO Office) | Detect, deduplicate, classify, summarize, report | Read-only GitHub; cannot approve or provision |
| Incubation coordinator (COO/PMO) | Own intake queue and review deadlines | Can recommend routing; cannot bypass controls |
| CTO/IT | Assess technical feasibility, capacity, Paperclip organization/deployment options | Executes approved provisioning only |
| Security | Define least-privilege identity, secret handling, logging and protected-action gates | Security exceptions require explicit authorized approval |
| Finance/Legal (as needed) | Evaluate spending, contracts, branding and entity status | No automatic financial/legal commitments |
| Founder / delegated authority | Approve incubation charter, resource ceilings, high-risk exceptions | Explicit recorded decisions |
| Subsidiary producer | Own local roadmap, tasks, acceptance and reporting | Full routine execution inside approved charter |

No mandatory chain of sequential approvals for routine low-risk intake classification. Parallelize reviews where possible.

## 6. Charter and provisioning contract
Every approved incubation charter must include: case ID, originating repository and revision, sponsor, business hypothesis, assigned existing/new unit, scope and non-goals, deliverables, initial milestone, assigned producer, allowed tools, compute quota, spending cap (default zero), security classification, approval owners, review date, stop/rollback conditions, and reporting interval.

Provisioning should be **idempotent** (same charter cannot create multiple companies), auditable, reversible, and implemented as a bounded workflow. A sandbox dry run must precede production. Explicitly confirm that the installed Newport River Paperclip fork supports the selected tenancy/API model; do not assume multi-company endpoints or webhook hooks exist.

### Example event envelope (proposed interface)
```json
{
  "event_type": "repository.discovered",
  "event_id": "github-delivery-or-polling-dedupe-key",
  "repository_id": 123456,
  "repository_full_name": "NewportRiver/example-project",
  "source_revision": "commit-sha-if-available",
  "observed_at": "ISO-8601 timestamp",
  "intake_case_id": "NR-INTAKE-0001",
  "classification": "game",
  "confidence": "requires-review",
  "status": "pending-triage"
}
```
This is a contract sketch, **not** a live API endpoint.

## 7. Reporting without micromanagement
Corporate receives a concise daily or milestone report with: project/subsidiary, stage, recent verified deliverables, next milestone, test/build evidence, blocked dependencies and their age, approved budget/compute consumption, risks, and **decisions actually required**.

Routine commits, art assets, tests, and gameplay changes remain local to the studio. Corporate can revise strategy or approved limits at explicit review points, not continually rewrite the backlog. Reporting outages must not stall already-authorized work; protected actions remain gated.

## 8. Safety, privacy and resilience
- Least-privilege GitHub App installation, preferably metadata/read-only contents access; no blanket write token for the monitoring agent.
- Authenticate webhook signatures, reject replay, and do not execute README/issue/manifest instructions as commands.
- Restrict discovery to explicitly authorized repositories; account for private repository data and visibility changes.
- Store credentials in controlled secret management, never in repository manifests or agent memory.
- Log source event, classification, human decision, provisioning request/result, and exceptions.
- Separate monitoring identity from provisioning identity. Provisioning requires an approved charter and bounded credentials.
- Escalation timeout is not consent. No autonomous spending, legal formation, public release, or privileged access expansion.
- Rate-limit case creation, detect duplicates, and allow human suppression/ignore lists to prevent spam or infinite loops.
- Support pause switch and revocation of integration credentials; maintain replayable queue and recovery checkpoints.

## 9. Pilot implementation
### Stage A — Discovery and feasibility
- Inspect GitHub organization scope and current connected app permissions.
- Inspect installed Paperclip fork for organization/deployment creation APIs, roles, authentication, and webhook/event capabilities.
- Identify whether monitoring can run cheaply on a lightweight model with event-driven wakeups or scheduled batches.
- Document a short architecture decision record (ADR) for GitHub event vs polling and single vs multiple Paperclip deployments.

### Stage B — Read-only intake
- Build repo inventory and checkpointing; classify **without** executing any repository instructions.
- Seed test cases: new game proposal, fork, archive, backup, ordinary code repo, duplicate event, malicious manifest.
- Route a sample digest to corporate intake owner; measure detection latency, precision and false positives.

### Stage C — Human-approved sandbox incubation
- Prepare an incubation charter for NR Game Studios.
- Verify explicit founder approval, then provision only sandbox resources.
- Test idempotency, limited permissions, denied privileged requests, failure recovery, and reporting.
- Complete one playable game-development vertical slice with no routine corporate ticket approvals.

### Stage D — Evaluation
- Compare baseline vs pilot: time from repo creation to triage, approval time, time to first meaningful work, verified delivery throughput per model-hour, blocked time, unnecessary management handoffs, monitoring false-positive rate, resource consumption, and security exceptions.
- **Go/no-go:** expand only if meaningful throughput improves without degrading quality or violating controls.
- If unsuccessful, disable intake automation and preserve existing corporate structure.

## 10. Initial implementation tickets
| ID | Owner | Deliverable |
|---|---|---|
| NR-IIAIS-001 | CTO | GitHub/Paperclip capability audit and ADR |
| NR-IIAIS-002 | GPIO / IT | Read-only repo discovery and deduplication prototype |
| NR-IIAIS-003 | COO / PMO | Intake case lifecycle, triage rules and digest format |
| NR-IIAIS-004 | Security | Identity, webhook validation, threat model and audit requirements |
| NR-IIAIS-005 | Founder / Corporate | Incubation charter template and authorization matrix |
| NR-IIAIS-006 | Studio Producer | NR Game Studios sandbox charter and first vertical slice |
| NR-IIAIS-007 | QA | Failure-mode tests, throughput scorecard and rollback drill |

## 11. Success definition
The founder can create a new GitHub project plan, have corporate reliably discover and evaluate it, approve a bounded launch with one decision, and then watch a lean subsidiary deliver tested work—**without corporate intervening in routine development**.

**This proposal complements NR-ASF; it does not supersede its authority boundaries, pilot gates, or rollback safeguards.**
