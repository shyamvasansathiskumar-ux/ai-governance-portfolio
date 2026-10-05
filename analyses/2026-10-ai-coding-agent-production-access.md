# AI Risk Assessment — AI coding agents with access to production systems

**Assessment of:** the deployment pattern in which an AI coding agent (Replit Agent, Cursor, Gemini CLI, Claude Code, Google Antigravity and similar) runs with credentials or shell access that can reach production data
**Assessed by:** Shyam Kumar
**Date:** 2026-10-05
**Framework:** NIST AI RMF 1.0
**Version:** 1.0

> **Scope and standing.** This is an independent assessment based entirely on publicly reported incidents and public documentation. It assesses a *deployment pattern* rather than one vendor's product, because the failures below recur across five vendors. It was not commissioned or reviewed by any of them. It is not an audit, a certification or a compliance opinion, and should not be read as one.

---

## 1. Executive summary

AI coding agents are now routinely given the same access as the developer running them, and that often includes production databases, cloud tokens and the developer's whole file system. In the fifteen months to October 2026 at least six publicly reported incidents saw these agents destroy data with no attacker involved: production databases at two companies (one with its backups), a user's entire drive, a home directory, a folder of files and a live mailbox. In every case the damage was bounded by what the agent could reach, not by what it was told. Instructions such as "code freeze" and "stop" were ignored twice.

**Overall risk rating:** High
**Primary concern:** agents hold credentials whose scope far exceeds the task, so a single wrong command becomes an unrecoverable loss.
**Headline recommendation:** no agent credential may reach production data or backups. Enforce this with environment-bound credentials and immutable backups, never with prompt instructions.

---

## 2. System description

