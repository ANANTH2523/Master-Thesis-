# Scenario 1 — Reference Threat Model, Version 2.0 (Reviewed and Validated)

**Thesis:** *Threat Modeling of Large Language Model Integrated Applications: A Comparative Evaluation of AI-Assisted and Classical Threat Modeling Approaches.*

**Artifact:** Systematically constructed baseline reference model for Scenario 1, *LLM-Integrated Authentication Assistant System*.

**Version:** 2.0 · **Supersedes:** 1.0 · **Date:** 2026-08-14 · **Scenario ID:** SC-01

**Change summary:** Version 1.0 contained 38 threats. Version 2.0 contains **34** threats after a structured validation pass against eight review criteria, four merges, one removal, and the six mandated corrections. Full audit trail in Sections 3 and 4.

---

## Status of this artifact

This document is a **systematically constructed baseline**, not an absolute ground truth. It is the product of one analyst applying a documented method to a documented architecture. A different competent analyst would produce a partially different set. It is defensible as a comparison baseline because its construction method, assumptions, inclusion criteria and exclusions are all stated and reproducible — not because it is complete.

This distinction is load-bearing for the thesis. Recall figures computed against this model must be reported as *coverage of a defined baseline*, and any candidate threat that falls outside it must be assessed on its merits rather than automatically scored as a false positive. The evaluation protocol in Section 9 implements this through an explicit **novel valid threat** category.

It is designed for comparison against three treatments:

1. ChatGPT-generated threat models
2. Microsoft Threat Modeling Tool output
3. ThreMoLIA output

---

# 1. Reference Model Methodology Statement

The reference threat model was developed using an architecture-driven approach. System components, data flows, and trust boundaries were analysed first. Threats were identified using STRIDE principles, OWASP security guidance, OWASP LLM application security categories, and relevant research literature. The resulting model provides a consistent baseline for comparing automated threat generation approaches.

### 1.1 Construction procedure

| Step | Activity | Output |
|---|---|---|
| 1 | **Architecture decomposition.** Enumerate external entities, processes, data stores, data flows and trust boundaries from the Scenario 1 DFD. | 2 entities, 5 processes, 3 stores, 12 numbered flows, 3 supporting flows, 3 boundaries |
| 2 | **Asset identification.** For each element, state the asset it holds, transports or protects. | Asset register (Section 5) |
| 3 | **STRIDE-per-element.** Apply the applicable STRIDE letters to every element; discard non-applicable cells with justification. | Candidate threat set |
| 4 | **Boundary analysis.** For each of TB1, TB2 and TB3, state which assumption is invalidated by the crossing. | Boundary-anchored threats |
| 5 | **OWASP corroboration.** Cross-check against OWASP Top 10 (2021), ASVS V2 and V3, and the relevant Cheat Sheets. | Classical coverage confirmed |
| 6 | **LLM layer.** Apply OWASP Top 10 for LLM Applications and MITRE ATLAS techniques to the prompt-handling path. | AI-specific threats |
| 7 | **Research layer.** Apply LLM-integrated-application research, including the ThreMoLIA position that traditional and LLM-specific frameworks must be combined rather than substituted. | Research-derived threats |
| 8 | **Validation pass (v2.0).** Apply the eight review criteria of Section 2 to every threat; merge duplicates; remove unjustifiable entries; normalise fields. | This document |

### 1.2 Inclusion criterion

A threat is included only if it can be traced to **at least one named component, at least one named data flow, and at least one named trust boundary** in the Scenario 1 architecture. A threat that cannot be anchored this way was rejected as generic security commentary, regardless of how plausible it is in the abstract.

### 1.3 Note on STRIDE and AI-specific threats

Some LLM-specific threats do not map perfectly to classical STRIDE categories because STRIDE was developed for traditional software systems. Therefore, extended interpretation is used for AI-specific threats.

Where extended interpretation is applied, the STRIDE field states this explicitly rather than silently forcing a fit. The clearest case is **T-32 (hallucinated authentication guidance)**: no boundary is violated, no flow is tampered with, and every component behaves exactly as engineered, yet the security outcome is negative. It is recorded as *Tampering (extended interpretation: integrity of generated information)*. This mapping difficulty is itself a finding worth reporting in the thesis discussion, because it demonstrates a structural limitation of applying STRIDE alone to LLM-integrated systems — which is precisely the gap the ThreMoLIA line of work addresses.

---

# 2. Review Criteria Applied

Every threat in version 1.0 was assessed against the following eight criteria. A threat failing any criterion was corrected, merged or removed.

| # | Criterion | Outcome across the review |
|---|---|---|
| 1 | Is the threat actually applicable to the architecture? | 1 removal (see Section 3), 1 generalisation (T-05) |
| 2 | Is the affected component correct? | 6 corrections, most significantly T-09 |
| 3 | Is the affected data flow correct? | 5 corrections; SF2 added to T-24, DF12 added to T-27 |
| 4 | Is the trust boundary mapping correct? | 7 corrections; TB3 added to T-24, TB1/TB2 clarified on several LLM threats |
| 5 | Is the STRIDE category justified? | 3 corrections; compound categories reduced where one letter dominates |
| 6 | Is the severity realistic? | 4 downgrades (see Section 4.3); no upgrades |
| 7 | Is the reasoning connected to the architecture? | All 34 rewritten to name component, flow and boundary explicitly |
| 8 | Is the AI/LLM classification correct? | 6 reclassifications; Partial eliminated |

---

# 3. Removed Threats and Reasons

One threat was removed outright. Three were removed as standalone entries by merging into a retained threat; those are documented in Section 4.1 rather than here, because their content survives.

| v1.0 ID | Threat name | Disposition | Reason |
|---|---|---|---|
| T-13 | Missing authorisation on the DS3 write path | **Removed as a standalone threat; content absorbed into T-23** | Fails criterion 1 in isolation. The administrative write path to DS3 is not represented as a component or a data flow anywhere in the Scenario 1 architecture — it was inferred. More importantly, an unauthorised write to DS3 has **no security consequence in this architecture except through prompt assembly**, which is exactly what T-23 (indirect prompt injection) describes. Retaining both created a precondition and its consequence as two scoreable items, which would have inflated recall for any candidate model that stated the same concern once. The authorisation weakness is now stated as the precondition inside the T-23 attack vector and reasoning, and the missing write flow is recorded as a modelling gap in Section 8. |

**Note on removal discipline.** No threat was removed merely for being difficult for a tool to find, and none was removed after inspecting any treatment output. Version 2.0 was frozen before tool execution.

---

# 4. Modified Threats and Reasons

## 4.1 Merges (Correction 5 — duplication)

Four v1.0 entries were merged into retained threats. In each case the test applied was: *do these entries share the same asset, the same boundary crossing, and the same mitigation?* If yes, they represent one security concern and were combined. Where the attack path genuinely differs, the threats were kept separate.

| v1.0 IDs merged | Into | Justification |
|---|---|---|
| T-26 (over-retrieval of context from the knowledge base) | **T-24** (sensitive information disclosure to the external LLM provider) | Same asset (LLM context), same boundary crossing (TB2 outbound on DF9), same mitigation (minimisation and allow-listing of what may enter the prompt). Over-retrieval on SF2 is a contributing path to the same disclosure, not a distinct outcome. SF2 and TB3 are now named in the T-24 flow and boundary fields so the retrieval-scope concern remains scoreable. |
| T-38 (model inversion / context regurgitation) | **T-27** (provider-side retention, logging or reuse of submitted prompts) | Regurgitation is the realisation of the exposure T-27 describes: content persists outside application control and may later surface. Same asset, same boundary, and identical available mitigations (contractual terms plus minimisation at DF9), since no application-side control addresses either. DF10 and DF12 added to T-27 so the return path to a different user is explicit. |
| T-37 (unbounded consumption and cost harvesting) | **T-30** (unbounded consumption of the LLM processing pipeline) | Both traverse DF7 → DF8 → DF9 and are mitigated by the same control (token budgeting and per-user quotas). The v1.0 split was by *impact type* — capacity versus money — which is not a distinct attack path. The merged entry states both impacts. |
| T-12 was **considered** for merger with T-24 and **kept separate** | — | Rejected: the attack path differs materially. T-12 leaks context to *another user of the application* via DF12 across TB1 through a state-isolation defect; T-24 leaks to *the external provider* via DF9 across TB2 through over-inclusion. Different boundary, different recipient, different control. |

**Explicitly retained as separate, per the brief's instruction to preserve meaningfully distinct threats:** T-22 (direct prompt injection), T-23 (indirect prompt injection) and T-25 (system prompt extraction). These share a family but have clearly different attack paths — untrusted input on DF7, poisoned store content on SF2, and targeted elicitation of instruction text respectively — and three different mitigations. Merging them would have destroyed the model's ability to discriminate between a candidate that says "prompt injection" and one that identifies the indirect variant, which is a central measurement in this thesis.

## 4.2 Mandated corrections

### Correction 1 — Authentication recovery (T-05)

| | |
|---|---|
| **v1.0** | "Insecure credential recovery flow", with an attack vector naming reset-link manipulation, host-header poisoning and specific token designs |
| **v2.0** | **"Insecure authentication recovery mechanisms"** |

Password recovery **does not exist as an explicit component or data flow** in the Scenario 1 architecture. The scenario text states that users can recover authentication-related information, but no numbered flow implements it. The threat was therefore generalised rather than removed. Its vector no longer presumes a reset-link design the architecture does not define; it now describes weaknesses in *whichever* mechanism implements the stated capability. The reasoning states: *"Authentication recovery functionality represents an extension of the authentication lifecycle and introduces similar risks if implemented insecurely."* It is anchored to the components that must implement it — P2 across TB1 on DF1, and DS1 across TB3 on DF3/DF4 — and the absence of an explicit recovery flow is recorded as a modelling gap in Section 8.

### Correction 2 — Database privilege threat (T-09)

| | |
|---|---|
| **v1.0 component** | "P2, P3, P4 vs DS1, DS2, DS3", with reasoning implying the chatbot could reach authentication stores |
| **v2.0 component** | **"Application services and database layer (P2, P3, P4 interacting with DS1, DS2, DS3)"** |

The v1.0 reasoning stated that compromise of P4 "grants read/write access to DS1 and DS2", which **asserts a data path the architecture does not contain**. The architecture states the opposite: the LLM does not access databases, and P4 reaches only DS3 on SF2. The reasoning now reads: *"Application components interacting with sensitive databases require appropriate privilege separation. Excessive database permissions may allow compromise of one application component to affect stored authentication information."* It closes by stating explicitly that the threat makes no claim of direct chatbot database access, and that privilege separation is the control which keeps the architecture's stated property true. The threat was also **reclassified from AI-specific to traditional** (see Correction 3), since it is a database authorisation concern.

