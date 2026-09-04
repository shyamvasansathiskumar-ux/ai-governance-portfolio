# AI Risk Assessment — [System name]

**Assessment of:** [product / feature]
**Assessed by:** Shyam Kumar
**Date:** YYYY-MM-DD
**Framework:** NIST AI RMF 1.0
**Version:** 0.1 draft

> **Scope and standing.** This is an independent, external assessment based entirely on publicly available documentation and observable system behaviour. It was not commissioned by, reviewed by, or conducted with the cooperation of [vendor]. It is not an audit, a certification, or a compliance opinion, and should not be read as one.

---

## 1. Executive summary

*Three to five sentences, written for someone who will read nothing else.* What the system is, the most significant risk identified, and the single recommendation that matters most. Write this section last, but keep it first.

**Overall risk rating:** Low / Medium / High / Critical
**Primary concern:**
**Headline recommendation:**

---

## 2. System description

| Field | Detail |
|---|---|
| System / feature | |
| Provider | |
| Purpose | What it's for, in the provider's own framing |
| Deployment context | Consumer / enterprise / embedded / API |
| Model type | e.g. general-purpose LLM, fine-tuned, multimodal |
| Users | Who interacts with it |
| Affected parties | Who is impacted but doesn't choose to use it — often the group that matters most |
| Autonomy level | Advisory / human-in-the-loop / semi-autonomous / autonomous |
| Data handled | Categories, including any sensitive or special-category data |
| Evidence base | Links to the public documentation this assessment relies on |

**Why this system was selected for assessment:**

---

## 3. Regulatory and framework context

| Framework | Applicability | Notes |
|---|---|---|
| EU AI Act | Prohibited / High-risk / Limited (transparency) / Minimal — *and the reasoning for that tier* | |
| NIST AI RMF | Voluntary, used here as the assessment structure | |
| Sector-specific (GDPR, HIPAA, DORA, sectoral regulators…) | | |

*Be explicit about uncertainty.* If a risk tier is genuinely arguable, say so and give both readings — a confident wrong classification is worse than a well-reasoned "this is contested, here's why."

---

## 4. Trustworthiness characteristics

NIST AI RMF names seven characteristics of trustworthy AI. Assess each; "not applicable" is a legitimate answer if justified.

| Characteristic | Assessment | Evidence | Concern level |
|---|---|---|---|
| Valid and reliable | | | |
| Safe | | | |
| Secure and resilient | | | |
| Accountable and transparent | | | |
| Explainable and interpretable | | | |
| Privacy-enhanced | | | |
| Fair, with harmful bias managed | | | |

---

## 5. Risk register

One row per identified risk. Keep it to risks you can actually evidence.

| ID | Risk | Affected party | Likelihood | Impact | Rating | OWASP LLM class | ATLAS technique |
|---|---|---|---|---|---|---|---|
| R1 | | | L/M/H | L/M/H | | | |
| R2 | | | | | | | |
| R3 | | | | | | | |

### R1 — [risk name]

**Description.** What could go wrong, mechanically.

**How it could occur.** The realistic path, not the theoretical one.

**Who is harmed, and how.**

**Evidence.** What you observed or what the documentation says. If this is inferred rather than observed, say so plainly.

**Existing mitigations.** What the provider already does, as far as is publicly visible.

**Residual risk.** What remains after those mitigations.

**Recommendation.**

*(Repeat for each risk.)*

---

## 6. NIST AI RMF function mapping

Map findings onto the four core functions. GOVERN is cross-cutting — it applies across the other three rather than sitting in sequence with them.

### GOVERN — culture, policy, accountability, oversight
*Is there a named owner? A documented policy? A route for affected people to contest an outcome?*

### MAP — context, purpose, and where risk originates
*Is the intended use clearly bounded? Are foreseeable misuses acknowledged?*

### MEASURE — analysis, testing, tracking
*How is performance evaluated? Is testing adversarial as well as functional? Are results published?*

### MANAGE — prioritisation, response, monitoring
*How are identified risks prioritised and acted on? What happens when something goes wrong in production?*

---

## 7. Recommendations

Ordered by priority. Each one names an owner-type and a horizon — a recommendation with neither is a wish.

| # | Recommendation | Addresses | Owner type | Horizon |
|---|---|---|---|---|
| 1 | | R1 | e.g. Product / Security / Legal | Immediate / 90 days / Next cycle |
| 2 | | | | |

---

## 8. Limitations

State these honestly and specifically — this section is what separates a credible assessment from an overreaching one, and reviewers read it closely.

- Based solely on public information; no access to training data, evaluation results, or internal controls.
- No privileged testing was performed against production systems.
- Assessment reflects the system as observed on [date]; these systems change frequently.
- [Any other constraint that genuinely applies]

---

## 9. Sources

1.
2.