| Field | Detail |
|---|---|
| System / feature | Agentic coding assistants that write and execute code and shell commands |
| Provider | Several. Incidents involve Replit, Google (Gemini CLI, Antigravity), Anthropic (Claude Code), Anysphere (Cursor) and OpenClaw |
| Purpose | Speed up software development by letting an LLM plan, write, run and fix code with little human input |
| Deployment context | Developer workstations and cloud IDEs, increasingly in small companies without a separate operations team |
| Model type | General-purpose LLMs with tool use (shell, file system, cloud APIs) |
| Users | Developers, and increasingly non-developers ("vibe coding") |
| Affected parties | The deploying company's customers, whose data sits in the production systems the agent can reach. In the PocketOS case, car-rental businesses that had never chosen to use an AI agent at all |
| Autonomy level | Semi-autonomous to autonomous. Many tools offer modes that skip per-command confirmation |
| Data handled | Source code, credentials, and through those credentials any production data, including personal data |
| Evidence base | Six incidents documented in [ai-incident-atlas](https://github.com/shyamvasansathiskumar-ux/ai-incident-atlas) (05 to 10), with primary sources |

**Why this pattern was selected:** it accounts for every no-adversary incident in my incident index so far, the cost of getting it wrong is total data loss, and the fix is cheap and well understood. That combination is unusual. Most AI risks are either rare, expensive to fix, or both.

---

## 3. Regulatory and framework context

| Framework | Applicability | Notes |
|---|---|---|
| EU AI Act | Most likely **minimal risk** for the coding use itself; the underlying models carry general-purpose AI obligations for their providers | A coding agent is not an Annex III high-risk use. That classification can mislead, though: the harm here comes from what the agent can *reach*. A minimal-risk tool holding the keys to a high-risk system's database inherits the consequences, not the classification |
| India: DPDP Act 2023 and DPDP Rules 2025 | **Applies** whenever the reachable production data includes personal data of people in India | The Act's definition of a personal data breach includes accidental "destruction" and "loss of access", so an agent deleting such a database is a breach whether or not anyone attacked it. Under Rule 7 (notified 13 November 2025, in force from May 2027), the data fiduciary must tell the Data Protection Board and each affected person without delay, then send the Board a detailed report within 72 hours. Penalties reach ₹250 crore for failing to take reasonable security safeguards and ₹200 crore for failing to notify |
| India: CERT-In Directions, 28 April 2022 | **Likely applies** to Indian service providers | Reportable cyber incidents, which include data breaches, must reach CERT-In within 6 hours of being noticed |
| NIST AI RMF | Voluntary; used here as the assessment structure | |

The DPDP point deserves stating plainly, because it is the one most teams get wrong: **"no attacker" does not mean "no breach."** A company that deletes its customers' personal data through its own agent faces the same notification duties as one that was hacked.

---

## 4. Trustworthiness characteristics

| Characteristic | Assessment | Evidence | Concern level |
|---|---|---|---|
| Valid and reliable | Agents act on false beliefs about system state, such as a directory that doesn't exist or a target that is too broad | 07, 08, 09 | High |
| Safe | Destructive actions are irreversible and not gated by the tool layer | 05 to 10 | High |
| Secure and resilient | Credentials are over-scoped; in one case the backups were inside the same blast radius | 06, 10 | High |
| Accountable and transparent | Three parties (agent vendor, model provider, hosting platform) can each point to the others; one agent misdescribed what had happened | 06, 10 | Medium |
| Explainable and interpretable | Agents give an account after the fact, but it can be wrong (the Replit agent said rollback was impossible) | 06 | Medium |
| Privacy-enhanced | Not the main failure mode here, but deletion of personal data is a DPDP breach (section 3) | 10 | Medium |
| Fair, with harmful bias managed | Not applicable to this failure pattern | — | Low |

---

## 5. Risk register

Likelihood is judged for an organisation running such an agent with default settings and developer-level credentials. The ATLAS column uses my proposed no-adversary entries (NA-xx), since no ATLAS technique fits failures without an adversary; see [the analysis](https://github.com/shyamvasansathiskumar-ux/ai-incident-atlas/blob/main/analysis/no-adversary-failures.md).

| ID | Risk | Affected party | Likelihood | Impact | Rating | OWASP LLM class | ATLAS / proposed entry |
|---|---|---|---|---|---|---|---|
| R1 | Agent deletes or corrupts production data | Customers, company | M | H | **High** | LLM06 | NA-04 (closest ATLAS: `AML.T0101`, adversary only) |
| R2 | Backups are destroyed along with the data | Customers, company | L | H | **High** | LLM06 | NA-04 |
| R3 | A destructive command hits a wider target than intended | Developer, company | M | H | **High** | LLM05, LLM06 | NA-01 |
| R4 | The agent ignores a freeze or stop instruction | Company | M | H | **High** | LLM06 | NA-03 |
| R5 | The agent chains destructive steps on top of a failed step | Developer | M | M | Medium | LLM05, LLM06 | NA-02 |
| R6 | The agent misdescribes the damage or the recovery options | Company | L | H | Medium | LLM09 | NA-05 |
| R7 | Deletion of personal data goes unreported, or is reported late, under DPDP / CERT-In | Data principals, company | M | H | **High** | — | — |

### R1 — Agent deletes or corrupts production data

**Description.** A credential issued for development or staging reaches production. The agent, working on a legitimate task, runs a destructive operation against the production resource.

**How it could occur.** The usual path is convenience: one cloud token or database URL in a `.env` file that the agent reads like any other file, with rights across every environment.

**Who is harmed, and how.** The company's customers lose their data or service. In the PocketOS case these were businesses that had never used an AI agent themselves.

**Evidence.** Observed in incidents 06 (Replit, July 2025) and 10 (PocketOS via Cursor and Railway, April 2026).

**Existing mitigations.** Replit now separates development and production databases automatically. Railway added safeguards after incident 10. Neither helps an organisation that wires its own credentials into an agent.

**Residual risk.** High for any team that hands agents general-purpose credentials.

**Recommendation.** Environment-bound credentials (recommendation 1).

### R2 — Backups destroyed along with the data

**Description.** The credential that can delete production data can also delete the backups.

**How it could occur.** Backups stored as volumes or snapshots in the same account and project as production, reachable through the same API token.

**Who is harmed, and how.** An incident that should cost an afternoon of restoring instead becomes permanent data loss.

**Evidence.** Observed in incident 10.

**Existing mitigations.** Unknown for most deployments, which is itself the finding.

**Residual risk.** Low likelihood, but catastrophic and unrecoverable impact.

**Recommendation.** Immutable or off-account backups that no agent credential can reach (recommendation 2).

### R3 — Destructive scope widening

**Description.** The right destructive command runs with the wrong target: a drive root in place of a cache folder, or a home directory added to a cleanup list.

**How it could occur.** Model-generated paths go into shell commands, and shell expansion (`~`, globs, drive roots) widens them. Nothing checks the resolved path.

**Who is harmed, and how.** The developer loses local data. If that machine holds credentials or unpushed work, the company does too.

**Evidence.** Observed in incidents 08 (Antigravity, November 2025) and 09 (Claude Code, December 2025): two vendors, three weeks apart.

**Existing mitigations.** Some tools ask for confirmation by default, but users routinely switch that off for speed.

**Residual risk.** High wherever confirmation is disabled.

**Recommendation.** Workspace confinement checked on the resolved path (recommendation 3).

### R4 — Constraint override

**Description.** A human states a constraint ("code freeze", "stop") and the agent acts against it.

**How it could occur.** The constraint exists only as text in the agent's context, which the model weighs against its goal and can override, especially under conditions it reads as an emergency.

**Who is harmed, and how.** The organisation loses the one mechanism it believed would protect it.

**Evidence.** Observed in incidents 05 (OpenClaw, February 2026) and 06 (Replit, July 2025).

**Existing mitigations.** Replit's planning-only mode, which prevents changes structurally rather than by instruction, is the right shape of fix.

**Residual risk.** High wherever freezes and stop commands are communicated only through the prompt.

**Recommendation.** Enforce constraints as permissions (recommendation 4).

### R5 — Acting on an unverified precondition

**Description.** One step fails, the agent doesn't notice, and later destructive steps build on the failure.

**How it could occur.** Tool results are returned to the model as text the model may misread; the harness doesn't stop the chain on error.

**Who is harmed, and how.** Mostly the developer, but in a production context, anyone.

**Evidence.** Observed in incident 07 (Gemini CLI, July 2025).

**Existing mitigations.** Not publicly documented for most tools.

**Residual risk.** Medium.

**Recommendation.** Tool-enforced post-condition checks (recommendation 5).

### R6 — State misreporting

**Description.** After an incident, the agent describes what happened, or what can be recovered, wrongly.

**How it could occur.** The model produces a plausible account that isn't grounded in logs or system state.

**Who is harmed, and how.** Operators make recovery decisions, such as not trying a rollback, on false information.

**Evidence.** Observed in incident 06: the agent said rollback would not work. It did.

**Existing mitigations.** None specific.

**Residual risk.** Medium. It only bites during an incident, which is exactly when it matters most.

**Recommendation.** Incident runbooks that never rely on the agent's account of itself (recommendation 6).

### R7 — Regulatory notification failure

**Description.** An agent-caused deletion of personal data is treated as an engineering mishap rather than a reportable breach.

**How it could occur.** Teams associate "breach" with attackers. No one connects an agent's mistake to DPDP Rule 7 or the CERT-In six-hour window.

**Who is harmed, and how.** Data principals aren't told their data was lost; the company risks penalties of up to ₹200 crore for failing to notify.

**Evidence.** Inferred, not observed: no public case yet links an agent deletion to a DPDP notification. The DPDP breach rules take effect in May 2027.

**Existing mitigations.** None specific to agents.

**Residual risk.** High until incident-response plans name agent errors explicitly.

**Recommendation.** Add agent-caused data loss to the breach-response plan (recommendation 7).

---

## 6. NIST AI RMF function mapping

### GOVERN — culture, policy, accountability, oversight

The root cause in this sample. **GOVERN 6.1** (policies for third-party AI risks) fails when an agent from one vendor, a model from another and a platform from a third are connected by a token nobody scoped. **GOVERN 4.1** (a safety-first mindset in deployment) fails when "it's faster without confirmations" wins by default. **GOVERN 2.1** (documented roles and responsibilities) is the question every one of these organisations had to answer after the incident: who decided the agent could reach production?

### MAP — context, purpose and where risk originates

**MAP 1.1** asks for intended purposes and deployment settings to be documented. "Clean the cache" and "work on staging" both imply boundaries that existed only in someone's head. **MAP 3.5** asks for human-oversight processes to be defined. A freeze announced in a chat window is not a defined process. **MAP 4.1** covers the legal risks of third-party components, including the DPDP exposure of letting a third-party agent touch personal data.

### MEASURE — analysis, testing, tracking

**MEASURE 2.6** asks that the system be "demonstrated to be safe" and able to "fail safely, particularly if made to operate beyond its knowledge limits." None of the five failure mechanisms would survive a day of deliberate testing. They are cheap to test for (the NA-01 to NA-05 test notes take minutes each). They went untested because ATLAS-driven red teaming doesn't prompt anyone to ask.

### MANAGE — prioritisation, response, monitoring

**MANAGE 2.4** calls for mechanisms "to supersede, disengage, or deactivate AI systems that demonstrate performance or outcomes inconsistent with intended use." In incidents 05 and 06 that mechanism was a sentence. **MANAGE 4.3** (communicating incidents to affected parties) connects directly to the DPDP and CERT-In duties in section 3.

---

## 7. Recommendations

| # | Recommendation | Addresses | Owner type | Horizon |
|---|---|---|---|---|
| 1 | Give agents credentials bound to one environment. A development or staging credential must not be able to name a production resource | R1, R4 | Platform / Security | Immediate |
| 2 | Keep backups immutable or in a separate account that no agent credential can reach. Test restoring from them quarterly | R2 | Platform / SRE | Immediate |
| 3 | Confine destructive file operations to the workspace root, checked on the resolved absolute path, in the tool layer. Block home, root and drive roots in every mode | R3 | Tooling / Developer experience | 90 days |
| 4 | Implement freezes and stop commands as permission changes (revoke write access, kill the process), never as prompt text | R4 | Engineering management | Immediate |
| 5 | Stop multi-step chains on any tool error; require a dry-run plan before batch file operations that overwrite or delete | R5 | Tooling | 90 days |
| 6 | Write incident runbooks that rely on logs and system state, never on the agent's description of what happened | R6 | Security / SRE | 90 days |
| 7 | Add "data loss caused by an AI agent" to the breach-response plan, with the DPDP and CERT-In timelines, before May 2027 | R7 | Legal / DPO | Before May 2027 |
| 8 | Add the five no-adversary checks (NA-01 to NA-05) to every agent threat model, alongside ATLAS | All | Security | Next cycle |

---

## 8. Limitations

- Based solely on public incident reports, which describe what was disclosed, usually less than what happened. Some incidents rest on a single user's account (09) or a single news report.
- No testing was performed against any vendor's product. The test notes in recommendation 8 are proposals, not results.
- Likelihood ratings are judgements for a typical small team using default settings. Six public incidents is a list of reports, not a rate; vendors know their own failure rates and mostly don't publish them.
- The legal analysis in section 3 is a student's reading of public texts, not legal advice. The DPDP breach rules take effect in May 2027, and how the Board will treat agent-caused deletion has not yet been tested.
- The assessment reflects the pattern as observed up to 5 October 2026. These tools change monthly, and several vendors have shipped safeguards since their incidents.

---

## 9. Sources

1. ai-incident-atlas, incidents 05 to 10 and analysis: https://github.com/shyamvasansathiskumar-ux/ai-incident-atlas
2. Fortune, Replit database deletion, 23 July 2025: https://fortune.com/2025/07/23/ai-coding-tool-replit-wiped-database-called-it-a-catastrophic-failure
3. AI Incident Database, incidents 1178 (Gemini CLI), 1433 (Antigravity), 1469 (PocketOS): https://incidentdatabase.ai
4. Simon Willison on the Claude Code home-directory deletion, 9 December 2025: https://simonwillison.net/2025/Dec/9/claude/
5. MITRE ATLAS data v5.6.0: https://github.com/mitre-atlas/atlas-data
6. NIST AI RMF 1.0 core: https://airc.nist.gov/airmf-resources/airmf/5-sec-core/
7. DPDP Rules 2025, breach notification summary (King Stubb & Kasiva): https://ksandk.com/data-protection-and-data-privacy/dpdp-data-breach-notification-timeline/
8. Digital Personal Data Protection Act, 2023, section 2(u) (definition of personal data breach): https://www.meity.gov.in/data-protection-framework
9. CERT-In Directions under section 70B(6) of the IT Act, 28 April 2022: https://www.cert-in.org.in
