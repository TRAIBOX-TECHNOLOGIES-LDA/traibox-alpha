# TRAIBOX Architecture Decision Records

This directory contains the engineering architecture decisions for TRAIBOX. `docs/governance/source-of-truth.yaml` records their authority and activation condition. Product and design decisions are not ADRs; the locked ones live in `docs/governance/decision-ledger.md`.

## Activation

ADR-001 through ADR-005 were activated by the merge of PR #38 (C0.1). ADR-006 and the supersession of ADR-005 take effect when PR #42 (C0.5) is approved and merged into `main` by the repository owner. An unmerged draft branch has no authority over `main`.

## Status vocabulary

- `Proposed`: under review; not implementation authority.
- `Accepted`: active once its recorded activation condition is satisfied.
- `Superseded`: replaced by a named later ADR.
- `Deprecated`: retained temporarily with an explicit removal path.

After activation, changing an Accepted decision requires a superseding ADR; editing history in place is prohibited except for status/supersession metadata, typo or link repair that does not change meaning.

## Accepted decisions

| ADR                                                              | Decision                                                      | Status   |
| ---------------------------------------------------------------- | ------------------------------------------------------------- | -------- |
| [ADR-001](ADR-001-fastify-pilot-modular-monolith.md)             | Fastify pilot modular monolith                                | Accepted |
| [ADR-002](ADR-002-postgres-utg-pilot-adapter.md)                 | PostgreSQL UTG pilot adapter                                  | Accepted |
| [ADR-003](ADR-003-durable-workflow-adoption-trigger.md)          | Durable workflow adoption triggers                            | Accepted |
| [ADR-004](ADR-004-nextjs-bff-session-boundary.md)                | Next.js BFF and session boundary                              | Accepted |
| [ADR-006](ADR-006-design-system-working-implementation.md)       | Design System v2 as working implementation; visual language open | Accepted |

## Superseded decisions

| ADR                                    | Decision                    | Superseded by |
| -------------------------------------- | --------------------------- | ------------- |
| [ADR-005](ADR-005-design-system-v2.md) | Design System v2 foundation | ADR-006       |

## ADR requirement

Create or supersede an ADR before changing a canonical backend or framework boundary, canonical data store, authenticated browser/session boundary, durable workflow substrate, provider-abstraction rule, or destructive/irreversible migration strategy. Visual-language changes do not need an ADR (see ADR-006).

Each ADR must state context, decision, hard constraints, measurable reconsideration triggers, compatibility/rollback consequences, and affected governance manifests.