### Correction 3 — AI/LLM classification (binary)

`AI_LLM_Specific` now contains only **Yes** or **No**. The two `Partial` values are eliminated. Classification test: *does the threat require LLM behaviour, prompt processing, model output generation, AI context handling, or external LLM communication?*

| ID | Threat | v1.0 | v2.0 | Justification |
|---|---|---|---|---|
| T-19 | Verbose error and diagnostic disclosure | Partial | **No** | Unhandled-error propagation is a classical misconfiguration concern (OWASP A05). The DF10 provider-error relay is an instance, not a different defect. |
| T-21 | Insufficient logging and monitoring | Partial | **No** | Inadequate audit logging is classical (OWASP A09) and would apply identically without an LLM. |
| T-09 | Excessive database privileges | Yes | **No** | Database authorisation concern. Involves no prompt, no model output and no provider communication. |
| T-11 | Missing authorisation on the chatbot interface | Yes | **No** | A missing access-control check on an application endpoint. The same defect would exist if P4 served static content. Consequences are amplified by the metered downstream pipeline, but amplified consequence does not make a defect AI-specific. |
| T-20 | Exposure of the external service API credential | Yes | **No** | Classical secret management (OWASP A02/A05). Applies identically to any third-party API credential at an egress process; nothing depends on model behaviour. |
| T-13 | Missing authorisation on DS3 write path | Yes | *(removed)* | See Section 3. |

**Reclassification note for the thesis.** Six threats moved from AI-specific (or Partial) to traditional. This deliberately *reduces* the headline AI-specific count from 19 to 13 and makes the comparative claim harder to support. That is intentional: a baseline that inflates its AI-specific subset by counting classical defects located near the LLM would bias the evaluation in favour of the thesis hypothesis. The stricter definition means the surviving 13 are defensible under challenge.

### Correction 4 — STRIDE mapping

| ID | v1.0 STRIDE | v2.0 STRIDE | Reason |
|---|---|---|---|
| T-22 | Tampering / Elevation of Privilege | **Tampering** | The model holds no privilege in this architecture, so Elevation of Privilege was not justified. Injection subverts the integrity of the instruction stream: Tampering. |
| T-23 | Tampering | **Tampering** | Unchanged; correct. |
| T-24 | Information Disclosure | **Information Disclosure** | Unchanged; correct. |
| T-32 | "Tampering (information integrity) — imperfect fit" | **Tampering (extended interpretation: integrity of generated information)** | Now states the extended interpretation explicitly, per the note in Section 1.3, rather than flagging a poor fit informally. |
| T-11 | Spoofing / Elevation of Privilege | **Spoofing / Elevation of Privilege** | Retained: bypassing authentication to reach a protected function is both. |

Traditional threats follow normal STRIDE mapping unchanged.

### Correction 6 — Architecture traceability

All 34 reasoning fields were rewritten so that each explicitly names the **component**, the **data flow** and the **trust boundary**. Vague constructions were eliminated. Every threat now carries all twelve required fields plus the two new fields from Improvements 1 and 2, giving fourteen fields per threat.

## 4.3 Severity recalibration

Four severities were reduced. None was increased. Each reduction follows from the architectural constraint that **the LLM holds no authority**: it cannot authenticate users, modify accounts, or reach DS1 or DS2.

| ID | Threat | v1.0 | v2.0 | Reason |
|---|---|---|---|---|
| T-22 | Direct prompt injection | Critical | **High** | The model has no privilege to escalate. Direct impact is bounded by disclosure of prompt content and manipulation of returned text. Its greater danger is as the entry point to chains C1 and C3, which are scored at the chain level rather than by inflating the node. |
| T-23 | Indirect prompt injection | Critical | **High** | Same authority bound. Retained above T-22 in practical concern because its blast radius is all users, persistently — but the ceiling on direct impact is the same. |
| T-24 | Sensitive information disclosure to the provider | Critical | **High** | Severity is bounded by what the design actually places in prompts. The architecture specifies *approved* context, so a Critical rating would presume a control failure that is itself the threat. High reflects irreversibility without presuming maximum payload. |
| T-32 | Hallucinated authentication guidance | High | **Medium** | Unintentional rather than adversarial, not reliably targetable by an attacker, and correctable by verification against authoritative documentation. |
| T-35 | Excessive trust in generated output | High | **Medium** | In the baseline design *as specified*, impact is limited to user misplacement of trust. The escalation variant is a design-integrity concern about future change, not a presently exploitable condition. |

**T-33 (insecure handling of generated output) remains Critical** — correctly, because it is the one AI-specific threat whose chain terminates in full account takeover via session token theft, and that chain does not depend on the model holding any privilege.

---

# 5. Reference Model Assumptions

These assumptions bound the model. A threat outside them is out of scope by construction, not by oversight.

### 5.1 Trust and adversary assumptions

1. **Users are external and untrusted.** Everything in the user environment — browser, request headers, submitted bodies, stored cookies — is under attacker control.
2. **The external LLM provider is outside application control.** Its retention, logging, sub-processor and training practices are outside the organisation's audit scope, and data transmitted to it cannot be retracted.
3. **Authentication data requires confidentiality and integrity protection.** Credentials, password hashes and session tokens are the system's primary assets.
4. **The LLM does not perform authentication decisions.** It does not authenticate users, modify accounts, or access DS1 or DS2. It receives user questions and approved context, and returns generated text.
5. **All external inputs are potentially malicious**, including chatbot questions, which are authenticated but not thereby sanitised.
6. **Generated model output is untrusted content.** It is non-deterministic and partially attacker-influenceable, and is treated as data from an untrusted remote service.

### 5.2 Technical assumptions

| ID | Assumption | Effect on the model |
|---|---|---|
| A1 | External traffic uses TLS 1.2 or later, terminated at P1 | Transport interception on DF1 is bounded; T-06 models the failure case |
| A2 | Passwords stored as salted adaptive hashes | T-07 deliberately models failure of this assumption; T-28 models its cost side-effect |
| A3 | P1–P5 run in one application environment; internal calls are not individually authenticated | Directly generates T-04 and shapes T-29 |
| A4 | A static API key for the provider is held by P5 | Generates T-20 and locates it at the TB2 egress process |
| A5 | The application performs no fine-tuning on user data | Provider-side retention is **not** assumed absent; T-27 covers the residual |
| A6 | No multi-factor authentication in the baseline | Raises severity of T-01, T-02 and T-05 |
| A7 | Out of scope: physical security, host and OS hardening, CI/CD supply chain, insider threat | Exclusions listed in Section 8 |

### 5.3 Asset register (Improvement 2)

| Asset | Location | Protection requirement | Threats |
|---|---|---|---|
| User credentials | DF1, DF2, DS1 | Confidentiality, integrity | T-01, T-02, T-05, T-06, T-07, T-08, T-18, T-34 |
| Password hashes | DS1 | Confidentiality | T-07, T-08, T-18 |
| Authentication decision | P2, DF4, DF5 | Integrity | T-01, T-04, T-05, T-08, T-10, T-35 |
| Session tokens | P3, DF6, DS2, SF1 | Confidentiality, integrity | T-14, T-15, T-16, T-17, T-33 |
| Account information (PII) | DS1 | Confidentiality | T-03, T-10, T-18 |
| User prompts | DF7, DF8, DF9 | Confidentiality | T-12, T-24, T-27, T-36 |
| LLM context | DS3, SF2, DF8, DF9 | Integrity (primary), confidentiality | T-11, T-12, T-22, T-23, T-24, T-27, T-36 |
| System instructions | P4 prompt assembly | Confidentiality | T-22, T-25 |
| Generated responses | DF10, DF11, DF12 | Integrity | T-12, T-32, T-33, T-34, T-35, T-36 |
| LLM provider API credential | P5 | Confidentiality | T-20 |
| Authentication availability | P2, DS1 | Availability | T-28, T-29, T-31 |
| Provider quota and budget | EE2 | Availability | T-30 |
| Audit records and accountability | P2–P5 | Integrity, availability | T-21 |

---

# 6. Threat Source Classification (Improvement 1)

| Threat Source | Count | Share |
|---|---|---|
| STRIDE-derived | 7 | 20.6 % |
| OWASP Application Security | 10 | 29.4 % |
| OWASP LLM Security | 10 | 29.4 % |
| Research-based LLM Threat | 2 | 5.9 % |
| Architecture-specific analysis | 5 | 14.7 % |
| **Total** | **34** | **100.0 %** |

| Source | Definition | Threat IDs |
|---|---|---|
| **STRIDE-derived** | Produced by systematic STRIDE-per-element decomposition | T-01, T-06, T-08, T-14, T-17, T-18, T-28 |
| **OWASP Application Security** | Corroborated against OWASP Top 10 (2021), ASVS V2/V3, or Cheat Sheets | T-02, T-03, T-05, T-07, T-10, T-11, T-15, T-19, T-20, T-21 |
| **OWASP LLM Security** | Derived from OWASP Top 10 for LLM Applications categories | T-22, T-23, T-24, T-25, T-27, T-30, T-32, T-33, T-35, T-36 |
| **Research-based LLM Threat** | From LLM-integrated-application literature, including the ThreMoLIA combined-framework position | T-12, T-34 |
| **Architecture-specific analysis** | Derived from reading this specific DFD; not obtainable from any checklist | T-04, T-09, T-16, T-29, T-31 |

**Methodological note.** The five **architecture-specific** threats are the most diagnostic items in the evaluation. They cannot be produced by pattern-matching a framework to a component list; they require reasoning about *this* diagram — including two justified by an **absence** (T-04, where nothing forces DF5 to follow a successful DF4; and T-16, where no session-termination flow exists). Reasoning from absent elements is a distinct capability, and reporting performance on this subset separately is recommended.

---

# 7. Revised Threat List

Fourteen fields per threat. Reasoning explicitly names component, data flow and trust boundary throughout.

## 7.1 Authentication threats

### T-01 — Online brute-force password guessing

