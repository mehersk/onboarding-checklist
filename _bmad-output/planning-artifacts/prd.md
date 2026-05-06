---
stepsCompleted: ["step-01-init", "step-02-discovery", "step-02b-vision", "step-02c-executive-summary", "step-03-success", "step-04-journeys", "step-05-functional", "step-06-nonfunctional", "step-07-epics", "step-08-stories", "step-12-complete"]
inputDocuments:
  - "_bmad-output/planning-artifacts/product-brief-onboarding-checklist.md"
workflowType: 'prd'
briefCount: 1
researchCount: 0
brainstormingCount: 0
projectDocsCount: 0
classification:
  projectType: web_app
  projectTypeNote: "Role-based workflow platform — four distinct user mental models sharing a database; treat background job system and notification pipeline as first-class architectural concerns, not features"
  domain: hr-tech
  domainNote: "Employment records, milestone audit trails, and pulse scores constitute personnel data with GDPR/CCPA exposure; data retention, soft-delete semantics, and audit log integrity are explicit PRD requirements"
  complexity: high
  complexityNote: "Dual SSO paths, temporal data model (snapshot-at-assignment versioning), business-day calendaring for escalation, multi-channel notifications with retry semantics, four-role UX"
  projectContext: greenfield
  architecturalConstraints:
    - "Self-hosted behind corporate firewall — no managed services available for scheduler or notification pipeline; both must be designed as first-class system concerns from day one"
---

# Product Requirements Document - Developer Onboarding Checklist App

**Author:** Meher
**Date:** 2026-05-06

## Executive Summary

The Developer Onboarding Checklist App is an internal, self-hosted web platform that replaces ad-hoc engineering onboarding — Confluence wikis, Slack questions, manual check-in calls — with a structured, role-aware, accountable system for the first 30–90 days of every new engineer's journey.

**Context:** A senior backend engineer joined last quarter and submitted their first PR on day 11, not day 5 — a week spent chasing access credentials and asking Slack questions that nobody owned. The org is about to hire 15 engineers across two quarters; the next cohort starts in 8 weeks. Broken onboarding at 50-person scale is painful. At 100-person scale, it compounds.

**The core problem is not missing checklists — it is missing accountability across three relationships simultaneously:**
- New joiners lack clarity on what "done" looks like at any phase boundary
- Team Leads have no visibility into joiner progress without scheduling a synchronous check-in
- HR cannot update onboarding content without filing an engineering ticket

**Target users:** New Joiners (primary — engineers in their first 30–90 days); Team Leads (primary — engineering managers and senior leads responsible for ramp); HR/People Ops (co-primary — own the template content surface); Engineering Managers (escalation path when Team Lead sign-off lapses beyond 5 business days).

**North star:** No new joiner should ever have to ask *"what am I supposed to be doing right now?"* — and no Team Lead should have to schedule a meeting to find out if their joiner is okay.

**The vision at 6 months:** Onboarding is boring. New joiners are heads-down and productive. Team Leads stopped scheduling weekly check-ins because the dashboard already told them. HR publishes template updates that nobody notices because they just work. The org has forgotten what the old way felt like.

**Design principle — the tool must feel invisible to the joiner.** It must not feel like compliance software or a ticketing system. If it feels like paperwork, adoption dies. Every UX decision is filtered through this constraint: the joiner's surface should feel like a personal task list, not an HR form.

### What Makes This Special

Not a generic task manager. Not a developer portal. Not a wiki. Three things together — in a single lightweight, self-hosted tool — that no current solution delivers:

1. **Role-typed by default.** Five engineering tracks (Frontend, Backend, DevOps, QA, Custom) are first-class, not configured-in afterthoughts. A Backend joiner and a DevOps joiner get fundamentally different checklists from Day 1 — no forcing anyone into a template that's 60% relevant.

2. **Structured accountability without overhead.** Milestone sign-off at phase boundaries — not per-task approvals — gives Team Leads a structured touchpoint at the natural rhythm of engineering progress. Automatic escalation to Engineering Manager after 5 business days removes the need for manual intervention when a Team Lead is unavailable. Joiners can flag blocked tasks in-product with a one-line reason — keeping friction inside the system rather than leaking back to Slack.

3. **HR template ownership without engineering dependency.** HR can draft, version, and publish templates without opening a ticket. Snapshot-at-assignment versioning ensures in-progress joiners are never disrupted by a template update. The org/content boundary is structurally enforced, not a convention that relies on discipline.

**Why not the alternatives:** BambooHR and Workday don't know what a backend engineer needs on Day 1. Backstage requires a dedicated platform team to maintain. Notion and Confluence lack enforced workflow, sign-off accountability, and audit semantics. This fills the gap between HR tooling and developer portals — purpose-built for engineering roles, lightweight enough for a 50–100 person org to actually adopt.

**Self-hosted architectural forcing function:** All background scheduling (overdue reminders, escalation timers) and notification pipelines (email, Slack) are first-class system design concerns — no managed services available behind the corporate firewall. This is not a deployment detail; it shapes the architecture from day one.

## Project Classification

