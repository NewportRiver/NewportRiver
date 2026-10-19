# Newport River Autonomous Subsidiary Framework (NR-ASF)
**Status:** Proposal — no operational changes authorized  
**Date:** 2026-10-19  
**Sponsor:** Founder, Newport River Company  
**Pilot:** Newport River Game Studios

## Executive decision requested
Approve a time-boxed pilot of a lean, operationally autonomous game-development unit that reports to Newport River corporate governance through a narrow, authenticated integration. Do **not** authorize production restructuring, credential sharing, spending, or deployment merely by approving this design.

## Problem statement
The current Paperclip corporate organization experiences long queues for cross-agent responses, approvals, and dependent tasks. A small number of underlying model runtimes may occupy many corporate seats; more reporting and handoffs do not necessarily create more execution capacity. Initial infrastructure construction adds latency but does not explain approval queues by itself.

**Hypothesis:** Separating day-to-day execution from corporate governance will reduce approval handoffs and improve verified task throughput without sacrificing security or accountability.

## Operating model
- **Parent — Newport River Company:** strategy, resource ceilings, portfolio reporting, security policy, legal/compliance requirements, high-risk approvals, and audit.
- **Subsidiary — Newport River Game Studios:** product backlog, sprint planning, gameplay design, implementation, local builds, testing, bug triage, and routine reversible changes within pre-approved limits.
- An operational Paperclip subsidiary is **not** automatically a separately incorporated legal entity; legal ownership and branding require separate review.
- Other possible subsidiaries (software, AI research, music) are future options, not part of this pilot.

## Pilot organization (roles, not dedicated models)
1. **Founder / Creative Director:** creative direction, milestone acceptance, exceptional decisions.
2. **Studio Producer / Workflow Controller:** prioritizes one backlog, owns dependencies, schedules available model capacity, escalates blockers.
3. **Game Programmer:** Unreal Engine implementation, code and integration.
4. **Game Designer / Artist:** gameplay specifications, level/art/UI deliverables.
5. **QA / Build Verification:** test plans, reproducible results, regression checks.
Roles may share model runtimes but should retain distinct instructions and task context. Avoid simultaneous heavy local inference workloads unless measured capacity supports them. No extra manager layers in the pilot.

## Authority matrix
| Action | Studio | Corporate / human |
|---|---|---|
| Prioritize backlog and assign routine work | Authorized | Informed via summary |
| Change reversible gameplay code on feature branches | Authorized | Not required |
| Run tests and non-destructive local builds | Authorized | Not required |
| Merge into protected branch | Subject to branch protections and required review | As configured |
| Spend money, accept contracts, establish legal entities | Not authorized | Explicit human approval |
| Issue or elevate credentials; modify security policy | Not authorized | Security approval under founder policy |
| Destructive operations, public releases, production deployment | Not authorized by default | Explicit designated approval |
| Exceed model/compute/budget quotas | Not authorized | Escalation required |

Approval deadlines cause **escalation**, never implicit approval. No agent can approve its own privileged request.

## Technical deployment options — verify before choosing
**Option A:** Separate Paperclip organizations within one deployment, *only if* the installed Newport River fork demonstrably provides adequate organization isolation, API permissions, and reporting.

**Option B:** Separate Paperclip deployments with separate service identities, databases/configuration as appropriate, and a reporting gateway. More isolation but greater operational overhead.

Do not assume native inter-organization API or event support exists. CTO must inspect installed fork, authentication, API endpoints, isolation boundaries, and heartbeat behavior before implementation.

### Reporting integration contract (proposed, not an existing API)
- Studio emits a minimal signed/authenticated summary at an agreed interval (initially daily) and on milestone completion.
- Suggested fields: subsidiary_id, reporting_period, active_tasks, completed_tasks, blocked_tasks, blockers_with_owner_and_age, build/test evidence references, compute usage, approved budget consumption, risks, requests_for_decision.
- Corporate may send *policy envelopes* (budget/quotas/approved objectives) and explicit exceptional decisions, **not** routine ticket-by-ticket instructions.
- Least-privilege scoped identities; TLS where networked; secrets in controlled storage, never in Git; audit log, idempotency keys, retries with backoff, and duplicate-event handling.
- Default data minimization: no full agent memory, private prompts, source secrets, or unnecessary task transcripts in portfolio reports.
- Corporate reporting outage must not block already-authorized studio work; protected actions remain gated.