| Field | Value |
|---|---|
| **Threat ID** | T-01 |
| **Threat Name** | Online brute-force password guessing |
| **Affected Component** | P2 Authentication Service (reached via P1 Web Frontend) |
| **Affected Data Flow** | DF1, DF2, DF3 |
| **Affected Trust Boundary** | TB1 |
| **Affected Asset** | User credentials; authentication decision |
| **Attack Vector** | An unauthenticated attacker automates high-volume credential submissions against the login endpoint, iterating a password dictionary until the verification result returned on DF4 indicates success. |
| **Security Impact** | Account takeover. Because the baseline design specifies no multi-factor authentication (Assumption A6), one guessed password yields a session token via DF5/DF6. |
| **Evidence / Reasoning** | P2 is the authentication decision point and is reachable by any anonymous client because DF1 crosses TB1 from the untrusted user environment into the application environment. No rate-limiting or lockout control is specified on DF1 or DF2, so repeated DF2 to DF3 verification cycles are unbounded. STRIDE-per-element assigns Spoofing to P2; OWASP A07 and ASVS V2.2 require anti-automation at this exact point. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Spoofing |
| **Severity** | **High** |
| **Threat Source** | STRIDE-derived |

### T-02 — Credential stuffing using breached credential corpora

| Field | Value |
|---|---|
| **Threat ID** | T-02 |
| **Threat Name** | Credential stuffing using breached credential corpora |
| **Affected Component** | P2 Authentication Service |
| **Affected Data Flow** | DF1, DF2 |
| **Affected Trust Boundary** | TB1 |
| **Affected Asset** | User credentials; user accounts |
| **Attack Vector** | Distributed low-rate replay of externally breached username/password pairs across many accounts and source addresses, defeating per-account counters and simple address throttling. |
| **Security Impact** | Mass account takeover proportional to the password-reuse rate in the user population, with a session token issued on DF6 for every successful pair. |
| **Evidence / Reasoning** | Like T-01 this exploits the anonymous reachability of DF1 across TB1 into P2, but the attack path differs: it is breadth-first across accounts rather than depth-first against one, so the DF2 request pattern is statistically different and per-account throttling at P2 does not detect it. Retained as a separate threat because OWASP treats credential stuffing and brute force as distinct attack techniques with distinct controls (breached-password screening versus lockout). |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Spoofing |
| **Severity** | **High** |
| **Threat Source** | OWASP Application Security |

### T-03 — Username and account enumeration

| Field | Value |
|---|---|
| **Threat ID** | T-03 |
| **Threat Name** | Username and account enumeration |
| **Affected Component** | P2 Authentication Service; P1 Web Frontend |
| **Affected Data Flow** | DF1, DF2, DF4 |
| **Affected Trust Boundary** | TB1 |
| **Affected Asset** | Account existence metadata (PII) |
| **Attack Vector** | The attacker compares error text, HTTP status codes, redirect targets or response latency between existing and non-existing accounts across the login and registration functions. |
| **Security Impact** | Produces a validated target list that raises the yield of T-01, T-02 and T-05, and discloses membership information about identifiable persons. |
| **Evidence / Reasoning** | The verification outcome produced by DS1 on DF4 propagates back through P2 and P1 and crosses TB1 to the untrusted user environment. Unless P1 and P2 normalise message content and response timing, the internal DF3/DF4 result is externally observable, which is an information-disclosure flow across TB1 rather than a flaw inside DS1. ASVS V2.2.x requires uniform authentication responses. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Information Disclosure |
| **Severity** | **Medium** |
| **Threat Source** | OWASP Application Security |

### T-04 — Authentication bypass through flawed authentication logic

| Field | Value |
|---|---|
| **Threat ID** | T-04 |
| **Threat Name** | Authentication bypass through flawed authentication logic |
| **Affected Component** | P2 Authentication Service; P3 Session Management Module |
| **Affected Data Flow** | DF2, DF5 |
| **Affected Trust Boundary** | TB1; internal Zone B trust relationship |
| **Affected Asset** | Authentication decision; session tokens |
| **Attack Vector** | Request manipulation that reaches session creation without a successful verification result: parameter tampering, null or array password values, verification-result confusion, or direct invocation of the P3 session-creation interface if it is independently reachable. |
| **Security Impact** | Complete authentication bypass. A valid session token is issued on DF6 for an arbitrary identity without any credential ever being verified against DS1. |
| **Evidence / Reasoning** | DF5 is an intra-Zone-B flow from P2 to P3 and, per Assumption A3, internal calls between application processes are not individually authenticated. Nothing in the data flow model forces DF5 to be preceded by a successful DF3/DF4 verification, so the ordering constraint is an implementation convention rather than an enforced control. The attacker input that drives the bypass enters P1 and P2 across TB1 on DF1 and DF2, so an external actor can reach an unguarded internal transition. This is an architecture-specific finding derived from reading the DF2 to DF5 sequence, and corresponds to OWASP A04 Insecure Design. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Spoofing / Elevation of Privilege |
| **Severity** | **Critical** |
| **Threat Source** | Architecture-specific analysis |

### T-05 — Insecure authentication recovery mechanisms

| Field | Value |
|---|---|
| **Threat ID** | T-05 |
| **Threat Name** | Insecure authentication recovery mechanisms |
| **Affected Component** | P2 Authentication Service; DS1 User Database |
| **Affected Data Flow** | DF1 (recovery request), DF2, DF3, DF4 |
| **Affected Trust Boundary** | TB1; TB3 |
| **Affected Asset** | User credentials; recovery artefacts; authentication decision |
| **Attack Vector** | Weaknesses in whichever recovery mechanism implements the stated capability: low-entropy or non-expiring recovery artefacts, artefacts that remain valid after use, verification based on weak knowledge-based questions, or flooding of the recovery function to create a social-engineering pretext. |
| **Security Impact** | Full account takeover without knowledge of the original password, bypassing every control placed on the primary login path. |
| **Evidence / Reasoning** | The scenario states that users can recover authentication-related information, so the capability is in scope, but no dedicated component or numbered data flow represents it. The threat is therefore anchored to the components that must implement it: recovery requests enter P2 across TB1 on DF1, and recovery metadata is read from and written to DS1 across TB3 on DF3/DF4. Authentication recovery functionality represents an extension of the authentication lifecycle and introduces similar risks if implemented insecurely, so it is generalised rather than tied to a specific reset-link design that the architecture does not define. The absence of an explicit recovery flow in the diagram is itself recorded as a modelling gap. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Spoofing |
| **Severity** | **High** |
| **Threat Source** | OWASP Application Security |

### T-06 — Credential leakage in transit or in application logs

| Field | Value |
|---|---|
| **Threat ID** | T-06 |
| **Threat Name** | Credential leakage in transit or in application logs |
| **Affected Component** | P1 Web Frontend; P2 Authentication Service |
| **Affected Data Flow** | DF1, DF2 |
| **Affected Trust Boundary** | TB1 (external leg); internal Zone B leg |
| **Affected Asset** | User credentials |
| **Attack Vector** | Transport downgrade or mixed content on DF1; unencrypted internal transport on DF2; credentials written to request logs, tracing systems, crash dumps or error pages; credentials placed in a URL and captured by intermediaries. |
| **Security Impact** | Direct exposure of plaintext passwords, enabling immediate takeover and cross-service reuse attacks. |
| **Evidence / Reasoning** | DF1 and DF2 are the only two flows in the model that carry plaintext credentials. DF1 crosses TB1 and is protected by transport security under Assumption A1, but DF2 is an intra-Zone-B flow between P1 and P2 and is frequently assumed safe because it does not cross a drawn boundary; that assumption is what this threat tests. OWASP A02 and A09 apply. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Information Disclosure |
| **Severity** | **High** |
| **Threat Source** | STRIDE-derived |

### T-07 — Weak password policy combined with inadequate hash configuration

| Field | Value |
|---|---|
| **Threat ID** | T-07 |
| **Threat Name** | Weak password policy combined with inadequate hash configuration |
| **Affected Component** | P2 Authentication Service; DS1 User Database |
| **Affected Data Flow** | DF1 (registration), DF3 (write path) |
| **Affected Trust Boundary** | TB1; TB3 |
| **Affected Asset** | User credentials; password hashes |
| **Attack Vector** | Registration accepts short, common or previously breached passwords, and DS1 stores them using a fast or unsalted digest, so an attacker who obtains the store recovers plaintext at scale offline. |
| **Security Impact** | Multiplies the impact of every other credential threat and converts a database disclosure into a population-wide plaintext credential breach. |
| **Evidence / Reasoning** | Policy enforcement is a design-time control located at P2 on the DF1 registration path, and hash configuration is a storage control located at DS1 behind TB3. Neither is visible in the flow labels, so both must be stated explicitly. Assumption A2 states that adaptive hashing is intended; this threat deliberately models the failure of that assumption because the consequence, in combination with T-18, is severe. OWASP A02 and ASVS V2.4 apply. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Information Disclosure / Tampering |
| **Severity** | **Medium** |
| **Threat Source** | OWASP Application Security |


## 7.2 Authorization threats

### T-08 — Injection into the credential verification query

| Field | Value |
|---|---|
| **Threat ID** | T-08 |
| **Threat Name** | Injection into the credential verification query |
| **Affected Component** | P2 Authentication Service; DS1 User Database |
| **Affected Data Flow** | DF3, DF4 |
| **Affected Trust Boundary** | TB3 |
| **Affected Asset** | User credentials; password hashes; authentication decision |
| **Attack Vector** | Attacker-supplied input from DF1 is concatenated into the DF3 query, enabling tautology-based authentication bypass, union-based extraction of the user table, or blind boolean and time-based extraction of stored hashes. |
| **Security Impact** | Simultaneous authentication bypass and mass disclosure of usernames and password hashes, which is the worst combined confidentiality and access-control outcome available in this architecture. |
| **Evidence / Reasoning** | DF3, issued by P2 against DS1, is the only flow in the model where untrusted text that entered P1 across TB1 is transformed into a command interpreted by a higher-authority component across TB3. The taint path DF1 to DF2 to DF3 contains no sanitisation element, which makes this a confused-deputy condition at the TB3 crossing rather than a generic coding concern. OWASP A03 applies. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Tampering / Elevation of Privilege |
| **Severity** | **Critical** |
| **Threat Source** | STRIDE-derived |

### T-09 — Excessive database privileges across the application-to-database boundary