| Dimension | Value |
|---|---|
| **Project Type** | `web_app` — role-based workflow platform; four distinct user mental models sharing a database |
| **Domain** | `hr-tech` with data privacy overlay — onboarding progress records, milestone sign-off audit trails, and pulse scores are personnel data; GDPR/CCPA exposure applies; data retention, soft-delete semantics, and audit log integrity are explicit PRD requirements |
| **Complexity** | High — dual SSO auth paths (Google Workspace / Azure AD), temporal data model (snapshot-at-assignment versioning), business-day calendaring for escalation timers, multi-channel notification pipeline, four-role UX with distinct IA per persona |
| **Project Context** | Greenfield |
| **Scale** | 50–100 engineers, single internal engineering org; not a SaaS product |
| **Arch Constraint** | Self-hosted behind corporate firewall — scheduler and notification pipeline are first-class system concerns from day one |

---

## Success Criteria

### User Success

The product succeeds for users when:

- **New joiners never ask "what am I supposed to be doing right now."** Day 1 delivers a role-specific checklist that answers this question without human intervention. The joiner's surface feels like a personal task list — not a compliance form, not a ticketing system. If it feels like paperwork, this criterion is failing.
- **Joiners feel unblocked at end of Week 1.** Captured via the milestone pulse score (1–5). Target: average ≥4/5 across the first three hiring cohorts. A joiner who is blocked can flag the specific task in-product with a one-line reason — no Slack message required.
- **Team Leads get progress visibility without scheduling a call.** Opening the dashboard replaces the weekly "how's it going?" check-in. The pulse score surfaces early-warning signals before they require a conversation.
- **HR publishes a template update and nobody notices.** Snapshot-at-assignment versioning means the update silently applies to new joiners only. In-progress joiners are unaffected. No tickets, no coordination overhead.

### Business Success

Measured at 60 days post-launch across the first cohort of new hires:

| Metric | Target | Measurement Method |
|---|---|---|
| Time-to-first-PR | < 5 days from start date | Git history, per role and seniority band |
| HR "what do I do next?" Slack messages in Week 1 | Near zero | Qualitative confirmation with HR team |
| Team Leads scheduling dedicated Week 1 check-in calls | ≤20% (≥80% report no call scheduled) | Post-launch survey at 60 days |
| Joiner Week 1 pulse score | ≥4/5 average | Captured in-product at Week 1 milestone sign-off |

**Caveat on time-to-first-PR:** This metric is influenced by access provisioning speed and PR review queue depth — neither controlled by this tool. It is the primary signal but should be read alongside the pulse score, not in isolation.

**Scale target:** All 15 engineers hired over the next two quarters onboard through the tool. Zero fallback to the old Confluence/Slack process.

### Technical Success

- Both SSO paths (Google Workspace and Azure AD) operational on day one of deployment
- Self-hosted deployment clean behind corporate firewall — no external SaaS dependencies, no data leaving the network
- Background scheduler reliable — overdue reminders fire at correct business-day intervals; escalation triggers at exactly 5 business days (not wall-clock days); holiday-aware
- Notification pipeline delivers with retry semantics — failed deliveries are logged and retried; no silent failures
- Audit log integrity maintained — milestone sign-off records are immutable and timestamped; escalation trail is not editable by any role including admins
- Template versioning correct — zero in-progress joiners disrupted by any template update under concurrent load
- Data retention implemented — records and pulse scores are soft-deleted; configurable retention period; data export available for right-of-access requests

### Measurable Outcomes

**v1 is working when all four of the following are true at 60-day post-launch review:**
1. Time-to-first-PR is below 5 days for the majority of the new cohort
2. Joiner Week 1 pulse average is ≥4/5 across the cohort
3. HR confirms "what do I do next?" messages have materially dropped
4. ≥80% of Team Leads report no dedicated onboarding check-in call in their most recent new joiner's first week

---

## Product Scope

### MVP — Minimum Viable Product

Everything required for the tool to be the single authoritative onboarding channel for the next cohort (8-week delivery target):

- Role-specific checklists: Frontend, Backend, DevOps, QA, Custom (5 tracks)
- Configurable ramp period: 30 / 60 / 90 days by seniority
- Weekly phase grouping with milestone sign-off (Week 1, Week 2, final completion)
- Automatic sign-off escalation to Engineering Manager after 5 business days (business-day aware)
- Joiner "flag as blocked" per task — flag icon + one-line free-text reason
- Milestone pulse score — one 1–5 question per sign-off, visible on Team Lead dashboard
- HR self-service template management with version-safe publishing (snapshot-at-assignment)
- Team Lead progress dashboard — per-joiner, per-phase, including pulse scores and blocked flags
- SSO authentication — Google Workspace and Azure AD
- Email and Slack reminders for overdue tasks (with retry semantics)
- Self-hosted deployment behind corporate firewall
- Soft-delete with audit log integrity; basic data export for GDPR/CCPA compliance
- **Pre-launch gate (not a product feature):** Joint engineering + HR content session to author initial task lists for all 5 role tracks

### Growth Features (Post-MVP)

- GitHub PR merge events confirming task completion automatically
- Jira ticket integration for access-provisioning tasks
- HR and leadership cohort analytics dashboard (ramp time trends, pulse trends)
- Slack-native notifications for milestone sign-offs
- Mobile web experience

### Vision (Future)