## Dependency and heartbeat rules
- Single accountable owner and explicit definition of done for each ticket.
- Represent prerequisite edges explicitly; detect cycles and missing owners.
- Independent work proceeds in parallel when compute allows.
- Reviews have target response times and an escalation owner; overdue protected approvals remain pending.
- Measure whether approval notifications trigger wakeups or wait for periodic heartbeat; optimize only after observing actual implementation.
- Studio producer maintains a ready queue and work-in-progress limit of **two** active execution tickets initially; tune from evidence.
- A blocked ticket must include dependency, responsible agent, last action, next action, and escalation time.
- Evidence-based completion: changed files/commit, build result, test output, and acceptance criteria, as applicable.

## Delivery phases and gates
### Phase 0 — Baseline and discovery (days 1–2)
- Capture existing corporate ticket samples, queue latency, handoffs, dependency age, and completion rate.
- Inspect Paperclip fork: multi-org isolation, API auth, webhooks, agent heartbeat/wakeup semantics, role permissions.
- Inventory local model runtime, Unreal MCP access, GitHub repositories, and capacity constraints.
- Deliver architecture decision record (ADR) selecting Option A or B; stop if isolation is inadequate.

### Phase 1 — Sandbox studio (days 3–4)
- Create a non-production pilot organization/deployment with minimal agent roles and a single backlog.
- Apply authority matrix, secrets separation, repo permissions, branch protections, and resource limits.
- Create mock reporting adapter; prove reporting failure does not stop routine execution.
- Test blocked approval, cycle detection, and denial of unauthorized privileged action.

### Phase 2 — Game-development vertical slice (days 5–10)
- Pick one small playable Unreal Engine feature with acceptance tests.
- Run design → code → build → QA → integration using the studio backlog.
- Produce daily structured summaries to corporate; request approvals only for defined exceptions.
- Track failures, GPU/CPU contention, context switching, and manual interventions.

### Phase 3 — Evaluation (days 11–12)
Compare against Phase 0 baseline and similar ticket complexity:
- Median time to first meaningful action
- Median blocked time and approval wait
- Handoffs per completed ticket
- Verified completed tickets per model-hour
- Build/test pass rate and rework
- Unauthorized actions or policy violations (target: zero)
- Human intervention count and resource consumption

**Provisional success criteria:** at least 30% lower median blocked time and 25% more verified completions per model-hour, with no increase in serious quality failures and zero security violations. Thresholds are pilot hypotheses, not promised results.

### Phase 4 — Decide
**Go:** expand subsidiary pattern only if measured improvement and control compliance are demonstrated.  
**Revise:** adjust heartbeat routing, authority boundaries, or role count.  
**No-go:** revert pilot configuration and retain corporate system unchanged.

## Rollback and safeguards
- No changes to the existing parent organization during the pilot without a separately approved change request.
- Keep configuration and agent instruction changes versioned; use separate branches and sandbox resources.
- Maintain recovery backups and tested restore instructions before integration.
- Disable integration credentials and pause pilot agents to stop the experiment; preserve audit logs.
- No automated credential transfer, unrestricted API write access, or cross-company task reassignment.

## Ownership and first tickets
| Ticket | Owner | Output |
|---|---|---|
| NR-ASF-001 | CTO / IT | Paperclip capability and security assessment |
| NR-ASF-002 | COO / PMO | Dependency/approval baseline report |
| NR-ASF-003 | Security / Legal | Delegation, identity, and legal-entity boundaries |
| NR-ASF-004 | Game Studio Producer | Lean roster, vertical-slice backlog, acceptance criteria |
| NR-ASF-005 | Integration Engineer | Read-only reporting contract and sandbox prototype |
| NR-ASF-006 | QA | Pilot scorecard and rollback test |

## Governing principle
**Corporate governs boundaries; subsidiaries execute inside those boundaries.** A reporting API must never recreate the approval bureaucracy it was designed to eliminate.
