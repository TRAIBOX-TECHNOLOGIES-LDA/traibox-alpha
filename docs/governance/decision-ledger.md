# TRAIBOX Decision Ledger

- Status: **CANONICAL** for which decisions are locked
- Owner: **@jeszymczuk** (Juan, founder and CEO)
- Last reviewed: **2026-10-22**

This is the short list of decisions that are actually locked. If a decision is not listed here, it is open, however settled it looks in Notion, an old branch, a repository document or a past chat. Only the founder adds, changes or removes entries, and every change lands through a pull request he merges.

## Product

| ID   | Decision                                                                                                                                                                                                                         | Since      |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| P-01 | The concept (Blueprint Ch.0) is being redefined. Until the revised Ch.0 is imported, no positioning, category, wedge, customer, workspace structure, screen set, flow or visual language is locked.                         | 2026-10-22 |
| P-02 | TRAIBOX is not a compliance product, a payments product or a finance product. Compliance, payments, finance and similar areas are capabilities TRAIBOX may contain; exploring or building one does not define the product. | 2026-10-22 |
| P-03 | There is no decided wedge or first customer segment. Earlier wedges (including the New Buyer Loop and the Port of Aveiro pilot) were experiments.                                                                         | 2026-10-22 |
| P-04 | The current demo will be replaced by a new demo built from scratch after the revised Ch.0 is resolved.                                                                                                                     | 2026-10-22 |
| P-05 | Next product goal: a well-defined, workable product that people can start using and pay for, built from the revised concept and the revised Blueprint.                                                                     | 2026-10-22 |
| P-06 | TRAIBOX is global by design. Countries, corridors, regions, currencies, languages, payment schemes and jurisdictions are configuration, never product scope. Any market order (for example Portugal, Spain, Germany, the EU) is a rollout step in a go-to-market plan, not a boundary of the product. | 2026-10-22 |
| P-07 | Personas and scenarios (including Maria and the TRX-00455 trade) are illustrative fiction. They explain and test the product; they are never a source of truth, a default, or a governance rule. | 2026-10-22 |
| P-08 | Compliance is one capability made of rules (sanctions, export controls, customs, CBAM and others). Each rule is versioned data with explicit jurisdiction and trade conditions inside a rule pack; no single rule is hard-coded, defaulted on, or treated as a headline feature. | 2026-10-22 |

## Engineering

Engineering decisions are recorded as ADRs and registered in `docs/governance/source-of-truth.yaml`:

- ADR-001 Fastify pilot modular monolith
- ADR-002 PostgreSQL UTG pilot adapter
- ADR-003 Durable workflow adoption triggers
- ADR-004 Next.js BFF and session boundary
- ADR-006 Design System v2 as working implementation; visual language open

The safety rules in `AGENTS.md` §2–§5 (backend boundary, session security, protected actions, tenant isolation and migrations) are locked independently of the product concept.

## Working defaults (not locked)

These describe the current implementation so work can continue. They may change with the revised concept without any ADR:

- Seven-workspace navigation: Intelligence, Trades, Finance, Network, Clearance, Operations Center, Settings.
- Design System v2 tokens and themes.
- Blueprint v7.0 vocabulary (Trade Room, readiness verdicts, Trade Passport, Trade Brain pillars).