- Authoritative record of every hire's first 90 days — pulse scores feeding into team health metrics and performance review context
- Cohort comparison: ramp time and experience quality across quarters and hiring waves
- Automated access provisioning tied to checklist task completion (IAM integration)
- Onboarding quality as a metric in engineering leadership OKRs

---

## Functional Requirements

### FR-1: Authentication & Authorisation

| ID | Requirement |
|---|---|
| FR-1.1 | The system MUST authenticate all users via SSO using Google Workspace or Azure AD. No username/password fallback. |
| FR-1.2 | The system MUST support four roles: New Joiner, Team Lead, HR Admin, Engineering Manager. |
| FR-1.3 | Role assignment MUST be configurable by an HR Admin or system admin at time of user provisioning. |
| FR-1.4 | A user's role MUST determine their accessible views and permitted actions — enforced server-side. |
| FR-1.5 | A Team Lead MUST only see the joiners assigned to them. An Engineering Manager MUST see all active joiners in their reporting chain. |
| FR-1.6 | HR Admins MUST have access to template management only — not to individual joiner progress records. |

### FR-2: New Joiner Checklist Experience

| ID | Requirement |
|---|---|
| FR-2.1 | Upon first login, a New Joiner MUST see their assigned role-specific checklist with all tasks grouped by weekly phase. |
| FR-2.2 | The checklist MUST display each task with: title, phase label, status (Pending / Complete / Blocked), and — if blocked — the joiner's one-line reason. |
| FR-2.3 | A New Joiner MUST be able to mark any pending task as complete with a single interaction. |
| FR-2.4 | A New Joiner MUST be able to flag any task as blocked by clicking a flag icon and entering a free-text reason (max 200 characters). |
| FR-2.5 | A New Joiner MUST be able to un-flag a blocked task and return it to pending status. |
| FR-2.6 | The checklist MUST display a progress indicator showing tasks completed vs. total tasks within each phase and overall. |
| FR-2.7 | The joiner's ramp period (30 / 60 / 90 days) and end date MUST be visible on their checklist view. |
| FR-2.8 | Tasks in future phases MUST be visible but clearly delineated as upcoming — joiners can preview but not complete them out of order. |
| FR-2.9 | The joiner's view MUST feel like a personal task list — no HR framing, no compliance language, no ticket numbers. |

### FR-3: Milestone Sign-off & Pulse

| ID | Requirement |
|---|---|
| FR-3.1 | The system MUST define milestone boundaries at the end of Week 1, Week 2, and at final completion of all tasks. |
| FR-3.2 | When a milestone boundary is reached (all tasks in a phase completed), the assigned Team Lead MUST receive a sign-off prompt. |
| FR-3.3 | A Team Lead MUST be able to approve a milestone sign-off from their dashboard. |
| FR-3.4 | At the point of Team Lead sign-off, the joiner MUST be presented with a single pulse question: "How is your onboarding going this week? 1–5". |
| FR-3.5 | The pulse score MUST be stored and displayed on the Team Lead's dashboard alongside the phase progress bar. |
| FR-3.6 | If a Team Lead has not signed off a milestone within 5 business days of it becoming due, the system MUST automatically grant sign-off rights to the Engineering Manager and notify them. |
| FR-3.7 | Business days MUST exclude weekends. A configurable list of public holidays MUST be supported. |
| FR-3.8 | An Engineering Manager MUST be able to complete a sign-off on behalf of a Team Lead. The record MUST note that sign-off was completed by the EM, not the Team Lead. |

### FR-4: Team Lead Dashboard

| ID | Requirement |
|---|---|
| FR-4.1 | A Team Lead MUST see a dashboard listing all active joiners assigned to them, with per-joiner progress (phase, % complete, days remaining). |
| FR-4.2 | The dashboard MUST surface any joiners with overdue tasks or missed milestone sign-offs. |
| FR-4.3 | A Team Lead MUST be able to drill into a specific joiner's full checklist view — read-only. |
| FR-4.4 | Blocked tasks MUST be surfaced on the dashboard with the joiner's stated reason, without requiring the Team Lead to drill into the checklist. |
| FR-4.5 | Pulse scores for each completed milestone MUST be visible alongside the phase name on the Team Lead view. |

### FR-5: HR Template Management

| ID | Requirement |
|---|---|
| FR-5.1 | An HR Admin MUST be able to create, edit, and delete checklist templates for each of the 5 role tracks (Frontend, Backend, DevOps, QA, Custom). |
| FR-5.2 | An HR Admin MUST be able to add, edit, reorder, and delete tasks within a template. Each task has a title (required), description (optional), and phase assignment (Week 1, Week 2, etc.). |
| FR-5.3 | Publishing a template update MUST create a new version. The published version MUST only apply to joiners assigned after the publish date. |
| FR-5.4 | Joiners currently in onboarding MUST remain on the template version that was active when they were assigned — regardless of subsequent template updates. |
| FR-5.5 | An HR Admin MUST be able to view which version of a template each active joiner is assigned to. |
| FR-5.6 | The Custom role track MUST allow a Team Lead (not only HR) to add or remove tasks from an assigned joiner's checklist after assignment, to accommodate non-standard roles. |
| FR-5.7 | Template changes MUST NOT require engineering involvement at any stage. |