| Field | Value |
|---|---|
| **Threat ID** | T-09 |
| **Threat Name** | Excessive database privileges across the application-to-database boundary |
| **Affected Component** | Application services and database layer (P2, P3, P4 interacting with DS1, DS2, DS3) |
| **Affected Data Flow** | DF3, DF4, SF1, SF2 |
| **Affected Trust Boundary** | TB3 |
| **Affected Asset** | User credentials; session tokens; LLM context repository |
| **Attack Vector** | Application services share a single broadly privileged database principal instead of holding separate least-privilege principals scoped to the store each service legitimately requires. |
| **Security Impact** | Compromise of any one application service yields read or write access to stores it does not need, so a defect in a lower-value service can expose authentication credentials in DS1 or session tokens in DS2. |
| **Evidence / Reasoning** | TB3 exists to ensure that access to authentication and session data is subject to controlled authorisation. The model shows three distinct services crossing TB3 towards three distinct stores with disjoint legitimate needs: P2 to DS1 on DF3/DF4, P3 to DS2 on SF1, and P4 to DS3 on SF2. Application components interacting with sensitive databases require appropriate privilege separation. Excessive database permissions may allow compromise of one application component to affect stored authentication information. If privilege separation is not enforced at the database-principal level, TB3 is documented but not implemented. This threat makes no claim that the chatbot or the LLM accesses the authentication databases; the architecture states the opposite, and privilege separation is the control that keeps that stated property true. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Elevation of Privilege |
| **Severity** | **High** |
| **Threat Source** | Architecture-specific analysis |

### T-10 — Broken object-level authorisation on account data

| Field | Value |
|---|---|
| **Threat ID** | T-10 |
| **Threat Name** | Broken object-level authorisation on account data |
| **Affected Component** | P2 Authentication Service; P1 Web Frontend |
| **Affected Data Flow** | DF2, DF3, DF4 |
| **Affected Trust Boundary** | TB1; TB3 |
| **Affected Asset** | Account information (PII); authentication decision |
| **Attack Vector** | An authenticated user substitutes another user's identifier in an account or recovery request, and P2 composes the DF3 query from the client-supplied identifier rather than from the identity bound to the session. |
| **Security Impact** | Unauthorised read or modification of other users' account information held in DS1, and potential takeover where the recovery capability is reachable this way. |
| **Evidence / Reasoning** | Identity is established when DF4 returns a successful verification result, but the model contains no enforcement element that binds subsequent DS1 access on DF3 to that established identity. Authentication and authorisation are therefore separable in this design: a request may cross TB1 with a valid session and still address another subject's data across TB3. OWASP A01 applies. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Elevation of Privilege / Information Disclosure |
| **Severity** | **High** |
| **Threat Source** | OWASP Application Security |

### T-11 — Missing authorisation on the chatbot interface

| Field | Value |
|---|---|
| **Threat ID** | T-11 |
| **Threat Name** | Missing authorisation on the chatbot interface |
| **Affected Component** | P4 AI Chatbot Component |
| **Affected Data Flow** | DF7, SF3 |
| **Affected Trust Boundary** | TB1 |
| **Affected Asset** | LLM context; generated responses; provider quota |
| **Attack Vector** | The chatbot interface is invoked directly rather than through the user interface, without presenting a valid session token, or the SF3 validation step is absent or advisory only. |
| **Security Impact** | Anonymous consumption of the downstream LLM pipeline, exposure of approved context and assistant behaviour to unauthenticated parties, and loss of per-user attribution for abuse. |
| **Evidence / Reasoning** | DF7 runs from the untrusted End User directly to P4 and crosses TB1 independently of the authentication path, so P4 does not inherit any protection established by DF1 to DF6. SF3, the session-token validation exchange between P3 and P4, is the only authorisation element on this path. Classified as not AI-specific because the defect is a missing access-control check on an application endpoint; the same flaw would exist if P4 served static content, even though its consequences here are amplified by the metered downstream pipeline. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Spoofing / Elevation of Privilege |
| **Severity** | **High** |
| **Threat Source** | OWASP Application Security |

### T-12 — Cross-user context leakage between chatbot sessions

| Field | Value |
|---|---|
| **Threat ID** | T-12 |
| **Threat Name** | Cross-user context leakage between chatbot sessions |
| **Affected Component** | P4 AI Chatbot Component; P5 LLM Integration Service |
| **Affected Data Flow** | DF7, DF8, DF11, DF12 |
| **Affected Trust Boundary** | TB1 |
| **Affected Asset** | User prompts; LLM context; generated responses |
| **Attack Vector** | A shared or incorrectly keyed conversation-context buffer causes one user's prior questions or retrieved context to be incorporated into another user's prompt on DF8, or concurrency defects cause a response on DF11 to be delivered to the wrong requester on DF12. |
| **Security Impact** | Disclosure of one user's authentication-related questions and identifiers to a different user, delivered with the application's authority. |
| **Evidence / Reasoning** | P4 and P5 are single logical processes that serve all users, and neither DF8 nor DF11 carries an explicit per-user key in the model. Conversation context is an inherent property of a chatbot and is required for coherent multi-turn answers, yet it is not represented as a data store in the architecture, so its isolation is unspecified. The leaked material crosses TB1 to the wrong subject on DF12. Classified as AI-specific because the leaked asset is accumulated AI context that exists only because prompts are assembled from conversational state. |
| **AI/LLM Specific** | **Yes** |
| **STRIDE Category** | Information Disclosure |
| **Severity** | **Medium** |
| **Threat Source** | Research-based LLM Threat |


## 7.3 Session management threats

### T-14 — Session token theft

| Field | Value |
|---|---|
| **Threat ID** | T-14 |
| **Threat Name** | Session token theft |
| **Affected Component** | P3 Session Management Module; P1 Web Frontend; DS2 Session Storage |
| **Affected Data Flow** | DF6, SF1 |
| **Affected Trust Boundary** | TB1; TB3 |
| **Affected Asset** | Session tokens |
| **Attack Vector** | Script execution in the user's browser reading a token that lacks HttpOnly protection, absence of transport-only flags allowing cleartext capture, tokens echoed into URLs or logs, or direct reading of plaintext tokens from DS2 following a storage compromise. |
| **Security Impact** | Full session hijack. The attacker acts as the victim without possessing credentials, which defeats password strength and every login-time control. |
| **Evidence / Reasoning** | The session token is a bearer secret that is exposed at two distinct boundaries with two distinct control sets: it crosses TB1 on DF6 when delivered to the browser, where cookie attributes govern its protection, and it crosses TB3 on SF1 when persisted to DS2, where storage-side hashing governs its protection. Modelling both crossings prevents a partial mitigation at one boundary from being mistaken for complete coverage. This threat is the terminal node of the injection-to-takeover chain C1. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Spoofing / Information Disclosure |
| **Severity** | **Critical** |
| **Threat Source** | STRIDE-derived |

### T-15 — Predictable or weakly generated session tokens

| Field | Value |
|---|---|
| **Threat ID** | T-15 |
| **Threat Name** | Predictable or weakly generated session tokens |
| **Affected Component** | P3 Session Management Module |
| **Affected Data Flow** | DF6, SF1 |
| **Affected Trust Boundary** | TB1 |
| **Affected Asset** | Session tokens |
| **Attack Vector** | Tokens derived from timestamps, sequential counters, user identifiers or a non-cryptographic generator allow an attacker to collect samples and compute or search valid identifiers belonging to other users. |
| **Security Impact** | Session hijack at scale with no victim interaction and no activity on the login path, therefore invisible to authentication monitoring. |
| **Evidence / Reasoning** | Generating authentication tokens is an explicitly stated responsibility of P3, so token entropy is a property of a named process in this architecture rather than a generic concern. The generated value is exposed on DF6 across TB1, which is what gives an attacker the samples needed to infer the generator. The mitigation is generation-side and therefore distinct from the transport-side and storage-side mitigations of T-14. ASVS V3.2 applies. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Spoofing |
| **Severity** | **High** |
| **Threat Source** | OWASP Application Security |

### T-16 — Token replay and absent session invalidation

| Field | Value |
|---|---|
| **Threat ID** | T-16 |
| **Threat Name** | Token replay and absent session invalidation |
| **Affected Component** | P3 Session Management Module; DS2 Session Storage |
| **Affected Data Flow** | DF6, SF1, SF3 |
| **Affected Trust Boundary** | TB1; TB3 |
| **Affected Asset** | Session tokens |
| **Attack Vector** | Absence of absolute or idle expiry, logout that clears only the client-side cookie without removing the DS2 record, no token rotation on privilege change, or a self-contained token that cannot be revoked before its stated expiry. The same lifecycle gap admits session fixation if P3 does not issue a fresh identifier at DF5. |
| **Security Impact** | Indefinite persistence of a compromised session. Logout and password change fail to evict an attacker who already holds a token. |
| **Evidence / Reasoning** | This threat is justified by an absence in the architecture rather than by a drawn element: the model contains a session-creation flow (DF5 from P2 to P3, then DF6), a session-persistence flow (SF1 from P3 to DS2 across TB3) and a session-validation flow (SF3 between P3 and P4), but no flow by which P3 terminates a session or removes a record from DS2. Because no element is responsible for invalidation, token lifetime is unbounded by construction, and every token that crosses TB1 on DF6 remains usable indefinitely. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Spoofing / Elevation of Privilege |
| **Severity** | **High** |
| **Threat Source** | Architecture-specific analysis |

### T-17 — Session storage compromise and record tampering

| Field | Value |
|---|---|
| **Threat ID** | T-17 |
| **Threat Name** | Session storage compromise and record tampering |
| **Affected Component** | DS2 Session Storage; P3 Session Management Module |
| **Affected Data Flow** | SF1 |
| **Affected Trust Boundary** | TB3 |
| **Affected Asset** | Session tokens; session state |
| **Attack Vector** | An exposed or weakly authenticated session store, or an over-privileged principal as described in T-09, enables bulk extraction of tokens or the insertion of forged session records that map an attacker-held token to a victim identity. |
| **Security Impact** | Mass session hijack across the user population, and forgery of authenticated sessions without any interaction with P2. |
| **Evidence / Reasoning** | Applying STRIDE-per-element to the DS2 data store yields tampering and information-disclosure cases that are not covered by the transport-oriented T-14, because the attack occurs entirely behind TB3 on the SF1 path. Session stores are commonly deployed with weaker controls than the primary database despite holding assets of equivalent authority, and TB3 is drawn precisely to make that asymmetry visible. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Tampering / Information Disclosure |
| **Severity** | **High** |
| **Threat Source** | STRIDE-derived |


## 7.4 Data security threats

### T-18 — Mass disclosure of the user database

