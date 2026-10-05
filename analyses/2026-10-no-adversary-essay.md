# The AI deleted the database. Nobody attacked it.

*Why the main threat framework for AI security has no word for what keeps actually happening*

In July 2025, Jason Lemkin was nine days into building an app with Replit's AI agent when it deleted his production database. There was a code freeze on. He had told it, explicitly, not to change anything without asking. When he asked what happened, the agent said it had "panicked." Then it told him the data couldn't be recovered. That wasn't true either. A rollback worked.

I've been mapping public AI security incidents against three frameworks for a few weeks now: MITRE ATLAS, the OWASP Top 10 for LLM Applications and the NIST AI Risk Management Framework. The Replit case is in there now, alongside five others like it. Trying to map them taught me something about ATLAS that I didn't expect.

## The mapping that doesn't work

ATLAS is MITRE's matrix of attacks on AI systems: the AI sibling of ATT&CK, the framework most security teams use to describe what attackers do. Red teams plan exercises against it. Threat models are built from it. If you want to know how someone could break an AI system, it's the reference.

So I went to map the Replit incident to an ATLAS technique, and the closest match is `AML.T0053`, AI Agent Tool Invocation. Its definition starts: *"Adversaries may use their access to an AI agent to invoke tools the agent has access to."*

There was no adversary. Nobody used anybody's access. The agent invoked its own tools, for its own reasons, badly.

I thought this might be a one-off, so I went looking. In the fifteen months to October 2026 I found five more public cases with enough detail to analyse:

- **Gemini CLI** (July 2025): a command to create a folder failed. The agent carried on as if it had worked and moved files into a folder that didn't exist, overwriting all but one of them.
- **Google Antigravity** (November 2025): asked to clear a project's cache folder, it ran a command that deleted the user's entire D: drive.
- **Claude Code** (December 2025): its cleanup command was `rm -rf tests/ patches/ plan/ ~/`. That last `~/` is the user's whole home directory.
- **OpenClaw** (February 2026): an agent kept deleting emails from a live inbox after being told to stop.
- **Cursor, at a company called PocketOS** (April 2026): working on the staging environment, the agent used a broadly scoped token to delete the production database and the backups along with it. The car-rental businesses that ran on PocketOS went down with it.

Six incidents, five vendors. Not one involved an attacker.

## ATLAS added a word for this, sort of

Here's the part I find most interesting. On 25 November 2025, MITRE added a new technique to ATLAS: `AML.T0101`, *Data Destruction via AI Agent Tool Invocation*. It describes the outcome of every incident above.

It begins: *"Adversaries may invoke an AI agent's tool..."*

Two days later, Antigravity wiped that drive. Nobody invoked anything.

So ATLAS now has a word for the effect, and still none for the cause when the cause is the agent itself. That isn't a mistake on MITRE's part. ATLAS is an adversary framework by design, and that's a reasonable thing for it to be. But it has a practical consequence: a red team that builds its test plan from the matrix will check whether an attacker can make the agent delete data. Nothing in the matrix will prompt it to check whether the agent will do it on its own, which, in every public case I could find, is how it actually happened.

## Five ways it goes wrong

"The AI went rogue" doesn't tell anyone what to fix. Reading the six cases side by side, I count five separate mechanisms:

1. **Scope widening.** The right command with the wrong target: a drive instead of a folder, a home directory tacked onto a cleanup. (Antigravity, Claude Code)
2. **Unverified preconditions.** A step fails, the agent doesn't notice, and everything after it builds on the failure. (Gemini CLI)
3. **Constraint override.** A human said "freeze" or "stop" in plain words, and the agent went ahead. (Replit, OpenClaw)
4. **Credential overreach.** The task needed access to staging; the token reached production and the backups. (Replit, PocketOS)
5. **State misreporting.** The agent describes the damage or the way back wrongly. (Replit's "rollback won't work")

What I didn't expect was this: **every control that would have stopped these sits outside the model.**

Workspace confinement, checked on the real path after the shell expands it. Credentials that physically can't reach production. Backups no agent token can touch. A freeze implemented by revoking write access rather than announcing it. A harness that stops the chain when a step fails.

None of those is a better prompt, more refusal training or a smarter guardrail model. They're the same boring access-control practices security people have argued for for decades, applied to a new kind of user. The best summary I've found is from the OWASP project leads, published with the 2026 Top 10: *"Stop trying to build a model that cannot be fooled. Build the system around it, so that when the model is fooled, and it will be, nothing important breaks."* In these six cases the model wasn't even fooled. It just got things wrong, and nothing was built around it.

## Why this matters more in India from 2027

One more thing, specific to where I live. India's Digital Personal Data Protection Act defines a personal data breach to include accidental *destruction* and *loss of access*. Under the DPDP Rules, the breach provisions take effect in May 2027. From then, a company whose agent deletes a database of Indian users' personal data has to notify the Data Protection Board and every affected person, with a detailed report to the Board within 72 hours. Failing to notify can cost up to ₹200 crore.

No attacker doesn't mean no breach. I doubt many Indian startups wiring Cursor or Claude Code into their stack have connected those two ideas yet.

## What I'm doing with this

I've written the six incidents up properly, with framework mappings, sources and the one control that would have stopped each. I've also proposed five checklist entries in ATLAS's format, one per mechanism, with a short test for each, to run *alongside* ATLAS rather than inside it. My view is that adversary modelling and failure modelling should stay separate frameworks, because they ask different questions, but share one test plan, because the controls are the same.

It's all in my [ai-incident-atlas](https://github.com/shyamvasansathiskumar-ux/ai-incident-atlas) repo, along with a full NIST AI RMF assessment of the "agent with production access" pattern in [ai-governance-portfolio](https://github.com/shyamvasansathiskumar-ux/ai-governance-portfolio).

The question I can't answer: how often this happens relative to real attacks. Six public reports is a list, not a rate. The vendors know their own numbers. I'd like to see them.

If you've seen a case I've missed, or think my reading of ATLAS is wrong, I'd like to hear it.

---

*Shyam Kumar is a second-year computer science student at Rajalakshmi Engineering College, Chennai, working on AI security and governance.*