### FR-6: Notification & Reminder Engine

| ID | Requirement |
|---|---|
| FR-6.1 | The system MUST send reminders to a New Joiner when they have overdue tasks (tasks not completed by the end of their scheduled phase). |
| FR-6.2 | Reminders MUST be delivered via email, Slack, or both — configurable per user at onboarding setup. |
| FR-6.3 | The reminder schedule MUST be: Day 1 after overdue, Day 3 after overdue. No further automated reminders beyond that. |
| FR-6.4 | The system MUST notify a Team Lead when a milestone sign-off becomes due. |
| FR-6.5 | If the Team Lead has not signed off within 5 business days, the system MUST notify the Engineering Manager of the escalation and grant them sign-off rights. |
| FR-6.6 | All notification delivery attempts MUST be logged. Failed deliveries MUST be retried at least once before being marked as failed. |
| FR-6.7 | Notification failures MUST NOT affect core application functionality. |

### FR-7: Data Management & Audit

| ID | Requirement |
|---|---|
| FR-7.1 | All milestone sign-off records MUST be immutable once created — no admin role may edit or delete a sign-off record. |
| FR-7.2 | Sign-off records MUST include: joiner ID, phase, timestamp (UTC), signing user ID, signing user role (Team Lead or Engineering Manager), and pulse score if applicable. |
| FR-7.3 | Escalation events MUST be logged: timestamp of milestone becoming due, timestamp of escalation trigger, and EM sign-off timestamp if completed. |
| FR-7.4 | Onboarding records (tasks, sign-offs, pulse scores) MUST be soft-deleted — not hard-deleted. A configurable retention period (default: 3 years) MUST be supported. |
| FR-7.5 | The system MUST provide a data export function — per joiner — that produces a structured export of all onboarding records for GDPR/CCPA right-of-access requests. |

---

## Non-Functional Requirements

### NFR-1: Performance

| ID | Requirement |
|---|---|
| NFR-1.1 | The New Joiner checklist page MUST load within 2 seconds for 95% of requests under normal load. |
| NFR-1.2 | The Team Lead dashboard MUST load within 3 seconds for 95% of requests when displaying up to 20 active joiners. |
| NFR-1.3 | Task completion and blocking actions MUST be acknowledged to the user within 500ms. |
| NFR-1.4 | The system MUST support up to 100 concurrent authenticated users without degradation. |

### NFR-2: Security

| ID | Requirement |
|---|---|
| NFR-2.1 | Authentication MUST use SSO exclusively — no local credential store. |
| NFR-2.2 | All data MUST remain within the corporate network — no external API calls that transmit personnel data. |
| NFR-2.3 | All inter-service communication MUST use TLS. |
| NFR-2.4 | Role-based access control MUST be enforced server-side on every API request — client-side hiding alone is insufficient. |
| NFR-2.5 | Session tokens MUST expire after 8 hours of inactivity. |
| NFR-2.6 | The audit log MUST be append-only at the database level — write permissions for the sign-off table must be insert-only for the application role. |

### NFR-3: Availability & Reliability

| ID | Requirement |
|---|---|
| NFR-3.1 | The system MUST maintain ≥99.5% availability during business hours (08:00–20:00, Mon–Fri, org timezone). |
| NFR-3.2 | Scheduled maintenance windows MUST be outside business hours. |
| NFR-3.3 | The background scheduler MUST recover automatically from restarts without losing pending jobs or firing duplicate notifications. |
| NFR-3.4 | Application and scheduler MUST be independently deployable — a scheduler restart MUST NOT require application downtime. |

### NFR-4: Data Privacy & Compliance

| ID | Requirement |
|---|---|
| NFR-4.1 | Pulse scores and onboarding progress records MUST be treated as personnel data — access governed by role; no anonymous or aggregate-only access at the individual level. |
| NFR-4.2 | The system MUST support configurable data retention periods (minimum 1 year, default 3 years). |
| NFR-4.3 | Deleted records MUST be soft-deleted with a tombstone record; hard deletion MUST only occur after the configured retention period expires, via a scheduled purge job. |
| NFR-4.4 | A per-joiner data export (JSON or CSV) MUST be producible by a system admin within 24 hours of a right-of-access request. |

### NFR-5: Operability

| ID | Requirement |
|---|---|
| NFR-5.1 | The system MUST be deployable as a self-contained application behind the corporate firewall with no external SaaS dependencies. |
| NFR-5.2 | Deployment MUST be documented and reproducible — a new environment MUST be bootstrappable by a single engineer in under 4 hours. |
| NFR-5.3 | Application logs MUST be structured (JSON) and written to stdout — compatible with the org's existing log aggregation tooling. |
| NFR-5.4 | A health-check endpoint MUST be available for infrastructure monitoring. |
| NFR-5.5 | The scheduler and notification pipeline MUST emit structured logs for every job execution — success, failure, and retry — so on-call engineers can diagnose delivery issues without database access. |

### NFR-6: Usability

| ID | Requirement |
|---|---|
| NFR-6.1 | The New Joiner view MUST be operable with no training — a new hire with no prior context MUST be able to complete their first task within 2 minutes of first login. |
| NFR-6.2 | The application MUST be accessible at WCAG 2.1 AA level. |
| NFR-6.3 | The application MUST function correctly on the latest two versions of Chrome, Firefox, and Safari on desktop. Mobile browsers are out of scope for v1. |