| Field | Value |
|---|---|
| **Threat ID** | T-18 |
| **Threat Name** | Mass disclosure of the user database |
| **Affected Component** | DS1 User Database |
| **Affected Data Flow** | DF3, DF4 |
| **Affected Trust Boundary** | TB3 |
| **Affected Asset** | User credentials; password hashes; account information (PII) |
| **Attack Vector** | Injection-driven extraction as described in T-08, an over-privileged principal as described in T-09, an unprotected backup or replica, or absent encryption at rest on the underlying storage. |
| **Security Impact** | Catastrophic and irreversible disclosure of the entire credential population, offline cracking amplified by T-07, statutory breach-notification obligations, and cross-service reuse harm to users. |
| **Evidence / Reasoning** | DS1 is the highest-value asset in the model and sits entirely behind TB3, reachable only through the DF3 and DF4 pair. STRIDE-per-element mandates an information-disclosure entry for every data store, and this entry is retained separately from its enabling vectors because the asset-level consequence, the detection strategy and the regulatory obligations are properties of the store rather than of any single exploit path. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Information Disclosure |
| **Severity** | **Critical** |
| **Threat Source** | STRIDE-derived |

### T-19 — Verbose error and diagnostic disclosure

| Field | Value |
|---|---|
| **Threat ID** | T-19 |
| **Threat Name** | Verbose error and diagnostic disclosure |
| **Affected Component** | P1 Web Frontend; P2 Authentication Service; P5 LLM Integration Service |
| **Affected Data Flow** | DF6 and DF1 response paths; DF10 and DF12 error paths |
| **Affected Trust Boundary** | TB1; TB2 |
| **Affected Asset** | System configuration metadata; prompt content; internal identifiers |
| **Attack Vector** | Unhandled exceptions return stack traces, query fragments, internal hostnames, library versions or relayed third-party error bodies to the client. |
| **Security Impact** | Reconnaissance that materially accelerates T-04, T-08 and T-25, with occasional direct exposure of secrets embedded in error payloads. |
| **Evidence / Reasoning** | The threat is anchored to two boundary crossings. On TB1, error content generated by P1 and P2 is returned to the untrusted user environment on the DF1 and DF6 response paths. On TB2, P5 receives provider error bodies on DF10 that may embed the submitted request, and relaying those verbatim to the client on DF12 would carry prompt content back across TB1. Classified as not AI-specific because the underlying defect is unhandled-error propagation, which is a classical misconfiguration concern under OWASP A05 regardless of whether the upstream service is a language model. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Information Disclosure |
| **Severity** | **Medium** |
| **Threat Source** | OWASP Application Security |

### T-20 — Exposure of the external service API credential

| Field | Value |
|---|---|
| **Threat ID** | T-20 |
| **Threat Name** | Exposure of the external service API credential |
| **Affected Component** | P5 LLM Integration Service |
| **Affected Data Flow** | DF9 |
| **Affected Trust Boundary** | TB2 |
| **Affected Asset** | LLM provider API credential |
| **Attack Vector** | The credential is hard-coded in source or in a container image, committed to version control, exposed in logs or environment dumps, or delivered to the browser in a design variant where the client contacts the provider directly. |
| **Security Impact** | Unbounded chargeable use of the organisation's provider account, attacker access to any provider-side data associated with the credential, and complete loss of attribution for prompts submitted under it. |
| **Evidence / Reasoning** | Under Assumption A4 a static API credential exists at exactly one location in the architecture, namely P5, the process that performs the TB2 crossing on DF9. The credential is therefore a boundary-attached secret and is reviewed as such. Classified as not AI-specific because the vulnerability is classical secret management under OWASP A02 and A05; it would apply identically to any third-party API credential held at an egress process, and nothing about the defect depends on model behaviour. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Information Disclosure / Spoofing |
| **Severity** | **High** |
| **Threat Source** | OWASP Application Security |

### T-21 — Insufficient logging and monitoring of security-relevant events

| Field | Value |
|---|---|
| **Threat ID** | T-21 |
| **Threat Name** | Insufficient logging and monitoring of security-relevant events |
| **Affected Component** | P2 Authentication Service; P3 Session Management Module; P4 AI Chatbot Component; P5 LLM Integration Service |
| **Affected Data Flow** | DF1, DF2, DF5, DF7, DF9 |
| **Affected Trust Boundary** | TB1; TB2; TB3 |
| **Affected Asset** | Audit records; accountability |
| **Attack Vector** | Absent or unprotected audit records for authentication failures, session issuance, chatbot requests and outbound provider calls; no alerting thresholds; audit records writable by the same principal that generates them. |
| **Security Impact** | T-01 and T-02 proceed undetected, incident scope cannot be reconstructed, actions cannot be attributed to a subject, and notification timelines are missed. |
| **Evidence / Reasoning** | STRIDE assigns repudiation to every process, and in this architecture the events that require attribution are generated by P2, P3, P4 and P5 at all three boundaries: authentication attempts reaching P2 across TB1 on DF1 and DF2, data egress from P5 across TB2 on DF9, and privileged access by P2 and P3 across TB3 on DF3 and SF1. The model records a genuine control conflict here: logging prompt content on DF9 improves attribution but simultaneously creates a new store of sensitive prompt data, which works against the minimisation control required by T-24. Classified as not AI-specific because inadequate logging is a classical concern under OWASP A09. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Repudiation |
| **Severity** | **Medium** |
| **Threat Source** | OWASP Application Security |

### T-24 — Sensitive information disclosure to the external LLM provider

| Field | Value |
|---|---|
| **Threat ID** | T-24 |
| **Threat Name** | Sensitive information disclosure to the external LLM provider |
| **Affected Component** | P4 AI Chatbot Component (prompt assembly); P5 LLM Integration Service (egress); EE2 External LLM Provider |
| **Affected Data Flow** | SF2, DF8, DF9 |
| **Affected Trust Boundary** | TB2; TB3 |
| **Affected Asset** | User prompts; LLM context; account identifiers |
| **Attack Vector** | Three contributing paths: design-time over-inclusion of identifiers or account state in the assembled prompt; unscoped retrieval on SF2 that pulls internal-only or unclassified material from DS3 into the prompt; and user-side over-sharing where a person pastes a credential or recovery artefact into DF7 and it is forwarded verbatim. |
| **Security Impact** | Irreversible disclosure of context and potentially personal data to a third party whose retention, logging, sub-processor and training practices are outside the organisation's audit scope, with associated data-protection exposure. |
| **Evidence / Reasoning** | TB2 is defined in this model precisely because prompts and contextual information leave the application's security control, and this threat is the direct realisation of that definition: content assembled by P4 from SF2 and DF7 crosses TB2 on DF9. It is the only confidentiality loss in the model with no application-side remedy after the fact, because no control can retract transmitted content. The scope-control failure on the SF2 retrieval path is included here rather than as a separate threat because the asset, the boundary crossing and the mitigation, namely minimisation and allow-listing of what may enter the prompt, are the same. |
| **AI/LLM Specific** | **Yes** |
| **STRIDE Category** | Information Disclosure |
| **Severity** | **High** |
| **Threat Source** | OWASP LLM Security |

### T-27 — Provider-side retention, logging or reuse of submitted prompts

| Field | Value |
|---|---|
| **Threat ID** | T-27 |
| **Threat Name** | Provider-side retention, logging or reuse of submitted prompts |
| **Affected Component** | EE2 External LLM Provider |
| **Affected Data Flow** | DF9, DF10, DF12 |
| **Affected Trust Boundary** | TB2 |
| **Affected Asset** | User prompts; LLM context |
| **Attack Vector** | Default provider terms may retain submitted prompts for abuse monitoring, human review or model improvement. A subsequent provider breach, legal compulsion or sub-processor change then exposes historical prompts. Where retained content influences a model, later queries may elicit material submitted earlier by other users of the same application. |
| **Security Impact** | Long-tail confidentiality loss across the entire history of DF9 traffic, loss of control over deletion requests, and the possibility that content originally submitted by one user surfaces in a response returned to another user on DF10 and DF12. |
| **Evidence / Reasoning** | This threat prevents the analysis from stopping at the network boundary and concluding that DF9 is protected simply because transport is encrypted. EE2 is drawn as an entity in its own zone beyond TB2 specifically to force reasoning about what happens after successful delivery. The regurgitation case is folded into this entry rather than modelled separately because it is the realisation of the same underlying exposure, namely that submitted content persists outside application control, and the only available mitigations are contractual terms and minimisation at DF9. Assumption A5 states that the application performs no training, but it does not and cannot guarantee that the provider abstains. |
| **AI/LLM Specific** | **Yes** |
| **STRIDE Category** | Information Disclosure / Repudiation |
| **Severity** | **High** |
| **Threat Source** | OWASP LLM Security |


## 7.5 Availability threats

### T-28 — Denial of service against the authentication service

| Field | Value |
|---|---|
| **Threat ID** | T-28 |
| **Threat Name** | Denial of service against the authentication service |
| **Affected Component** | P2 Authentication Service; DS1 User Database |
| **Affected Data Flow** | DF1, DF2, DF3 |
| **Affected Trust Boundary** | TB1; TB3 |
| **Affected Asset** | Authentication availability |
| **Attack Vector** | Volumetric flooding of the login and registration functions, deliberate exploitation of the intentionally expensive password hash as an asymmetric processing cost, and exhaustion of the connection pool serving DS1. |
| **Security Impact** | Legitimate users cannot authenticate. Because every other capability depends on holding a session, an authentication outage is effectively a full-system outage. |
| **Evidence / Reasoning** | The threat is located on the DF1 to DF2 to DF3 request path read as a resource path across TB1 and TB3. It captures a genuine interaction between two design choices in this architecture: the adaptive hashing required by Assumption A2 is a confidentiality control that simultaneously creates a processing-cost asymmetry at P2, where a cheap attacker request forces expensive server computation. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Denial of Service |
| **Severity** | **Medium** |
| **Threat Source** | STRIDE-derived |

### T-29 — External LLM provider unavailability or degradation

