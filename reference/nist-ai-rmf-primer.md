# NIST AI RMF 1.0 — structural primer

**This is a map, not a substitute for reading the document.** It gives you the shape so the real text is navigable — the actual reading is the Tuesday session, four weeks, one function per week. Fill in the "my notes" sections as you go; that's where the learning actually lands.

Source: NIST AI Risk Management Framework 1.0 (NIST AI 100-1), January 2023 — free at [nist.gov](https://www.nist.gov/itl/ai-risk-management-framework). Also check the companion Playbook and the Generative AI Profile (NIST AI 600-1), which extends the framework to generative systems and is the more directly relevant one for your work.

## Why this framework and not another

Voluntary, not law — which is exactly why it appears in job postings. It's the common vocabulary US organisations use to describe AI risk internally, so being fluent in it means you can read a company's risk documentation on day one. The EU AI Act tells you what you *must* do; the AI RMF gives you a structure for *how*. Serious roles expect familiarity with both.

## The four core functions

**GOVERN** is cross-cutting — it isn't step one of four, it runs through all of the others.

| Function | Question it answers |
|---|---|
| **GOVERN** | Who is accountable, what's the policy, and is there a culture that surfaces problems? |
| **MAP** | What is this system for, in what context, and where does risk come from? |
| **MEASURE** | How do we analyse, test, and track the risks we identified? |
| **MANAGE** | How do we prioritise, respond, and monitor over time? |

A common failure mode in real organisations is strong MEASURE and weak GOVERN — lots of evaluation, no one accountable for acting on it. Worth watching for when you assess a real product.

## Seven characteristics of trustworthy AI

These are what the framework is ultimately trying to protect. The risk-assessment template scores each one.

1. Valid and reliable
2. Safe
3. Secure and resilient
4. Accountable and transparent
5. Explainable and interpretable
6. Privacy-enhanced
7. Fair, with harmful bias managed

Note that several genuinely trade off against each other — explainability against performance, privacy against bias measurement. Being able to name a specific tension in a specific system is what makes an analysis read as practitioner rather than student work.

---

## My notes

### GOVERN — *week of ____*

*Subcategories that stood out:*

*Where I've seen this fail in practice:*

*Question I still have:*

### MAP — *week of ____*

### MEASURE — *week of ____*

### MANAGE — *week of ____*

---

## Cross-reference

Once the OWASP→ATLAS mapping in `redteam-log/03-owasp-atlas/` is underway, note which RMF function each vulnerability class lands under. Most technical findings sit in MEASURE and MANAGE — being able to say *which* is the translation skill the whole plan is built around.