---

## Epics & User Stories

Stories are ordered by delivery priority. New Joiner checklist experience (E1) is first; the anchor story NJ-1 leads.

---

### E1 — New Joiner Onboarding Experience ⭐ Highest Priority

*The joiner's entire onboarding surface. Every story in this epic is filtered through the design principle: the tool must feel invisible — a personal task list, never a compliance form.*

---

#### NJ-1 ⚓ ANCHOR — First Login: Role-Specific Checklist

**Story:** As a new joiner, when I log in for the first time, I can see my role-specific checklist with all pending tasks clearly listed.

**Acceptance Criteria:**

- **AC-NJ-1.1** Given I am a New Joiner authenticated via SSO and my onboarding has been set up, when I land on my dashboard for the first time, then I see my assigned role track (e.g., "Backend Engineer") displayed prominently and my checklist of tasks below it.
- **AC-NJ-1.2** Given my checklist is displayed, then all tasks are presented in phase order (Week 1 tasks first), each showing: task title, phase label, and status (Pending by default on first login).
- **AC-NJ-1.3** Given I have tasks across multiple phases, then Week 1 tasks are shown in an expanded state; subsequent phases are visible and clearly labelled but visually delineated as upcoming.
- **AC-NJ-1.4** Given my ramp period has been configured (30, 60, or 90 days), then my expected completion date is visible on the page.
- **AC-NJ-1.5** Given the page loads, then it MUST render within 2 seconds on the corporate network.
- **AC-NJ-1.6** Given the design principle, then the checklist page contains no HR-department framing, no ticket numbers, no form-like UI patterns — the visual presentation is that of a personal task list.
- **AC-NJ-1.7** Given I have not yet been assigned a checklist (edge case: onboarding setup incomplete), then I see a clear holding message explaining that my onboarding is being set up, with a contact prompt — I do not see a broken or empty UI.

---

#### NJ-2 — Complete a Task

**Story:** As a new joiner, I can mark a task as complete so I can track my progress through the checklist.

**Acceptance Criteria:**

- **AC-NJ-2.1** Given a task is in Pending status, when I mark it as complete, then the task status changes to Complete immediately and the change is persisted.
- **AC-NJ-2.2** Given I mark a task complete, then my phase and overall progress indicators update without a full page reload.
- **AC-NJ-2.3** Given a task is marked Complete, then I can undo it (return to Pending) — tasks can be toggled until the milestone is signed off by my Team Lead.
- **AC-NJ-2.4** Given my Team Lead has signed off a milestone phase, then tasks within that phase are locked — I cannot toggle their status.
- **AC-NJ-2.5** Given I complete the last task in a phase, then I receive an in-app confirmation that the phase is complete and that my Team Lead has been notified to sign off.

---

#### NJ-3 — Flag a Task as Blocked

**Story:** As a new joiner, I can flag a task as blocked with a one-line reason so I can signal an impediment without sending a Slack message.

**Acceptance Criteria:**

- **AC-NJ-3.1** Given a task is in Pending status, when I click the flag icon, then I am prompted to enter a one-line reason (max 200 characters, required).
- **AC-NJ-3.2** Given I submit the blocked reason, then the task status changes to Blocked, the reason is displayed on my checklist, and the blocked state is immediately visible on my Team Lead's dashboard.
- **AC-NJ-3.3** Given a task is Blocked, then I can un-flag it — the task returns to Pending and the blocked flag is cleared from the Team Lead's dashboard.
- **AC-NJ-3.4** Given a task is Blocked, then I cannot also mark it as Complete — blocked and complete are mutually exclusive states.
- **AC-NJ-3.5** Given the design principle, then the blocked flag UI uses a minimal icon interaction — it does not feel like filing a support ticket.

---

#### NJ-4 — Progress Visibility

**Story:** As a new joiner, I can see my overall progress so I know how far through my onboarding I am at any moment.

**Acceptance Criteria:**

- **AC-NJ-4.1** Given my checklist is displayed, then a progress indicator shows tasks completed vs. total tasks for the current phase (e.g., "3 of 8 tasks complete — Week 1").
- **AC-NJ-4.2** Given my checklist is displayed, then an overall progress indicator shows total tasks completed vs. total tasks across all phases.
- **AC-NJ-4.3** Given I am past Week 1, then completed and signed-off phases are visually distinct from the active phase and upcoming phases.
- **AC-NJ-4.4** Given my ramp end date is within 5 business days, then a visual indicator surfaces this so I am aware time is running short.

---

#### NJ-5 — Overdue Task Reminders

**Story:** As a new joiner, I receive a reminder when I have overdue tasks so I don't fall behind without realising it.

**Acceptance Criteria:**

