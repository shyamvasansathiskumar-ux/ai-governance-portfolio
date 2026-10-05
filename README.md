# ai-governance-portfolio

Written work on AI governance, risk and assurance: risk assessments, policy artefacts and published analyses.

**Shyam Kumar** · CSE, Rajalakshmi Engineering College · started August 2026

This is deliberately separate from [`redteam-log`](https://github.com/shyamvasansathiskumar-ux/redteam-log). That repo is a lab notebook; this one is finished work meant to be read. Governance hiring rarely looks at GitHub at all, so this repo exists so that when someone *does* look, they find writing rather than scan dumps.

## Start here

**[AI coding agents with access to production systems: a NIST AI RMF assessment](analyses/2026-10-ai-coding-agent-production-access.md)** (October 2026)

Six public incidents in fifteen months, from five vendors, in which coding agents destroyed data with no attacker involved. The assessment rates the pattern High, maps seven risks to the RMF, and makes eight recommendations. It also covers what India's DPDP Rules mean when the deletion is accidental: no attacker does not mean no breach.

## Layout

| Folder | What goes here |
|---|---|
| `analyses/` | Finished, published pieces, the ones that also go on LinkedIn and Substack |
| `templates/` | Reusable artefacts: risk assessment, one-page policy |
| `reference/` | Framework notes and cheat sheets built while reading |

## Standard for anything in `analyses/`

1. **Narrow beats broad.** "How [specific product]'s [specific feature] maps to EU AI Act transparency obligations" is a piece. "AI governance is important" is not.
2. **Every claim is sourced.** Link the framework clause, the regulation article, the vendor documentation. Unsourced assertion is the fastest way to lose a technical reader.
3. **Say what you'd do.** Analysis that stops at "this is a risk" is half-finished. Name the control.
4. **Assessments of real products are external, black-box and clearly labelled as such.** They are based on public documentation and observable behaviour, not privileged access, and are never presented as an official or commissioned audit of a company.

## Index

| Date | Piece | Type | Published |
|---|---|---|---|
| 2026-10-05 | [AI coding agents with access to production systems](analyses/2026-10-ai-coding-agent-production-access.md) | Risk assessment (NIST AI RMF) | GitHub |
| 2026-10-05 | [The AI deleted the database. Nobody attacked it.](analyses/2026-10-no-adversary-essay.md) | Essay | GitHub, Substack, LinkedIn |

## Related

- [`ai-incident-atlas`](https://github.com/shyamvasansathiskumar-ux/ai-incident-atlas): the incident evidence behind the assessment above
- [`redteam-log`](https://github.com/shyamvasansathiskumar-ux/redteam-log): OWASP LLM Top 10 to MITRE ATLAS mapping, all ten classes
