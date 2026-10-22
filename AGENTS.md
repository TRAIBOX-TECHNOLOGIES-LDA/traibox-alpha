# TRAIBOX Repository Constitution

This file is the repository-wide instruction baseline for human and automated contributors (Claude Code, Codex, and people). Read it before changing any file. The longer-form version is `docs/governance/repository-constitution.md`; the machine-readable maps are in `docs/governance/`.

## 0. Where TRAIBOX is right now

TRAIBOX is being redefined. The concept (Blueprint Ch.0) is under revision, the Blueprint is being revised and completed, and the next goal is a well-defined, workable product that people can start using and pay for. That has consequences for everyone working here:

- **Nothing at product level is locked** unless it is in `docs/governance/decision-ledger.md`. Positioning, category, wedge, customer, workspace structure, screens, flows, visual language and the demo are all open.
- **Past material is context, not commitment.** Notion Blueprint v6.1/v7.0 chapters, old branches, PR #26/#30, repository docs marked `REVIEW`, and earlier chat output are explorations. Use them for ideas and vocabulary; do not treat them as requirements, and do not defend them against a newer founder instruction.
- **TRAIBOX is not a compliance product, a payments product or a finance product.** Those are capabilities it may contain. Do not frame code, copy, naming or UI around any one of them as the product's identity.
- **The current demo and `/alpha` story will be replaced** once Ch.0 is resolved. Do not invest in polishing them unless the task asks for it.
- **The engineering safety rules in §2–§5 are locked regardless.** They protect customers' data, money and trust whatever the product becomes.

## 1. Where direction comes from

1. **The founder's current brief** for the task (issue, PR description, or session instruction) is the product authority. The latest explicit founder statement wins over older documents.
2. **`docs/governance/decision-ledger.md`** lists the decisions that are actually locked. If a decision is not there, it is open.
3. **Approved ADRs** (`docs/adrs/`, registered as `APPROVED` in `docs/governance/source-of-truth.yaml`) govern engineering boundaries.
4. **Code, contracts, migrations and tests** show what exists today. They are evidence, not product direction.
5. **Everything else** (Notion chapters, `REVIEW`/`SUPERSEDED`/`ARCHIVED`/`PENDING_IMPORT` material) is context.

When a product decision is open, **do not stop**. Make the smallest reasonable choice that keeps options open, mark it in code and in the PR with `PROVISIONAL:`, and list it under "Open product decisions" in the PR. Do not invent product meaning and present it as decided, and do not reconstruct a missing Blueprint chapter as if it were authoritative. Stop only for the conditions in §10.

## 2. Architectural boundaries (approved)

- The canonical backend is the independently deployed TypeScript application API. Fastify is the approved pilot modular-monolith implementation under ADR-001; NestJS is a reviewed medium-term option, not a current migration.
- Supabase is approved pilot managed infrastructure for PostgreSQL, private object storage, and authentication. PostgREST, Edge Functions, browser business logic, database workflow triggers, and direct browser-to-database mutations are not the canonical TRAIBOX backend.
- PostgreSQL is canonical transactional truth. The PostgreSQL UTG implementation is an adapter behind a stable service contract; any later Neo4j graph is derived and non-canonical under ADR-002.
- The Python/FastAPI Trade Brain is the intelligence plane. It may analyse, assess, explain, recommend, draft, and prepare evidence; it does not own canonical product state.
- The current worker is approved only for non-durable background work. Apply ADR-003 before introducing long-lived or protected multi-step workflow execution.
- Provider implementations stay behind provider-neutral ports and adapters. External schemas do not define TRAIBOX contracts.

## 3. Security and session boundary

Follow ADR-004. The authenticated browser boundary is a thin Next.js BFF with secure server-managed sessions. The BFF may manage sessions, attach credentials, correlate requests, proxy SSE, normalize transport, and safely normalize errors. It may not own domain decisions, Finance calculations, approval decisions, canonical state, protected-action logic, agent authorization, or provider-specific business policy.

Never store credentials, access or refresh tokens, or private trade transcripts in `localStorage`, `sessionStorage`, or IndexedDB. Never place an authentication token in a query string except for an explicitly approved, short-lived, single-purpose signed artifact URL.

