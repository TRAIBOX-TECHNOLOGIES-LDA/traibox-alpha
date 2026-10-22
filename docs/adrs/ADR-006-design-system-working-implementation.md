# ADR-006: Design System v2 as working implementation; visual language open

- Status: Accepted
- Status semantics: intended post-approval
- Date: 2026-10-22
- Owners: @jeszymczuk
- Supersedes: ADR-005
- Effective when: PR #42 is approved and merged into `main` by the repository owner
- Approval record: the merge of PR #42
- Draft branch authority: none over `main`

## Context

ADR-005 locked Design System v2 as the visual foundation and required a superseding ADR, with user evidence and a migration plan, for any competing redesign. Since then the founder has confirmed that nothing in the TRAIBOX design is locked: the brand mark, palette, component rules and visual signature are still being explored, and the concept itself (Blueprint Ch.0) is being redefined. A later design direction (the "Payments standard, applied everywhere" proposal of 28 July 2026) already departs from ADR-005's anti-chip and anti-eyebrow rules. Keeping ADR-005 in force would make every design iteration a governance exercise.

## Decision

- Design System v2 tokens, themes and shared primitives remain the **working implementation**. They describe what the product looks like today.
- The **visual language is open**. Palette, typography, density, surface treatment, iconography, component styling and the brand mark may change through the founder's design brief without a superseding ADR.
- Visual changes are made by **editing the semantic tokens and shared primitives**, not by bypassing them.

## Hard constraints (kept)

- Compose semantic tokens; do not hand-pick duplicate colors, spacing or type where a token exists. The component-token audit stays in CI.
- Accessibility, focus, reduced motion, contrast, responsive behavior, and all required screen states are part of every UI change.
- Customer-facing UI does not expose provider, model, cloud, or internal infrastructure details.

## Reconsideration triggers

Lock a visual language again (in `docs/governance/decision-ledger.md`) when the founder approves a final design system, or when two or more customer-facing surfaces ship to real users and visual churn would break their experience.

## Compatibility and rollback

No runtime change. Existing tokens, themes and audits stay as they are. Rolling back means restoring ADR-005 to Accepted and marking this ADR Superseded.

## Affected governance manifests

- `docs/adrs/README.md`
- `docs/governance/source-of-truth.yaml`
- `docs/governance/decision-ledger.md`
