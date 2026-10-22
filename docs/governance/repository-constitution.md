# TRAIBOX Repository Constitution

- Status: **CANONICAL** for repository governance
- Status semantics: **intended post-approval**
- Decision: **C0.5** (revises C0.1)
- Owner: **@jeszymczuk**
- Effective when: **PR #42 is approved and merged into `main` by the repository owner**
- Founder approval record: **the merge of PR #42**
- Draft branch authority: **none over `main`**

`AGENTS.md` is the short form of this constitution. If the two ever disagree, stop and report the exact sections.

## 1. Purpose and authority

This constitution sets the engineering, security, data and development baseline for this repository and says where product direction comes from. C0.1 (PR #38, July 2026) ranked twelve Blueprint chapters above the code and made their absence a reason to stop. None of those chapters was ever imported, and the founder has since reopened the TRAIBOX concept itself, so C0.5 replaces that ranking.

TRAIBOX is being redefined. The concept (Blueprint Ch.0) is under revision, the Blueprint is being revised and completed, and the next goal is a well-defined, workable product that people can start using and pay for. Product decisions are therefore open unless `docs/governance/decision-ledger.md` lists them as locked. The engineering safety rules in sections 4 to 8 are locked regardless of the concept.

The order of authority is:

1. the founder's current brief for the task (issue, PR description, or session instruction); the latest explicit founder statement wins;
2. `docs/governance/decision-ledger.md` — the decisions that are actually locked;
3. the revised Ch.0 concept, once approved and imported (`PENDING_IMPORT`);
4. the revised and completed Blueprint, once approved and imported (`PENDING_IMPORT`);
5. approved engineering ADRs;
6. executable repository contracts, migrations, and tests (implementation evidence, never product direction);
7. Blueprint v6.1 and v7.0 chapters and other non-authoritative material (context and vocabulary only).

When a product decision is open or a written source is missing, work continues: make the smallest reasonable choice that keeps options open, mark it `PROVISIONAL:` in code and in the pull request, and list it under open product decisions. Do not fabricate product meaning or present a reconstruction as authoritative.

## 2. Document lifecycle

Only these uppercase statuses are valid:

- `CANONICAL`: controlling source at its declared scope.
- `APPROVED`: accepted implementation authority at its declared scope.
- `REVIEW`: proposal or evidence awaiting approval; not implementation authority.
- `SUPERSEDED`: replaced historical material; context only.
- `ARCHIVED`: retained historical material; context only.
- `PENDING_IMPORT`: known source not yet available in the repository; its absence does not block work.

`REVIEW`, `SUPERSEDED`, `ARCHIVED`, and `PENDING_IMPORT` material governs only when an approved decision or the founder's current brief activates a specific, named section. Repository contracts and tests reveal implementation reality but never lock product decisions.

## 3. Product structure (working defaults)

### 3.1 Workspaces

The current navigation has seven customer workspaces: Intelligence, Trades, Finance, Network, Clearance, Operations Center, and Settings. Payments sits inside Finance; Inbox is not a top-level workspace; partner and financier experiences use separate role-specific shells, permissions, and information architecture.

This structure is the working default, not a locked product decision. It may change with the revised concept. Do not add, remove, or rename top-level workspaces unless the founder's brief says so.

### 3.2 Routes and screens

Every production route is registered in `route-manifest.yaml`, which CI checks for exact coverage. A route declares its workspace, audience, role and tenant behavior, and implements loading, empty, error, permission-denied, degraded, accessibility, and responsive behavior. Link a screen contract when one exists; while the concept is being redefined, a `missing` screen contract is acceptable when the pull request describes the intended behavior.

### 3.3 Visual design

Design System v2 tokens and shared primitives are the working implementation (ADR-006, superseding ADR-005). UI work composes semantic tokens rather than hard-coded values, and the component-token audit stays in CI. The visual language itself is open and changes through the founder's design brief by editing the tokens, without a superseding ADR.

## 4. Pilot system architecture

The approved current architecture is:

- Next.js web application, currently deployed on Vercel;
- independently deployed Fastify TypeScript application API on Fly.io;
- independently deployed background worker on Fly.io;
- Supabase-managed PostgreSQL;
- private Supabase Storage;
- Supabase Auth for the pilot;
- Python/FastAPI Trade Brain intelligence plane;
- the current worker for non-durable background work.

The independently deployed TypeScript application API is the canonical TRAIBOX application backend. Canonical Trade, Finance, Payment, Clearance, Approval, instrument, escrow, and agent-related mutations pass through its authorization, validation, policy, idempotency, independent domain validation, and audit controls.

Supabase is approved managed infrastructure for PostgreSQL, private object storage, and authentication. Supabase PostgREST, Edge Functions, frontend business logic, database triggers containing product workflow logic, and direct browser-to-database mutations are not the canonical backend.

Fastify is approved as the pilot modular monolith by ADR-001. NestJS is a reviewed medium-term target, not an immediate migration. PostgreSQL is canonical transactional truth. ADR-002 governs the PostgreSQL UTG adapter and any later derived Neo4j layer. ADR-003 governs durable workflow adoption. The Trade Brain is an intelligence plane and does not own canonical state.

## 5. Authenticated browser boundary

ADR-004 defines the boundary: a thin Next.js BFF with secure, server-managed sessions.

The BFF may manage sessions, attach credentials, correlate requests, proxy SSE, normalize transport, and safely normalize errors. It may not contain domain decisions, Finance calculations, approval decisions, canonical state ownership, protected-action logic, agent authorization decisions, or provider-specific business policy.

Sensitive credentials, access tokens, refresh tokens, and private trade transcripts must not be stored in `localStorage`, `sessionStorage`, or IndexedDB. Authentication tokens must not appear in URL query strings except for an explicitly approved, short-lived, single-purpose signed artifact URL. `docs/governance/browser-security-boundary.md` records the C0.4 remediation of the browser-storage and query-token findings originally listed under C0.1.

## 6. Agents and protected actions

A model or agent may analyse, assess, explain, recommend, draft, prepare evidence, and propose an action. It may not directly execute a protected action or directly mutate canonical Trade, Finance, Payment, Clearance, Approval, instrument, or escrow state.

Canonical execution requires typed validation, authorization, policy evaluation, idempotency where required, independent domain validation, human approval where required, audit evidence, and replayability. Reasoning and workflow execution remain separate architectural responsibilities.

Only identifiers that actually exist in the shared contract are recorded in `protected-actions.yaml`. That manifest is an evidence map, not permission by itself. Its discrepancies are release risks to remediate in scoped follow-up work.

## 7. Data and migrations

PostgreSQL is the canonical transactional source of truth. Every tenant-owned table requires explicit ownership, RLS, tenant-isolation tests, audit consideration, and retention consideration.

Migrations are additive by default. A destructive or irreversible migration requires an approved ADR, explicit migration approval, backup/recovery analysis, compatibility analysis, and rollback or forward-fix analysis. Database triggers may enforce integrity and audit invariants; they may not contain product workflow decisions.

## 8. Providers and customer-facing abstraction

Provider implementations remain behind provider-neutral ports and adapters. External providers adapt to TRAIBOX contracts; TRAIBOX contracts are not defined by provider schemas. Customer UI must not hardcode model-provider names, model identifiers, cloud-provider details, or internal infrastructure implementation details.

## 9. Development governance

An engineering architecture deviation requires an ADR; product and design decisions do not, and the locked ones are recorded in the decision ledger. One implementation agent authors a workstream; another may review it. Claude Code and Codex must not independently implement the same workstream in parallel.

Stubs, mocks and scaffolding are labelled as such in code, ADRs, and user-facing copy; no hollow capability claims. Pull requests identify the brief or ledger entries they follow, architecture, routes, protected actions, security/privacy, migrations/RLS, providers, validation, compatibility, rollback, open product decisions, and anything marked `PROVISIONAL:`.

Contributors stop and report when an action is destructive or irreversible, a protected-action, security or tenant-isolation boundary cannot be kept, unrelated work risks loss, permissions block safe progress, or two founder instructions directly contradict each other. An open product decision or a missing Blueprint chapter is not a reason to stop.

## 10. Known implementation drift (evidence, not authority)

Recorded so it is not mistaken for intended design:

1. The left navigation on `main` exposes Payments and Inbox as top-level items and mixes internal surfaces (My Space, Internal Alpha, UI Demo, Partner Portal) with customer workspaces. A fix exists as a local, unpushed commit (`fix(web): conform left navigation`) and has not reached `main`.
2. `/operations` and `/operations-center` are separate implemented screens.
3. Legacy `/intelligence/workspace`, `/finance/workspace`, `/network/workspace`, and `/clearance/workspace` routes remain implemented without redirects.
4. The shared contract declares more protected actions than the API catalog annotates; proof share requests exist in Fastify but are absent from the executable endpoint catalog.
5. The Ch.17.A implementation copy in `docs/frontend/` calls itself active; it is `REVIEW` and is context only.

Items closed by earlier governed work (C0.3 protected-action repair and C0.4 browser boundary) are recorded in their pull requests.

## 11. Pull-request and validation gate

Use `.github/pull_request_template.md`. Run applicable YAML parsing, formatting, conformance, lint, typecheck, tests, Trade Brain tests, Trade Brain evaluation, and build checks without production credentials. Report all failures and classify them as introduced, pre-existing, credential-related, tooling-related, or unrelated.

PR #26 is superseded historical context and not authority. PR #30 is an unmerged source branch and must not be merged or copied wholesale.

## 12. Conformance boundary

Structural conformance (manifest schemas, exact route and protected-action coverage, ADR registration, API catalog/server alignment, status-vocabulary drift, lint tooling, design-token usage, browser security, release integrity) runs in CI and must pass.

Semantic conformance (final route ownership and redirects, screen and component contracts, interaction semantics, final workspace behavior, product-meaning validations) is deferred until the revised Ch.0 and Blueprint are approved and imported. Deferral never blocks product work, and conformance checks must not promote observed behavior into product authority.