- **AC-NJ-5.1** Given a task is not completed by the end of its scheduled phase, then I receive a reminder notification on Day 1 after the phase end date.
- **AC-NJ-5.2** Given the task remains incomplete, then I receive a second reminder on Day 3 after the phase end date. No further automated reminders are sent.
- **AC-NJ-5.3** Given my notification preference is email, then the reminder is delivered to my work email address.
- **AC-NJ-5.4** Given my notification preference is Slack, then the reminder is delivered as a Slack DM to my user account.
- **AC-NJ-5.5** Given a reminder delivery fails, then the system retries once before logging a delivery failure — the core application is unaffected.
- **AC-NJ-5.6** Given the design principle, then reminder messages are phrased as helpful nudges — not as warnings or HR escalation notices.

---

#### NJ-6 — Pulse Score at Milestone

**Story:** As a new joiner, I am asked how my onboarding is going at each milestone so my experience is captured alongside my progress.

**Acceptance Criteria:**

- **AC-NJ-6.1** Given all tasks in a phase are complete, when my Team Lead's sign-off prompt is triggered, then I am simultaneously presented with the pulse question: "How is your onboarding going this week? 1–5".
- **AC-NJ-6.2** Given the pulse prompt is shown, then it is a single-tap / single-click interaction — 5 numbered options, no open text required.
- **AC-NJ-6.3** Given I submit my pulse score, then it is stored against my record for this milestone and is not editable after submission.
- **AC-NJ-6.4** Given the pulse prompt, then I cannot dismiss it without answering — it is a lightweight required step, not optional.
- **AC-NJ-6.5** Given the design principle, then the pulse question is framed conversationally — not as a survey or form.

---

### E2 — Team Lead Visibility & Sign-off

*The Team Lead never needs to schedule a check-in call. The dashboard tells them everything; the sign-off is the only action required of them.*

---

#### TL-1 — Joiner Progress Dashboard

**Story:** As a Team Lead, I can see all my active new joiners and their current progress so I can monitor ramp without scheduling a check-in.

**Acceptance Criteria:**

- **AC-TL-1.1** Given I am a Team Lead, when I log in, then I see a dashboard listing all joiners currently assigned to me, each showing: name, role track, current phase, % of current phase complete, and days remaining in ramp.
- **AC-TL-1.2** Given a joiner has blocked tasks, then a blocked indicator is surfaced on their dashboard row — I can see the reason without drilling into their checklist.
- **AC-TL-1.3** Given a joiner requires a milestone sign-off from me, then their row is visually highlighted and a sign-off action is immediately accessible.
- **AC-TL-1.4** Given I have no active joiners, then the dashboard shows a clear empty state — not a broken UI.
- **AC-TL-1.5** Given the dashboard loads, then it renders within 3 seconds for up to 20 active joiners.

---

#### TL-2 — Milestone Sign-off

**Story:** As a Team Lead, I can sign off on a milestone phase for a new joiner so I can formally advance their onboarding record.

**Acceptance Criteria:**

- **AC-TL-2.1** Given all tasks in a joiner's current phase are complete, then I receive a notification (email or Slack) that a sign-off is due.
- **AC-TL-2.2** Given a sign-off is due, then I can approve it directly from the dashboard — no navigation to a separate screen required.
- **AC-TL-2.3** Given I approve a sign-off, then the joiner's phase advances, the sign-off is recorded (my user ID, timestamp, phase), and the joiner is notified.
- **AC-TL-2.4** Given a sign-off has been completed, then it cannot be undone or edited by any user.
- **AC-TL-2.5** Given I have not signed off within 5 business days of the sign-off becoming due, then the Engineering Manager is notified and granted sign-off rights — I am also notified of the escalation.

---

#### TL-3 — Drill Into a Joiner's Checklist

**Story:** As a Team Lead, I can view a specific joiner's full checklist so I can understand the detail of their progress when needed.

**Acceptance Criteria:**

- **AC-TL-3.1** Given I click on a joiner from my dashboard, then I see their full checklist in read-only mode — all phases, all tasks, all statuses (including blocked reasons).
- **AC-TL-3.2** Given the joiner's checklist view, then I can see all pulse scores submitted at previous milestones.
- **AC-TL-3.3** Given the joiner's checklist view, then I cannot modify any task status — this view is read-only for Team Leads.

---

### E3 — Engineering Manager Escalation

*The EM is a safety valve — not a routine actor. Their surface is minimal: receive escalation, act once, done.*

---

#### EM-1 — Escalation Notification

**Story:** As an Engineering Manager, I am notified when a Team Lead has not signed off a milestone within 5 business days so I can act as the fallback approver.

**Acceptance Criteria:**

- **AC-EM-1.1** Given a milestone sign-off has been due for 5 business days without action, then I receive a notification via email or Slack identifying the joiner, the phase, the Team Lead who has not signed off, and a direct link to complete the sign-off.
- **AC-EM-1.2** Given the escalation notification, then it is clear that I am acting as fallback — the message frames this as an exception, not a routine action.
- **AC-EM-1.3** Given 5 business days are calculated, then weekends and public holidays configured in the system are excluded.

---

#### EM-2 — Fallback Sign-off

**Story:** As an Engineering Manager, I can complete a milestone sign-off on behalf of a Team Lead so that a joiner's onboarding record is not stalled.

**Acceptance Criteria:**