## 4. Agents and protected actions

Use only protected-action identifiers present in `docs/governance/protected-actions.yaml` and the shared contract. A model or agent may never directly execute a protected action or mutate canonical Trade, Finance, Payment, Clearance, Approval, instrument, or escrow state.

Canonical execution requires typed validation, authorization, policy evaluation, idempotency where required, independent domain validation, human approval where required, audit evidence, and replayability. Agent reasoning and business workflow execution are separate responsibilities.

## 5. Data, migrations, and RLS

- Every tenant-owned table needs explicit ownership, enabled and forced RLS where applicable, tenant-isolation tests, audit consideration, and retention consideration.
- Migrations are additive by default.
- A destructive or irreversible migration requires an approved ADR, explicit migration approval, backup/recovery analysis, compatibility analysis, and rollback or forward-fix analysis.
- Never put product workflow decisions in database triggers.

## 6. Routes, screens, and visual design

- Register every production route in `docs/governance/route-manifest.yaml` (CI checks exact coverage). Link a screen contract when one exists; while the concept is being redefined, `missing` is acceptable as long as the PR describes the intended behavior.
- Every screen implements loading, empty, error, permission-denied, and degraded states, and is accessible and responsive.
- The seven-workspace navigation (Intelligence, Trades, Finance, Network, Clearance, Operations Center, Settings) is the **working default**, not a locked product decision. Do not add, remove, or rename top-level workspaces unless the founder's brief says so.
- Design System v2 tokens are the **working implementation** (ADR-006). Always compose semantic tokens and shared primitives instead of hard-coded colors, spacing or type. The visual language itself is open: changes come from the founder/design brief and are made by editing the tokens, not by a superseding ADR.
- Customer-facing UI never exposes provider names, model identifiers, cloud details, or internal infrastructure.

## 7. When an ADR is required

Create or supersede an ADR for an architecture deviation, a backend/framework boundary change, a new canonical data store, a durable workflow engine, a new authenticated browser/session pattern, a provider abstraction breach, or a destructive/irreversible migration. Add approved ADRs to `docs/adrs/README.md` and `docs/governance/source-of-truth.yaml`. Product and design decisions do not need ADRs; record locked ones in `docs/governance/decision-ledger.md`.

## 8. Workstream, honesty, and pull-request discipline

- One implementation agent authors a workstream. A different agent may review it. Claude Code and Codex must not implement the same workstream independently in parallel.
- Preserve unrelated work. Do not clean, reset, stash, overwrite, or commit it without explicit authorization.
- Label stubs, mocks and scaffolding as such in code, ADRs, and user-facing copy. No hollow capability claims.
- Complete `.github/pull_request_template.md`, including open product decisions and anything marked `PROVISIONAL:`.
- Do not claim a check passed unless it was run in the current worktree. Run the feature and look at it in the browser before calling UI work done.

## 9. Validation baseline

Run the checks applicable to the change without production credentials:

```bash
pnpm conformance
pnpm lint
pnpm typecheck
pnpm test
pnpm test:trade-brain
pnpm eval:trade-brain
pnpm build
```

For YAML changes, parse every changed YAML file. For migrations, also run the migration dry-run and tenant-isolation coverage appropriate to the change. Classify every failure as introduced, pre-existing, credential-related, tooling-related, or unrelated.

## 10. Stop conditions

Stop and report when: an action is destructive or irreversible; a protected-action, security, or tenant-isolation boundary cannot be kept; unrelated work risks loss; permissions block safe progress; or two founder instructions directly contradict each other.

An open product decision or a missing Blueprint chapter is **not** a stop condition — see §1.

## 11. Working with the founder

- Bring a real recommendation and honest pushback. Do not just ratify, and do not offer options without saying which one you would pick.
- Build. Do not hand work off or wait on documents; follow §1 when something is open.
- Do not anchor on past positions. The latest instruction from the founder wins over older Notion chapters, docs, branches or chat history.
- Speak plainly. The founder is not an engineer: explain what changed and why in product terms, and keep command-level detail in the pull request.