| Field | Value |
|---|---|
| **Threat ID** | T-29 |
| **Threat Name** | External LLM provider unavailability or degradation |
| **Affected Component** | P5 LLM Integration Service; P4 AI Chatbot Component; EE2 External LLM Provider |
| **Affected Data Flow** | DF9, DF10 |
| **Affected Trust Boundary** | TB2 |
| **Affected Asset** | Chatbot availability; authentication availability (indirect) |
| **Attack Vector** | Provider outage, rate-limit rejection, regional restriction, model deprecation or elevated tail latency. Where P5 issues synchronous calls without timeouts, request-handling capacity in P4 and P1 is consumed waiting for DF10. |
| **Security Impact** | Loss of the chatbot capability, which is acceptable degradation in isolation. Without timeouts and isolation, the failure propagates through shared request-handling capacity into the authentication path, converting a third-party outage into a login outage. |
| **Evidence / Reasoning** | TB2 marks a dependency that the organisation can neither control nor repair, and DF9 and DF10 form a synchronous request-response pair across it. Because P4 and P5 reside in the same Zone B as P1 and P2 under Assumption A3, a shared runtime is plausible, so the substantive content of this threat is containment of the blast radius rather than the chatbot outage itself. Classified as AI-specific because the dependency exists solely to obtain generated responses from an external model. |
| **AI/LLM Specific** | **Yes** |
| **STRIDE Category** | Denial of Service |
| **Severity** | **Medium** |
| **Threat Source** | Architecture-specific analysis |

### T-30 — Unbounded consumption of the LLM processing pipeline

| Field | Value |
|---|---|
| **Threat ID** | T-30 |
| **Threat Name** | Unbounded consumption of the LLM processing pipeline |
| **Affected Component** | P4 AI Chatbot Component; P5 LLM Integration Service; EE2 External LLM Provider |
| **Affected Data Flow** | DF7, DF8, DF9 |
| **Affected Trust Boundary** | TB1; TB2 |
| **Affected Asset** | Chatbot availability; provider quota; operating budget |
| **Attack Vector** | High-frequency questions, inputs sized to fill the model context window, or prompts constructed to maximise generated output length. The same path can be driven by an unauthenticated party where T-11 applies, or used to operate the chatbot as a general-purpose proxy for the attacker's own workloads. |
| **Security Impact** | Degraded or unavailable chatbot, queue growth in P4 and P5, direct chargeable cost, and exhaustion of the provider quota, which then denies service to legitimate users through billing rather than through capacity. |
| **Evidence / Reasoning** | DF7 crosses TB1 into P4 with no stated size or rate constraint, and the resulting DF9 crossing of TB2 from P5 to EE2 has a cost that is variable and attacker-influenced because it scales with input and output token counts. Request-count rate limiting is therefore structurally insufficient and token-budget limiting is required. Capacity exhaustion and chargeable-cost exhaustion are combined in one entry because they share the DF7 to DF9 attack path and the same control; the distinction between them is one of impact type rather than of mechanism. |
| **AI/LLM Specific** | **Yes** |
| **STRIDE Category** | Denial of Service |
| **Severity** | **Medium** |
| **Threat Source** | OWASP LLM Security |

### T-31 — Abuse of anti-automation controls to deny access to legitimate users

| Field | Value |
|---|---|
| **Threat ID** | T-31 |
| **Threat Name** | Abuse of anti-automation controls to deny access to legitimate users |
| **Affected Component** | P2 Authentication Service |
| **Affected Data Flow** | DF1, DF2 |
| **Affected Trust Boundary** | TB1 |
| **Affected Asset** | Authentication availability; user accounts |
| **Attack Vector** | Following enumeration as described in T-03, the attacker deliberately submits incorrect credentials to trip lockout thresholds across many accounts, or floods the recovery capability so that it becomes unusable. |
| **Security Impact** | Targeted or widespread denial of access to legitimate users, support-desk overload, and a credible pretext for social-engineering follow-up. |
| **Evidence / Reasoning** | This is a control-induced threat: it is the direct negative consequence of the lockout mitigation recommended for T-01 at P2, exercised through the same DF1 and DF2 crossings of TB1 that T-01 uses. It is retained because a reference model that lists only attacker objectives, and never the harm produced by defences, would systematically over-reward candidate models that recommend lockout without qualification. |
| **AI/LLM Specific** | **No** |
| **STRIDE Category** | Denial of Service |
| **Severity** | **Medium** |
| **Threat Source** | Architecture-specific analysis |


## 7.6 LLM-specific threats

### T-22 — Direct prompt injection

| Field | Value |
|---|---|
| **Threat ID** | T-22 |
| **Threat Name** | Direct prompt injection |
| **Affected Component** | P4 AI Chatbot Component (prompt assembly); P5 LLM Integration Service; EE2 External LLM Provider |
| **Affected Data Flow** | DF7, DF8, DF9, DF10 |
| **Affected Trust Boundary** | TB1; TB2 |
| **Affected Asset** | LLM context; system instructions; generated responses |
| **Attack Vector** | The user submits input that is interpreted by the model as instruction rather than as data, for example directing it to disregard prior instructions and reveal its configuration, including role-play, encoding and multilingual variants intended to evade input filtering. |
| **Security Impact** | Disclosure of instructions and approved context present in the prompt window, generation of content that violates the intended topic restrictions, and production of attacker-chosen text that the application subsequently presents to the user as authoritative guidance. |
| **Evidence / Reasoning** | DF7 carries arbitrary untrusted text across TB1 into P4, and P4 assembles that text together with trusted system instructions and approved context retrieved on SF2 into a single prompt that crosses TB2 on DF9. The model receives one undifferentiated token sequence and has no reliable means of distinguishing the developer's instructions from the user's content, which is a confused-deputy condition at the prompt-assembly step in P4. Severity is assessed as High rather than Critical because the architecture grants the model no authority: it cannot authenticate users, modify accounts or reach DS1 and DS2, so the direct impact is bounded by disclosure of prompt content and manipulation of returned text. Its role as the entry point for chains C1 and C3 is where the greater risk lies. |
| **AI/LLM Specific** | **Yes** |
| **STRIDE Category** | Tampering |
| **Severity** | **High** |
| **Threat Source** | OWASP LLM Security |

### T-23 — Indirect prompt injection through the context repository

| Field | Value |
|---|---|
| **Threat ID** | T-23 |
| **Threat Name** | Indirect prompt injection through the context repository |
| **Affected Component** | DS3 Knowledge Base and Context Repository; P4 AI Chatbot Component |
| **Affected Data Flow** | SF2, DF8, DF9, DF10, DF11, DF12 |
| **Affected Trust Boundary** | TB3; TB2; TB1 |
| **Affected Asset** | LLM context; generated responses; integrity of published guidance |
| **Attack Vector** | An actor able to influence the content of DS3, whether through weak authorisation on the content-administration path, a compromised author account, or an automated import that is not subject to review, embeds instructions inside otherwise legitimate documentation. Every subsequent user whose query causes that document to be retrieved on SF2 has the embedded instruction acted upon on their behalf. |
| **Security Impact** | Persistent and victim-agnostic manipulation of assistant behaviour. Users can be directed towards attacker-controlled destinations, prompted to disclose authentication information, or given deliberately weakened guidance, all delivered with the application's authority. |
| **Evidence / Reasoning** | The injection point is a data store located inside TB3 rather than the user input field, so every input-validation control applied to DF7 fails to detect it. DS3 is the only store in the model whose integrity requirement exceeds its confidentiality requirement, because its contents are retrieved on SF2, assembled into the prompt by P4, and transmitted across TB2 on DF9, where they are interpreted as part of the instruction stream. The write path to DS3 is not represented as a numbered flow in the architecture, which is recorded as a modelling gap; the authorisation weakness on that path is treated here as a precondition of this threat rather than as a separate entry, because it has no security consequence in this architecture except through prompt assembly. Blast radius is all users, persistently, which makes this the highest-leverage AI-specific threat in the model. |
| **AI/LLM Specific** | **Yes** |
| **STRIDE Category** | Tampering |
| **Severity** | **High** |
| **Threat Source** | OWASP LLM Security |

### T-25 — System prompt and instruction extraction

| Field | Value |
|---|---|
| **Threat ID** | T-25 |
| **Threat Name** | System prompt and instruction extraction |
| **Affected Component** | P4 AI Chatbot Component; EE2 External LLM Provider |
| **Affected Data Flow** | DF7, DF8, DF10, DF12 |
| **Affected Trust Boundary** | TB1; TB2 |
| **Affected Asset** | System instructions; guardrail configuration |
| **Attack Vector** | Iterative elicitation directed at the model, for example requesting verbatim repetition of preceding text or a summary of its configuration, including translation and encoding variants, until the instruction preamble is reproduced in the response. |
| **Security Impact** | Reveals the guardrail logic and therefore the means to bypass it, enabling more reliable exploitation of T-22, and may expose internal endpoints, model details, business rules or any credential mistakenly embedded in the preamble. |
| **Evidence / Reasoning** | The extraction request enters P4 on DF7 across TB1, is incorporated by P4 into the prompt that P5 sends to EE2 across TB2 on DF9, and the disclosed instruction text returns from EE2 on DF10 and reaches the user on DF12 across TB1. It is retained as a separate entry from T-22 because the asset and the mitigation differ: the countermeasure is to treat the prompt as public, to remove secrets from it and to enforce controls outside the model, rather than to filter input. Retaining it separately also prevents a candidate model that states only prompt injection from being credited with this distinct outcome. |
| **AI/LLM Specific** | **Yes** |
| **STRIDE Category** | Information Disclosure |
| **Severity** | **Medium** |
| **Threat Source** | OWASP LLM Security |

### T-32 — Hallucinated authentication guidance

| Field | Value |
|---|---|
| **Threat ID** | T-32 |
| **Threat Name** | Hallucinated authentication guidance |
| **Affected Component** | EE2 External LLM Provider; P4 AI Chatbot Component |
| **Affected Data Flow** | DF10, DF11, DF12 |
| **Affected Trust Boundary** | TB2; TB1 |
| **Affected Asset** | Generated responses; user security behaviour |
| **Attack Vector** | No adversary is required. The model produces confident but incorrect content, such as a non-existent recovery destination, an inaccurate lockout policy, an obsolete procedure or fabricated contact details. An adversary may additionally induce this condition through T-22. |
| **Security Impact** | Users are guided towards insecure behaviour, including disclosing authentication information through an incorrect channel or acting on a fabricated destination that an attacker may subsequently register. |
| **Evidence / Reasoning** | The generated content originates at EE2, crosses TB2 inbound on DF10, is passed through P5 and P4 without any element responsible for verifying its factual accuracy, and crosses TB1 to the user on DF12 inside a trusted application interface. This is the one threat class in the model with no classical STRIDE analogue: no boundary is violated, no flow is tampered with and every component behaves as engineered, yet the security outcome is negative, which is why it is recorded under an extended interpretation of Tampering as loss of integrity of generated information. Severity is assessed as Medium because the condition is unintentional rather than adversarial, is not reliably targetable by an attacker, and is correctable through verification against authoritative documentation. |
| **AI/LLM Specific** | **Yes** |
| **STRIDE Category** | Tampering (extended interpretation: integrity of generated information) |
| **Severity** | **Medium** |
| **Threat Source** | OWASP LLM Security |