- **AC-EM-2.1** Given I have been granted escalation sign-off rights for a joiner, then I can approve the milestone from the notification link or from my dashboard.
- **AC-EM-2.2** Given I complete the sign-off, then the record shows my user ID and role (Engineering Manager) — it is clearly distinguished from a standard Team Lead sign-off.
- **AC-EM-2.3** Given I complete the sign-off, then the original Team Lead is notified that the escalation has been resolved.
- **AC-EM-2.4** Given the sign-off is complete, then the joiner's progress advances identically to a standard Team Lead sign-off — no joiner-facing difference.

---

### E4 — HR Template Management

*HR owns the content surface. No engineering ticket, no deployment, no coordination. The boundary is structural.*

---

#### HR-1 — Create and Edit Templates

**Story:** As an HR Admin, I can create and maintain checklist templates for each engineering role track so I control the onboarding content independently.

**Acceptance Criteria:**

- **AC-HR-1.1** Given I am an HR Admin, then I have access to a template management interface showing all 5 role tracks.
- **AC-HR-1.2** Given I open a template, then I can add, edit, reorder, and delete tasks. Each task has a title (required, max 150 characters), description (optional, max 1000 characters), and phase assignment (Week 1, Week 2, etc.).
- **AC-HR-1.3** Given I edit a template, then my changes are saved as a draft and do not affect any active or future joiners until I explicitly publish.
- **AC-HR-1.4** Given I delete a task from a template draft, then the deletion only takes effect for joiners assigned after the next publish — existing joiner assignments are unaffected.
- **AC-HR-1.5** Given the template editor, then I do not require any engineering involvement to make or publish changes.

---

#### HR-2 — Publish with Version Safety

**Story:** As an HR Admin, I can publish a template update knowing that joiners currently in onboarding will not be affected.

**Acceptance Criteria:**

- **AC-HR-2.1** Given I publish a template update, then a new version is created with a timestamp and my user ID recorded.
- **AC-HR-2.2** Given a joiner was assigned before the publish date, then their checklist reflects the template version active at their assignment date — they see no change.
- **AC-HR-2.3** Given a joiner is assigned after the publish date, then they receive the newly published template version.
- **AC-HR-2.4** Given I view the template management interface, then I can see which version is currently published and the history of prior versions with their publish dates.
- **AC-HR-2.5** Given I publish a template, then the system confirms the action and displays how many active joiners are unaffected (to provide confidence the change is safe).

---

#### HR-3 — View Active Joiner Template Versions

**Story:** As an HR Admin, I can see which template version each active joiner is on so I understand the state of in-flight onboarding.

**Acceptance Criteria:**

- **AC-HR-3.1** Given I view the HR admin dashboard, then I can see a list of all active joiners, their role track, and the template version they were assigned.
- **AC-HR-3.2** Given a joiner is on an older template version, then this is visually indicated — but it is not surfaced as an error or warning; it is expected and normal.

---

#### HR-4 — Custom Track: Team Lead Task Customisation

**Story:** As a Team Lead, I can add or remove tasks on a Custom track joiner's checklist so I can tailor the onboarding to their specific role.

**Acceptance Criteria:**

- **AC-HR-4.1** Given a joiner is assigned to the Custom role track, then I (their assigned Team Lead) can add tasks to their checklist after assignment.
- **AC-HR-4.2** Given I add a task, then I specify a title (required) and phase assignment. The task is immediately visible on the joiner's checklist.
- **AC-HR-4.3** Given I added a custom task, then I can remove it — but only if the joiner has not yet completed it.
- **AC-HR-4.4** Given the joiner's Custom checklist, then HR-authored base tasks and Team Lead-added custom tasks are visually distinguishable.
- **AC-HR-4.5** Given I am a Team Lead for a non-Custom track joiner, then I do not have task add/remove permissions — only HR can modify those templates.

---

### E5 — Authentication & Role-Based Access

*SSO is the only door. Roles are server-enforced. No exceptions.*

---

#### AU-1 — SSO Login

**Story:** As any user, I can log in using my organisation's SSO so I don't need a separate password.

**Acceptance Criteria:**

- **AC-AU-1.1** Given I navigate to the application, then I am redirected to the SSO login page (Google Workspace or Azure AD, per org configuration).
- **AC-AU-1.2** Given I successfully authenticate via SSO, then I am redirected to my role-appropriate landing view without additional login steps.
- **AC-AU-1.3** Given SSO authentication fails (e.g., account suspended), then I see a clear error message and am not granted access.
- **AC-AU-1.4** Given no SSO provider is available (network issue), then I see a service unavailable message — no fallback credential login is presented.
- **AC-AU-1.5** Given I am inactive for 8 hours, then my session expires and I am redirected to SSO login on next action.

---

#### AU-2 — Role-Based Access Enforcement

**Story:** As any user, I only see and can perform actions appropriate to my role so the system enforces access without relying on convention.

**Acceptance Criteria:**

- **AC-AU-2.1** Given I am a New Joiner, then I can only access my own checklist — I cannot access the Team Lead dashboard, HR template management, or any other joiner's data.
- **AC-AU-2.2** Given I am a Team Lead, then I can only access the dashboards and checklists of joiners assigned to me — I cannot access joiners assigned to other Team Leads.
- **AC-AU-2.3** Given I am an HR Admin, then I can access template management — I cannot access individual joiner progress records.
- **AC-AU-2.4** Given I am an Engineering Manager, then I can access all active joiners in my reporting chain — including escalation sign-off actions.
- **AC-AU-2.5** Given any user attempts to access a resource outside their role permissions via a direct URL or API call, then they receive a 403 Forbidden response — server-side enforcement, not client-side hiding.

---

### E6 — Data Integrity & Compliance

*Personnel data. Immutable audit trails. Soft-delete. This epic is a PRD requirement, not a nice-to-have.*

---

#### DC-1 — Immutable Sign-off Audit Trail

**Story:** As a system, milestone sign-off records are immutable and timestamped so the audit trail cannot be altered retroactively.

**Acceptance Criteria:**

- **AC-DC-1.1** Given a sign-off is recorded, then the record contains: joiner ID, phase name, timestamp (UTC), signing user ID, signing user role, and pulse score (if applicable).
- **AC-DC-1.2** Given a sign-off record exists, then no user — including HR Admins and Engineering Managers — can edit or delete it via the application interface.
- **AC-DC-1.3** Given the database schema, then the sign-off table grants insert-only permissions to the application database role — UPDATE and DELETE are not permitted.
- **AC-DC-1.4** Given an escalation event occurs, then a separate escalation record is created: milestone due date, escalation trigger timestamp, notified EM user ID, and resolution timestamp if signed off.

---

#### DC-2 — Data Retention & Soft Delete

**Story:** As a system administrator, I can configure how long onboarding records are retained so the organisation can meet its data obligations.

**Acceptance Criteria:**

- **AC-DC-2.1** Given an onboarding record is deleted (e.g., an employee leaves), then it is soft-deleted — a tombstone record is retained with deletion timestamp and acting user ID.
- **AC-DC-2.2** Given the configured retention period expires for a soft-deleted record, then a scheduled purge job permanently removes the record and logs the purge action.
- **AC-DC-2.3** Given the default configuration, then the retention period is 3 years. This MUST be configurable by a system admin without a code deployment.
- **AC-DC-2.4** Given a record is soft-deleted, then it is no longer visible in any application UI — it only exists in the database and system logs.

---

#### DC-3 — Data Export for Right-of-Access Requests

**Story:** As a system administrator, I can export a complete record of a joiner's onboarding data so the organisation can respond to GDPR/CCPA right-of-access requests.

**Acceptance Criteria:**

- **AC-DC-3.1** Given a right-of-access request, then a system admin can trigger a per-joiner data export from the admin interface.
- **AC-DC-3.2** Given the export is generated, then it includes: all task records (title, status, completion timestamp if applicable, blocked reason if applicable), all milestone sign-off records, all pulse scores, all escalation events, and all notification delivery records for that joiner.
- **AC-DC-3.3** Given the export, then it is produced in JSON or CSV format, clearly labelled with the joiner ID and export timestamp.
- **AC-DC-3.4** Given the export is generated, then the action is logged in the audit log (admin user ID, timestamp, joiner ID).
- **AC-DC-3.5** Given the requirement, then the export MUST be producible within 24 hours of the request — it MUST NOT require a custom database query or engineering involvement.

---

## Assumptions & Constraints

| # | Type | Statement |
|---|---|---|
| A1 | Assumption | All users have valid SSO accounts in Google Workspace or Azure AD before accessing the system. Provisioning of SSO accounts is handled externally. |
| A2 | Assumption | The engineering + HR joint content session (pre-launch gate) will be completed at least 1 week before the first joiner is onboarded. |
| A3 | Assumption | The org has an existing Slack workspace with a bot token available for Slack notifications. |
| A4 | Assumption | A public holiday calendar for the relevant jurisdiction will be provided before deployment to support business-day escalation calculations. |
| C1 | Constraint | The system must be self-hosted behind the corporate firewall. No third-party SaaS data processors may handle personnel data. |
| C2 | Constraint | No managed services (AWS SQS, Azure Service Bus, etc.) are available — the notification and scheduling pipeline must run on self-managed infrastructure. |
| C3 | Constraint | Mobile experience is explicitly out of scope for v1. Desktop web browsers only (latest 2 versions of Chrome, Firefox, Safari). |
| C4 | Constraint | GitHub, Jira, and other external system integrations are explicitly out of scope for v1. |
| C5 | Constraint | The system must be deployable by a single engineer — no dedicated platform team available for operations. |

---

## Open Questions

| # | Question | Owner | Target Resolution |
|---|---|---|---|
| OQ-1 | Which SSO provider (Google Workspace or Azure AD) will be configured first? Dual-provider support in v1 adds integration complexity. | Meher / IT | Before architecture kickoff |
| OQ-2 | What is the org's public holiday calendar for business-day escalation calculations? | HR | Before scheduler design |
| OQ-3 | What is the Slack bot permission scope required for DM delivery? Does the org already have a bot configured? | IT / Engineering | Before notification design |
| OQ-4 | Who is the system admin role — the person who can trigger data exports and configure retention periods? Is this HR, IT, or Engineering? | Meher | Before Epic 6 implementation |
| OQ-5 | For the Custom role track: can multiple Team Leads add tasks to the same joiner's checklist (e.g., if the joiner has two mentors), or is task customisation restricted to the single assigned Team Lead? | Meher | Before E4 / HR-4 implementation |