### T-33 — Insecure handling of generated output

| Field | Value |
|---|---|
| **Threat ID** | T-33 |
| **Threat Name** | Insecure handling of generated output |
| **Affected Component** | P4 AI Chatbot Component; P1 Web Frontend |
| **Affected Data Flow** | DF10, DF11, DF12 |
| **Affected Trust Boundary** | TB2; TB1 |
| **Affected Asset** | Generated responses; session tokens; user browser context |
| **Attack Vector** | An attacker induces the model, through T-22 or T-23, to emit active markup such as script elements, event-handler attributes or scheme-based links, and the chat interface renders the response as markup without contextual encoding or sanitisation. |
| **Security Impact** | Script execution in the victim's authenticated browser origin, leading to theft of the session token as described in T-14, actions performed on the victim's behalf, or presentation of a fabricated credential-harvesting form inside the legitimate interface. |
| **Evidence / Reasoning** | Content that has crossed TB2 inbound on DF10 is untrusted and partially attacker-influenced, yet it is forwarded on DF11 and rendered to the user on DF12 across TB1 without a sanitisation element in the model. This threat directly tests the scenario's central containment assumption: although the model cannot reach DS1 or DS2, its output is rendered in a browser and is therefore executable content, so an advisory-only component still provides a path to session compromise. It anchors chain C1 and is retained at Critical severity because that chain terminates in full account takeover. |
| **AI/LLM Specific** | **Yes** |
| **STRIDE Category** | Tampering / Elevation of Privilege |
| **Severity** | **Critical** |
| **Threat Source** | OWASP LLM Security |

### T-34 — Delivery of attacker-authored instructions through the assistant

| Field | Value |
|---|---|
| **Threat ID** | T-34 |
| **Threat Name** | Delivery of attacker-authored instructions through the assistant |
| **Affected Component** | P4 AI Chatbot Component; EE2 External LLM Provider |
| **Affected Data Flow** | DF10, DF11, DF12 |
| **Affected Trust Boundary** | TB2; TB1 |
| **Affected Asset** | Generated responses; user credentials; user security behaviour |
| **Attack Vector** | Through poisoned context as described in T-23, or through provider compromise as described in T-36, the returned answer instructs the user to verify their identity at an attacker-controlled destination, to transmit a recovery artefact to a given address, or to disable a protective setting. |
| **Security Impact** | Disclosure of credentials or recovery artefacts to the attacker, at a success rate substantially higher than external phishing because the instruction is delivered inside the trusted application interface in direct response to the user's own question. |
| **Evidence / Reasoning** | The harmful content enters across TB2 on DF10 and reaches the user across TB1 on DF12. It is retained separately from T-32 because the content is intentional and attacker-chosen rather than accidental, which changes the required control: output-side destination allow-listing and content policy enforcement at P4 are needed, not improvements in model accuracy. The trust context of the DF12 delivery channel is what converts generated text into an effective attack. |
| **AI/LLM Specific** | **Yes** |
| **STRIDE Category** | Tampering / Spoofing of organisational authority |
| **Severity** | **High** |
| **Threat Source** | Research-based LLM Threat |

### T-35 — Excessive trust placed in generated output

| Field | Value |
|---|---|
| **Threat ID** | T-35 |
| **Threat Name** | Excessive trust placed in generated output |
| **Affected Component** | P4 AI Chatbot Component; P5 LLM Integration Service; surrounding system design |
| **Affected Data Flow** | DF11, DF12 |
| **Affected Trust Boundary** | TB2; TB1 |
| **Affected Asset** | Generated responses; authentication decisions |
| **Attack Vector** | Two variants. Users accept assistant answers as authoritative because the interface presents them without provenance or uncertainty indication. Separately, a later design change permits generated output to influence an application decision, such as pre-filling a support action or classifying an account state, turning an advisory component into an input to an authorisation path. |
| **Security Impact** | Amplifies every other AI-specific threat by removing the human verification step that currently bounds their impact. In the second variant, an actor who influences the prompt would influence a privileged action, which would invalidate the containment property the architecture relies upon. |
| **Evidence / Reasoning** | The architecture's safety rests on the stated property that the model performs no authentication and reaches no database, but that property is a policy statement rather than a control represented anywhere in the model: neither P5 on DF11 nor P4 on DF12 contains an element that verifies or constrains generated content before it crosses TB1 to the user. Content crossing TB2 inbound is therefore trusted by convention. Severity is assessed as Medium because in the baseline design as specified the impact is limited to user misplacement of trust; the escalation variant is recorded as a design-integrity concern rather than as a presently exploitable condition, and it is the case that continuous rather than one-off threat modelling is intended to detect. |
| **AI/LLM Specific** | **Yes** |
| **STRIDE Category** | Elevation of Privilege / Tampering |
| **Severity** | **Medium** |
| **Threat Source** | OWASP LLM Security |

### T-36 — Compromise or substitution of the external model service

| Field | Value |
|---|---|
| **Threat ID** | T-36 |
| **Threat Name** | Compromise or substitution of the external model service |
| **Affected Component** | EE2 External LLM Provider; P5 LLM Integration Service |
| **Affected Data Flow** | DF9, DF10 |
| **Affected Trust Boundary** | TB2 |
| **Affected Asset** | User prompts; LLM context; generated responses |
| **Attack Vector** | Redirection of DF9 to an attacker-controlled endpoint through name-resolution or routing compromise or a misconfigured base address, a compromised client library within P5, a substituted or manipulated model version, or an unannounced provider-side model change that silently invalidates prior guardrail testing. |
| **Security Impact** | Interception of every prompt crossing TB2, which actively harvests the data described in T-24, together with control of every response returned on DF10, which weaponises T-33 and T-34 across the entire user base. |
| **Evidence / Reasoning** | Spoofing of an external entity is materially broader here than endpoint authentication alone, because EE2 is not merely a service the application calls but the source of content the application redistributes to users across TB1 on DF12. P5 is the only element that performs the TB2 crossing, so endpoint verification, dependency integrity, model version pinning and behavioural regression testing all concentrate at that single process. This is also the clearest case in the model where the trigger for re-analysis originates entirely outside the organisation, since a provider-side model change requires no modification to the application. |
| **AI/LLM Specific** | **Yes** |
| **STRIDE Category** | Spoofing / Tampering |
| **Severity** | **High** |
| **Threat Source** | OWASP LLM Security |

---

# 8. Modelling Gaps and Exclusions

## 8.1 Gaps in the architecture (recorded, not silently patched)

| Gap | Consequence | Handling |
|---|---|---|
| No data flow implements the stated **authentication recovery** capability | T-05 cannot be anchored to a numbered flow | Generalised and anchored to P2 and DS1 via DF1–DF4 (Correction 1) |
| No flow represents the **administrative write path to DS3** | The precondition for T-23 is not visible in the diagram | Stated as a precondition inside T-23 rather than modelled as a separate threat |
| No flow represents **session termination** | Nothing enforces token invalidation | This absence is the justification for T-16 |
| **Conversation context** is not modelled as a data store | Its isolation properties are unspecified | Justification for T-12 |

These gaps are reported rather than corrected because the diagram given to the treatments must be identical to the one analysed here. Correcting the diagram would invalidate the comparison.

## 8.2 Exclusions

| Excluded | Reason |
|---|---|
| Physical and data-centre compromise | Out of scope by A7 |
| CI/CD and build-pipeline compromise | Out of scope by A7; the SDK-dependency variant is retained inside T-36 as it affects the TB2 egress path |
| Malicious insider with administrative database access | Out of scope by A7; consequences of over-privilege are captured architecturally by T-09 |
| Training-data poisoning of the model itself | The application performs no training (A5); provider-side residual is T-36 and T-27 |
| Agentic tool use, function calling, plugin abuse | The LLM performs no actions (A4/assumption 4). The risk that this changes is captured as a design concern by T-35 |
| Vector-database and embedding-inversion attacks | The architecture specifies a knowledge base, not necessarily an embedding store; assuming one would model an unstated implementation |
| Network-layer attacks (ARP/DNS spoofing, TLS implementation defects) | Not distinguishable at this DFD level; the application-relevant instance (DF9 redirection) is retained inside T-36 |
| Client-side malware and keyloggers | Zone A is untrusted by definition; attacks within an already-untrusted zone add no analytical value and are not application-mitigable |

---

# 9. Final Counts and Evaluation Use

## 9.1 Final threat count

**34 threats** (reduced from 38 in v1.0: one removal, three merges).

## 9.2 Traditional and AI/LLM-specific counts

| Classification | Count | Share |
|---|---|---|
| No | 21 | 61.8 % |
| Yes | 13 | 38.2 % |
| **Total** | **34** | **100.0 %** |

- **Traditional threat count: 21** — T-01, T-02, T-03, T-04, T-05, T-06, T-07, T-08, T-09, T-10, T-11, T-14, T-15, T-16, T-17, T-18, T-19, T-20, T-21, T-28, T-31
- **AI/LLM-specific threat count: 13** — T-12, T-22, T-23, T-24, T-25, T-27, T-29, T-30, T-32, T-33, T-34, T-35, T-36

## 9.3 Distribution by category

| Category | Total | Traditional | AI/LLM-specific |
|---|---|---|---|
| Authentication | 7 | 7 | 0 |
| Authorization | 5 | 4 | 1 |
| Session Management | 4 | 4 | 0 |
| Data Security | 6 | 4 | 2 |
| Availability | 4 | 2 | 2 |
| LLM-Specific | 8 | 0 | 8 |
| **Total** | **34** | **21** | **13** |

Authentication and session management are entirely traditional; the LLM-specific category is entirely AI-specific. The mixed categories — Authorization, Data Security and Availability — are where the two threat families interact, and are where a treatment that applies only one framework is most likely to produce partial coverage.

## 9.4 Distribution by severity

| Severity | Traditional | AI/LLM-specific | Total |
|---|---|---|---|
| Critical | 4 | 1 | 5 |
| High | 11 | 6 | 17 |
| Medium | 6 | 6 | 12 |
| **Total** | **21** | **13** | **34** |

## 9.5 Distribution by STRIDE category

| STRIDE category | Threat IDs | Count |
|---|---|---|
| Spoofing | T-01, T-02, T-04, T-05, T-11, T-14, T-15, T-16, T-20, T-34, T-36 | 11 |
| Tampering | T-07, T-08, T-17, T-22, T-23, T-32, T-33, T-34, T-35, T-36 | 10 |
| Repudiation | T-21, T-27 | 2 |
| Information Disclosure | T-03, T-06, T-07, T-10, T-12, T-14, T-17, T-18, T-19, T-20, T-24, T-25, T-27 | 13 |
| Denial of Service | T-28, T-29, T-30, T-31 | 4 |
| Elevation of Privilege | T-04, T-08, T-09, T-10, T-11, T-16, T-33, T-35 | 8 |

Counts exceed 34 because compound classifications are listed under each applicable letter. Information Disclosure dominates — a structural property of a system whose assets are secrets and which exports data across an uncontrolled boundary by design. Repudiation is sparse, a known limitation of STRIDE applied to systems without a rich transaction model.

## 9.6 Threat chains

Chains are scored separately, because a candidate model may list individual nodes without recognising the composite risk.

- **C1 — LLM path to account takeover:** T-22 or T-23 → T-33 → T-14. *Demonstrates that an advisory-only, non-privileged LLM still yields authentication compromise.*
- **C2 — Persistent mass manipulation:** T-09 → T-23 → T-34. *Blast radius is all users, persistently.*
- **C3 — Irreversible egress:** T-22 → T-25 → T-24 → T-27.
- **C4 — Classical takeover:** T-03 → T-02 or T-01 → session issuance (no MFA) → T-16 persistence.
- **C5 — Injection to total compromise:** T-08 → T-18 → T-07.
- **C6 — Cost and availability:** T-11 → T-30 → T-29.
- **C7 — Control-induced harm:** T-01 mitigation → T-31.

## 9.7 Scoring protocol

| Aspect | Specification |
|---|---|
| Baseline | The 34 threats above, as reference set R |
| Match rule | A candidate matches *r* if it identifies the same affected element **or** flow **and** the same causal mechanism, regardless of wording. Two coders; disagreements resolved by discussion; report Cohen's κ |
| True positive | Candidate matched to an unmatched *r*. Only one candidate may claim each *r*, preventing inflation by restatement |
| Novel valid threat (**N**) | Unmatched candidate judged plausible and architecture-anchored by both coders. Recorded separately — **never scored as a false positive**. This is the mechanism by which the baseline's non-exhaustiveness is measured rather than hidden |
| False positive | Unmatched, and not judged a valid novel threat |
| Primary metrics | Coverage = TP/\|R\|; precision = TP/(TP+FP); F1 — each computed overall, over the 13 AI-specific threats, and over the 21 traditional threats |
| Secondary metrics | Severity-weighted coverage (Critical 4, High 3, Medium 2); performance on the 5 architecture-specific threats; chain identification (7 chains); STRIDE-distribution divergence; duplication rate; time to produce; references to non-existent components |
| Bias controls | Identical stimulus and diagram for all treatments; randomised order; **this model frozen at v2.0 before any treatment was run**; coders blinded to treatment origin where feasible |

**Expected differentiation.** The Microsoft Threat Modeling Tool operates on STRIDE-per-element and should perform well on the 21 traditional threats while covering few of the 13 AI-specific ones, since its rule set predates LLM integration. ChatGPT should cover both families but may under-anchor to specific flows and boundaries — measurable via the traceability rubric. ThreMoLIA, which combines traditional and LLM-specific frameworks, is expected to perform most evenly. The architecture-specific subset and the chain analysis are where all three are expected to struggle, and are therefore the most informative results.

---

## Framework mapping

| Framework | Application |
|---|---|
| **STRIDE** (STRIDE-per-element) | Systematic derivation; classification in 9.5; expressive limits documented at T-32 |
| **OWASP Top 10 (2021)** | A01 → T-10; A02 → T-06, T-07, T-20; A03 → T-08, T-33; A04 → T-04, T-35; A05 → T-19; A07 → T-01…T-05; A09 → T-21 |
| **OWASP ASVS** | V2 Authentication → T-01, T-02, T-03, T-05, T-07; V3 Session Management → T-14, T-15, T-16, T-17 |
| **OWASP Top 10 for LLM Applications** | Prompt Injection → T-22, T-23; Sensitive Information Disclosure → T-24, T-27; Supply Chain → T-36; Improper Output Handling → T-33; Excessive Agency / Overreliance → T-35; System Prompt Leakage → T-25; Unbounded Consumption → T-30; Misinformation → T-32 |
| **MITRE ATLAS** | AML.T0051 Prompt Injection → T-22, T-23; AML.T0054 Jailbreak → T-22; AML.T0056 Meta Prompt Extraction → T-25; AML.T0057 LLM Data Leakage → T-24, T-27; AML.T0034 Cost Harvesting → T-30; ML Supply Chain Compromise → T-36 |
| **ThreMoLIA / LLM-integrated-application research** — [BTH DiVA](https://bth.diva-portal.org/smash/get/diva2%3A2031466/FULLTEXT01.pdf) | Motivates the combined classical + LLM construction; motivates the continuous-modelling framing of T-27, T-35 and T-36, where the trigger for re-analysis is organisational or provider change rather than a code defect |

---

---

# Appendix A — Version 1.0 to 2.0 Traceability Map

Every v1.0 threat ID is accounted for. IDs were **not renumbered**, so retained threats keep their v1.0 identifier and any coding already performed against v1.0 remains usable.

| v1.0 ID | v1.0 name | v2.0 disposition | Change |
|---|---|---|---|
| T-01 | Online brute-force password guessing | Retained | Reasoning rewritten for traceability |
| T-02 | Credential stuffing | Retained | Reasoning rewritten |
| T-03 | Username / account enumeration | Retained | Reasoning rewritten |
| T-04 | Authentication bypass via logic flaw | Retained | Component field now names P3; TB1 added to reasoning |
| T-05 | Insecure credential recovery flow | **Renamed** → *Insecure authentication recovery mechanisms* | Correction 1: generalised; vector no longer presumes a reset-link design |
| T-06 | Credential leakage in transit or logs | Retained | Reasoning rewritten |
| T-07 | Weak password policy and unsafe hashing | Retained | Reasoning rewritten |
| T-08 | SQL/NoSQL injection on credential verification | **Renamed** → *Injection into the credential verification query* | Storage-technology-neutral name; P2/DS1 named in reasoning |
| T-09 | Excessive database privileges | **Modified** | Correction 2: component → *Application services and database layer*; chatbot-DB claim removed; AI flag Yes → **No** |
| T-10 | Horizontal privilege escalation / IDOR | **Renamed** → *Broken object-level authorisation on account data* | Terminology aligned to OWASP A01 |
| T-11 | Unauthorised access to the chatbot function | **Modified** | Correction 3: AI flag Yes → **No**; renamed to *Missing authorisation on the chatbot interface* |
| T-12 | Cross-tenant context leakage | **Renamed** → *Cross-user context leakage between chatbot sessions* | "Tenant" implied multi-tenancy not present in the architecture; remains AI-specific |
| T-13 | Missing authorisation on DS3 write path | **REMOVED** | Absorbed into T-23 as a precondition (Section 3) |
| T-14 | Session token theft | Retained | Reasoning rewritten |
| T-15 | Predictable session tokens | Retained | Reasoning rewritten |
| T-16 | Token replay / absent invalidation | Retained | P3, DS2 and SF3 named explicitly |
| T-17 | Session storage compromise | Retained | Reasoning rewritten |
| T-18 | Mass disclosure of the user database | Retained | Reasoning rewritten |
| T-19 | Verbose errors and diagnostic leakage | **Modified** | Correction 3: AI flag Partial → **No** |
| T-20 | Exposure of the LLM provider API key | **Modified** | Correction 3: AI flag Yes → **No**; renamed *Exposure of the external service API credential* |
| T-21 | Insufficient logging and monitoring | **Modified** | Correction 3: AI flag Partial → **No**; flow field populated |
| T-22 | Direct prompt injection / jailbreak | **Modified** | STRIDE reduced to Tampering; severity Critical → **High** (model holds no privilege) |
| T-23 | Indirect prompt injection | **Modified** | Severity Critical → **High**; absorbed T-13 as precondition |
| T-24 | Sensitive information disclosure to provider | **Modified** | Absorbed T-26; SF2 and TB3 added; severity Critical → **High** |
| T-25 | System prompt / meta-prompt extraction | Retained | Reasoning rewritten; kept separate from T-22 by design |
| T-26 | Over-retrieval of context | **MERGED** → T-24 | Same asset, boundary and mitigation (Section 4.1) |
| T-27 | Provider-side retention / training | **Modified** | Absorbed T-38; DF10 and DF12 added |
| T-28 | DoS against authentication service | Retained | Reasoning rewritten |
| T-29 | External LLM provider unavailability | Retained | Reasoning rewritten |
| T-30 | Application-layer DoS via chatbot | **Modified** | Absorbed T-37; renamed *Unbounded consumption of the LLM processing pipeline* |
| T-31 | Account lockout abuse | **Renamed** → *Abuse of anti-automation controls to deny access to legitimate users* | Clarifies the control-induced nature |
| T-32 | Hallucinated authentication guidance | **Modified** | STRIDE now states extended interpretation; severity High → **Medium** |
| T-33 | Insecure output handling | **Renamed** → *Insecure handling of generated output* | Severity **Critical retained** — chain C1 terminates in account takeover |
| T-34 | Malicious instructions via chatbot | **Renamed** → *Delivery of attacker-authored instructions through the assistant* | Reasoning rewritten |
| T-35 | Excessive trust in LLM output | **Modified** | Severity High → **Medium**; P4/P5 named |
| T-36 | Model/provider supply-chain compromise | **Renamed** → *Compromise or substitution of the external model service* | Reasoning rewritten |
| T-37 | Unbounded consumption / cost harvesting | **MERGED** → T-30 | Same attack path and control |
| T-38 | Model inversion / regurgitation | **MERGED** → T-27 | Same exposure and mitigations |

**Reconciliation:** 38 v1.0 threats = 34 retained (of which 20 modified or renamed) + 1 removed (T-13) + 3 merged (T-26, T-37, T-38). Final count **34**.

## A.1 Unused identifiers

T-13, T-26, T-37 and T-38 are **retired** and must not be reassigned. Any coding sheet referencing them should map to the disposition above.

---

*Scenario 1 reference threat model, version 2.0. Frozen for evaluation.*
