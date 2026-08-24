# Scenario 3 — Reference Threat Model, Version 2.0 (Reviewed and Corrected)

**Thesis:** *Threat Modeling of Large Language Model Integrated Applications: A Comparative Evaluation of AI-Assisted and Classical Threat Modeling Approaches.*

**Scenario:** SC-03, LLM-Integrated Account Recovery and Profile Management System
**Version:** 2.0 · **Supersedes:** 1.0 · **Date:** 2026-08-14 · **Status:** frozen for evaluation

## Status of this artifact

This model is **an architecture-grounded baseline threat model created through systematic threat identification.** It is **not** absolute ground truth. It was produced by one analyst applying a documented method to a documented architecture, and a different competent analyst would produce a partially different set. Its value as a baseline rests on the fact that its method, assumptions, inclusion criteria and exclusions are stated and reproducible, and that every threat is traceable to a named component, asset, data flow and trust boundary.

It is the comparison baseline for five treatments: ChatGPT-generated threats, Ollama-generated threats, the Microsoft Threat Modeling Tool, OWASP-based analysis, and ThreMoLIA. Because the baseline is not exhaustive, the scoring protocol retains an explicit **novel valid threat** category, so that a defensible threat produced by a treatment but absent here is recorded as a finding rather than penalised as a false positive.

## Scope of this revision

Version 2.0 is a **correction pass, not a regeneration**. All 42 threats from version 1.0 are preserved with their original identifiers, architecture mappings and reasoning. No threat was added and none was removed. Changes were confined to the five mandated corrections and one consequential distinction clarification.

The unifying principle of this revision is the removal of **assumption-bearing framing**. Three threats previously derived their force from asserting that the architecture *lacks* a component or mechanism — a separate verification source for profile updates, a context assembly and retrieval path for the assistant, and provider-side model management. Each has been reframed to express the same security concern as an **insufficiency of a control that must exist somewhere**, rather than as an assertion about what the architecture does or does not contain. The threats remain equally scoreable, and are now defensible against the objection that they model an architecture other than the one given.

---

# 1. Change Log

## 1.1 Modified threats


### S3-T-13 — Insufficient authorization verification during profile updates

| | |
|---|---|
| **Previous issue** | Threat name and reasoning were framed around the absence of a separate verification source, which assumed a component the architecture does not contain. |
| **Correction applied** | Renamed to 'Insufficient authorization verification during profile updates'. Reasoning replaced with the mandated wording and extended with the DF8, DF9 and TB3 mapping. The threat now expresses insufficiency of the check performed inside P3 and assumes no additional verification component. Severity, STRIDE and architecture mapping unchanged. |
| **Current STRIDE** | Elevation of Privilege |
| **Current severity** | Critical |
| **Current AI/LLM Specific** | No |

### S3-T-30 — Unauthorized inclusion of sensitive account context in AI prompts

| | |
|---|---|
| **Previous issue** | Threat name and reasoning were framed around an undepicted context assembly mechanism, which assumed a retrieval path the architecture does not contain. |
| **Correction applied** | Renamed to 'Unauthorized inclusion of sensitive account context in AI prompts'. Reasoning replaced with the mandated wording and extended with the DF10, DF11, DF12, TB1 and TB5 mapping. The threat now concerns the sufficiency of authorization filtering before the TB5 crossing and assumes no additional retrieval system. Severity, STRIDE and architecture mapping unchanged. |
| **Current STRIDE** | Information Disclosure |
| **Current severity** | Critical |
| **Current AI/LLM Specific** | Yes |

### S3-T-37 — Unsafe handling of generated output rendered to the user

| | |
|---|---|
| **Previous issue** | Severity of Critical overstated the impact, because the assistant performs no privileged account operation: it does not modify accounts, generate recovery tokens, or bypass security controls. |
| **Correction applied** | Severity reduced to High. Reasoning replaced with the mandated wording and extended with the DF13, DF14, DF15, TB5 and TB1 mapping, stating explicitly why the absence of privileged assistant operations bounds the severity. Name, STRIDE and architecture mapping unchanged. |
| **Current STRIDE** | Elevation of Privilege |
| **Current severity** | High |
| **Current AI/LLM Specific** | Yes |

### S3-T-38 — Malicious or incorrect AI-generated recovery guidance influencing user actions

| | |
|---|---|
| **Previous issue** | Threat name presumed that an attacker directly authored the response, which is narrower than the range of causes the architecture supports. |
| **Correction applied** | Renamed to 'Malicious or incorrect AI-generated recovery guidance influencing user actions'. Reasoning replaced with the mandated wording and extended to state the distinction from S3-T-36, which is now drawn by nature of harm rather than by cause: S3-T-36 concerns descriptive accuracy, S3-T-38 concerns guidance that directs a harmful user action. Severity, STRIDE and architecture mapping unchanged. |
| **Current STRIDE** | Spoofing |
| **Current severity** | High |
| **Current AI/LLM Specific** | Yes |

### S3-T-39 — Compromise of external LLM communication endpoint

| | |
|---|---|
| **Previous issue** | Threat name and reasoning covered provider-side model substitution, which is broader than the communication path the architecture depicts. |
| **Correction applied** | Renamed to 'Compromise of external LLM communication endpoint'. Reasoning replaced with the mandated wording and extended with the DF12, DF13, DF15 and TB5 mapping. Scope narrowed to the depicted communication endpoint and path, with the model-substitution case removed. Severity, STRIDE and architecture mapping unchanged. |
| **Current STRIDE** | Spoofing |
| **Current severity** | High |
| **Current AI/LLM Specific** | Yes |

### S3-T-36 — Hallucinated account recovery guidance

| | |
|---|---|
| **Previous issue** | After the S3-T-38 rename widened that threat to include incorrect as well as manipulated guidance, the boundary between the two became implicit and created a duplication risk during coding. |
| **Correction applied** | Reasoning extended to state the distinction explicitly: S3-T-36 is a descriptive accuracy failure about system behaviour, S3-T-38 is guidance directing a harmful user action. Different controls stated for each. Name, severity, STRIDE and architecture mapping unchanged. |
| **Current STRIDE** | Tampering |
| **Current severity** | Medium |
| **Current AI/LLM Specific** | Yes |

## 1.2 Removed threats

**None.** No threat in version 1.0 failed the applicability or traceability tests, so no removals were required. All 42 identifiers remain in use, and none has been retired or reassigned.

Every threat was re-tested against the inclusion criterion — that it must trace to at least one named component, one named asset, one named data flow and one named trust boundary — and all 42 passed. This was verified programmatically rather than by inspection.

## 1.3 STRIDE normalisation

**No change required.** The STRIDE field already contained exactly one primary category for every threat in version 1.0, drawn only from the six permitted values, and this was re-verified programmatically against the final model. Because each threat carries a single category, the STRIDE counts sum to the total of 42.

Where a threat has a genuine secondary effect, that effect is recorded in the Security Impact field rather than in the STRIDE field, so no analytical content is lost to the single-category constraint.

### Note on STRIDE and LLM-specific threats

> Some LLM-specific threats do not map perfectly to classical STRIDE categories because STRIDE was originally designed for traditional software systems.

Two threats are mapped under extended interpretation and state this in their reasoning fields. **S3-T-36** (hallucinated account recovery guidance) and **S3-T-40** (excessive trust in assistant guidance) are both mapped to *Tampering*, applied respectively to the integrity of generated information and to reliance on unverified generated content. In neither case is a trust boundary crossed illegitimately, is a data flow modified by an attacker, or does any component behave other than as engineered, yet the security outcome is negative. This mapping difficulty is itself a reportable finding about the limits of applying STRIDE alone to LLM-integrated systems.

## 1.4 Duplication and differentiation review

Similar threats were examined and **differentiated rather than merged**, with the distinction stated inside each reasoning field so that coders cannot inadvertently treat one candidate threat as covering both.

| Pair | Decision | Basis for the distinction |
|---|---|---|
| S3-T-36 vs S3-T-38 | **Both retained** | Re-examined in this revision, because the S3-T-38 rename widened it to include incorrect as well as manipulated guidance. The boundary is now drawn by **nature of harm**: S3-T-36 is a descriptive accuracy failure, where the response misdescribes how the recovery process implemented in P2 behaves. S3-T-38 is guidance that directs the user to take a specific harmful action disclosing verification material or recovery tokens. Different controls: accuracy verification against authoritative procedure, versus output-side destination and action restriction at P4. |
| S3-T-33 vs S3-T-34 | **Both retained** (unchanged) | S3-T-33 is the outbound transmission risk on DF12, controllable by the platform through minimisation at P4 and P5. S3-T-34 is the provider's subsequent retention and processing of what it received, addressable only contractually. |
| S3-T-13 vs S3-T-14 | **Both retained** (unchanged) | S3-T-13 concerns the sufficiency of the authorization check performed in P3. S3-T-14 concerns the binding of the write on DF9 to the requesting identity, and remains applicable even if the check in S3-T-13 is strengthened. |
| S3-T-01 / S3-T-02 / S3-T-10 / S3-T-11 | **All retained** (unchanged) | Four mechanically different failures of the same verification control: unenforced ordering, insufficient factor strength, query manipulation, and exhaustive search. Four different remediations. |
| S3-T-03 / S3-T-04 / S3-T-05 / S3-T-06 | **All retained** (unchanged) | The same asset at four lifecycle stages: generation entropy, transport exposure, consumption marking, expiry. Four different controls. |

---

# 2. Quality Review Results

All checks executed programmatically against the final model.

| # | Check | Result |
|---|---|---|
| 1 | No assumptions about missing components | **Pass.** The three assumption-bearing threats (S3-T-13, S3-T-30, S3-T-39) were reframed as control-insufficiency statements. Verified that no corrected threat asserts an undepicted component, flow or mechanism. |
| 2 | No assumptions that the AI assistant has privileged access | **Pass.** No threat attributes identity verification, token generation, account modification, profile update or recovery-control bypass to P4. The only occurrences of such phrasing are explicit denials, most notably in the corrected S3-T-37. |
| 3 | No assumptions that the LLM can modify accounts | **Pass.** Verified by pattern matching across every field of all 42 threats, excluding denials. |
| 4 | No assumptions that recovery tokens can be generated by the AI assistant | **Pass.** Token generation is attributed solely to P2 on DF5 throughout. S3-T-38 concerns the assistant inducing a *user* to disclose a token, not the assistant producing one. |
| 5 | Traditional and LLM-specific threats clearly separated | **Pass.** The AI/LLM Specific flag is binary. All 13 LLM-Specific category threats are flagged Yes; the 27 threats in the three traditional categories are flagged No. The two exceptions are S3-T-28 and S3-T-29, categorised under Availability but flagged Yes, because both exist solely because of the external model dependency across TB5. |
| 6 | Severity ratings realistic | **Pass.** Recalibrated — see 2.1. |
| 7 | Similar threats differentiated rather than duplicated | **Pass.** Five clusters examined; all retained with explicit in-field distinctions; no duplicate identifiers or threat names. |

## 2.1 Severity calibration

One severity changed in this revision: **S3-T-37 from Critical to High**, because the assistant performs no privileged account operation. It does not modify accounts, generate recovery tokens, or bypass security controls, so the impact of unsafe generated output depends on subsequent user or browser behaviour rather than on any privileged action the assistant itself performs.

This changes the Critical count from 9 to 8 and makes the assistant path carry exactly one Critical threat, **S3-T-30**, which is retained at Critical because its impact is direct disclosure of another user's account information rather than influence over user behaviour.

The calibration rules established during construction remain in force and were re-verified:

- **Recovery-path compromise is Critical** where it yields account takeover without legitimate verification: S3-T-01, S3-T-02, S3-T-03, S3-T-10.
- **S3-T-20 is Critical** on an asymmetry specific to recovery systems: disclosed identity verification data cannot be rotated the way a credential can, so the impact is permanent.
- **S3-T-15 is Critical** because it converts a limited profile-write capability into full account takeover through the DS1 coupling between TB3 and TB2.
- **The assistant holds no authority**, so injection threats S3-T-31 and S3-T-32 remain High, and S3-T-37 now joins them at High.

---

# 3. Final Statistics


**Total threats: 42**
**Traditional threats: 27**
**AI/LLM-specific threats: 15**

| Classification | Count | Share |
|---|---|---|
| Traditional (AI/LLM Specific = No) | 27 | 64.3 % |
| AI/LLM-specific (AI/LLM Specific = Yes) | 15 | 35.7 % |
| **Total** | **42** | **100.0 %** |

- **Traditional:** S3-T-01, S3-T-02, S3-T-03, S3-T-04, S3-T-05, S3-T-06, S3-T-07, S3-T-08, S3-T-09, S3-T-10, S3-T-11, S3-T-12, S3-T-13, S3-T-14, S3-T-15, S3-T-16, S3-T-17, S3-T-18, S3-T-19, S3-T-20, S3-T-21, S3-T-22, S3-T-23, S3-T-24, S3-T-25, S3-T-26, S3-T-27
- **AI/LLM-specific:** S3-T-28, S3-T-29, S3-T-30, S3-T-31, S3-T-32, S3-T-33, S3-T-34, S3-T-35, S3-T-36, S3-T-37, S3-T-38, S3-T-39, S3-T-40, S3-T-41, S3-T-42

## 3.1 By primary category

| Category | Total | Traditional | AI/LLM-specific |
|---|---|---|---|
| Account Recovery Security | 12 | 12 | 0 |
| Profile Management Security | 7 | 7 | 0 |
| Data Protection | 6 | 6 | 0 |
| Availability | 4 | 2 | 2 |
| LLM-Specific | 13 | 0 | 13 |
| **Total** | **42** | **27** | **15** |

## 3.2 By severity

| Severity | Traditional | AI/LLM-specific | Total |
|---|---|---|---|
| Critical | 7 | 1 | 8 |
| High | 13 | 6 | 19 |
| Medium | 7 | 8 | 15 |
| **Total** | **27** | **15** | **42** |

The eight Critical threats are S3-T-01, S3-T-02, S3-T-03, S3-T-10 (recovery bypass and impersonation), S3-T-13 and S3-T-15 (profile path, including the conversion into account takeover), S3-T-20 (non-rotatable verification data) and S3-T-30 (unauthorized account context in prompts). Seven are traditional and one is AI/LLM-specific, which reflects that the highest-impact outcomes in this architecture are reached through the recovery and profile paths rather than through the advisory assistant.

## 3.3 By STRIDE category (one primary category per threat)

| STRIDE category | Threat IDs | Count |
|---|---|---|
| Spoofing | S3-T-02, S3-T-03, S3-T-05, S3-T-09, S3-T-11, S3-T-38, S3-T-39 | 7 |
| Tampering | S3-T-07, S3-T-10, S3-T-15, S3-T-17, S3-T-31, S3-T-32, S3-T-36, S3-T-40 | 8 |
| Repudiation | S3-T-12, S3-T-18 | 2 |
| Information Disclosure | S3-T-04, S3-T-08, S3-T-20, S3-T-21, S3-T-22, S3-T-23, S3-T-24, S3-T-25, S3-T-30, S3-T-33, S3-T-34, S3-T-35, S3-T-41, S3-T-42 | 14 |
| Denial of Service | S3-T-26, S3-T-27, S3-T-28, S3-T-29 | 4 |
| Elevation of Privilege | S3-T-01, S3-T-06, S3-T-13, S3-T-14, S3-T-16, S3-T-19, S3-T-37 | 7 |
| **Total** | | **42** |

## 3.4 By threat source

| Threat Source | Count | Share |
|---|---|---|
| STRIDE-derived | 7 | 16.7 % |
| OWASP Application Security | 12 | 28.6 % |
| OWASP LLM Security | 10 | 23.8 % |
| Research-based LLM Threat | 3 | 7.1 % |
| Architecture-specific analysis | 10 | 23.8 % |
| **Total** | **42** | **100.0 %** |

| Source | Threat IDs |
|---|---|
| STRIDE-derived | S3-T-03, S3-T-04, S3-T-10, S3-T-20, S3-T-21, S3-T-23, S3-T-26 |
| OWASP Application Security | S3-T-02, S3-T-08, S3-T-09, S3-T-11, S3-T-12, S3-T-14, S3-T-16, S3-T-17, S3-T-19, S3-T-22, S3-T-24, S3-T-25 |
| OWASP LLM Security | S3-T-29, S3-T-31, S3-T-32, S3-T-33, S3-T-34, S3-T-35, S3-T-36, S3-T-37, S3-T-39, S3-T-40 |
| Research-based LLM Threat | S3-T-38, S3-T-41, S3-T-42 |
| Architecture-specific analysis | S3-T-01, S3-T-05, S3-T-06, S3-T-07, S3-T-13, S3-T-15, S3-T-18, S3-T-27, S3-T-28, S3-T-30 |

The **10 architecture-specific threats** remain the most diagnostic items in the evaluation, because they cannot be produced by pattern-matching a framework to a component list. Note that after this revision they are architecture-specific in the sense that they were **derived from analysing this diagram**, not in the sense that they assert absences in it: S3-T-13 and S3-T-30 were discovered through structural analysis of the flow topology but are now expressed as control-insufficiency statements that stand independently of that discovery route. Performance on this subset should still be reported separately.

---

# 4. Corrected Reference Threat Model

The full model is provided in `Scenario3_Reference_Threat_Model_v2.xlsx` and `Scenario3_Reference_Threat_Model_v2.csv`, using exactly the required columns:

`Threat ID` · `Threat Name` · `Threat Source` · `Affected Component` · `Affected Asset` · `Affected Data Flow` · `Affected Trust Boundary` · `Attack Vector or Exploitation Path` · `Security Impact` · `Evidence / Reasoning` · `AI/LLM Specific` · `STRIDE Category` · `Severity`

A fourteenth column, `Primary Category`, is appended for filtering during analysis and may be dropped without affecting the required schema.

## 4.1 Threat index

| ID | Threat Name | STRIDE | AI/LLM | Severity | Source |
|---|---|---|---|---|---|
| S3-T-01 | Account recovery bypass through unenforced verification sequencing | Elevation of Privilege | No | Critical | Architecture-specific analysis |
| S3-T-02 | Weak identity verification permitting impersonation during recovery | Spoofing | No | Critical | OWASP Application Security |
| S3-T-03 | Recovery token prediction due to insufficient entropy | Spoofing | No | Critical | STRIDE-derived |
| S3-T-04 | Recovery token theft in transit or from the user environment | Information Disclosure | No | High | STRIDE-derived |
| S3-T-05 | Recovery token replay due to absent single-use enforcement | Spoofing | No | High | Architecture-specific analysis |
| S3-T-06 | Unbounded recovery token lifetime due to missing expiry and invalidation | Elevation of Privilege | No | High | Architecture-specific analysis |
| S3-T-07 | Manipulation of the recovery outcome carried on the combined result flow | Tampering | No | High | Architecture-specific analysis |
| S3-T-08 | Account enumeration through differential recovery responses | Information Disclosure | No | Medium | OWASP Application Security |
| S3-T-09 | Recovery token issued to an attacker-controlled destination | Spoofing | No | High | OWASP Application Security |
| S3-T-10 | Injection into the identity verification query | Tampering | No | Critical | STRIDE-derived |
| S3-T-11 | Brute-force and automated guessing of verification factors and tokens | Spoofing | No | High | OWASP Application Security |
| S3-T-12 | Insufficient logging of recovery decisions and token issuance | Repudiation | No | Medium | OWASP Application Security |
| S3-T-13 | Insufficient authorization verification during profile updates | Elevation of Privilege | No | Critical | Architecture-specific analysis |
| S3-T-14 | Unauthorized modification of another user's profile through identifier substitution | Elevation of Privilege | No | High | OWASP Application Security |
| S3-T-15 | Modification of security-relevant profile attributes to enable later recovery | Tampering | No | Critical | Architecture-specific analysis |
| S3-T-16 | Mass assignment of restricted fields through the profile update request | Elevation of Privilege | No | High | OWASP Application Security |
| S3-T-17 | Insufficient validation of profile content written to storage | Tampering | No | High | OWASP Application Security |
| S3-T-18 | Profile changes performed without confirmation or notification to the account holder | Repudiation | No | Medium | Architecture-specific analysis |
| S3-T-19 | Unauthorized access to backend recovery and profile functionality | Elevation of Privilege | No | High | OWASP Application Security |
| S3-T-20 | Disclosure of identity verification data held in the user database | Information Disclosure | No | Critical | STRIDE-derived |
| S3-T-21 | Disclosure of active recovery tokens held in token storage | Information Disclosure | No | High | STRIDE-derived |
| S3-T-22 | Excessive disclosure of account and recovery information in responses | Information Disclosure | No | Medium | OWASP Application Security |
| S3-T-23 | Interception of recovery and profile data in transit across the user boundary | Information Disclosure | No | High | STRIDE-derived |
| S3-T-24 | Exposure of recovery details through verbose errors and diagnostics | Information Disclosure | No | Medium | OWASP Application Security |
| S3-T-25 | Exposure of credentials used for external LLM communication | Information Disclosure | No | High | OWASP Application Security |
| S3-T-26 | Denial of service against the account recovery service | Denial of Service | No | Medium | STRIDE-derived |
| S3-T-27 | Recovery token generation abuse and token storage exhaustion | Denial of Service | No | Medium | Architecture-specific analysis |
| S3-T-28 | Loss of assistant availability through external provider disruption | Denial of Service | Yes | Medium | Architecture-specific analysis |
| S3-T-29 | Unbounded consumption of the assistant processing path | Denial of Service | Yes | Medium | OWASP LLM Security |
| S3-T-30 | Unauthorized inclusion of sensitive account context in AI prompts | Information Disclosure | Yes | Critical | Architecture-specific analysis |
| S3-T-31 | Direct prompt injection through the account-related question | Tampering | Yes | High | OWASP LLM Security |
| S3-T-32 | Indirect prompt injection through attacker-controlled profile content | Tampering | Yes | High | OWASP LLM Security |
| S3-T-33 | Disclosure of account and recovery information to the external LLM provider | Information Disclosure | Yes | High | OWASP LLM Security |
| S3-T-34 | Provider-side retention or reuse of transmitted context | Information Disclosure | Yes | Medium | OWASP LLM Security |
| S3-T-35 | Extraction of assistant instructions through elicitation | Information Disclosure | Yes | Medium | OWASP LLM Security |
| S3-T-36 | Hallucinated account recovery guidance | Tampering | Yes | Medium | OWASP LLM Security |
| S3-T-37 | Unsafe handling of generated output rendered to the user | Elevation of Privilege | Yes | High | OWASP LLM Security |
| S3-T-38 | Malicious or incorrect AI-generated recovery guidance influencing user actions | Spoofing | Yes | High | Research-based LLM Threat |
| S3-T-39 | Compromise of external LLM communication endpoint | Spoofing | Yes | High | OWASP LLM Security |
| S3-T-40 | Excessive trust in assistant guidance during account recovery | Tampering | Yes | Medium | OWASP LLM Security |
| S3-T-41 | Cross-user context leakage within the assistant pipeline | Information Disclosure | Yes | Medium | Research-based LLM Threat |
| S3-T-42 | Assistant used to enumerate account existence and recovery requirements | Information Disclosure | Yes | Medium | Research-based LLM Threat |

---

*Scenario 3 reference threat model, version 2.0. Architecture-grounded baseline threat model created through systematic threat identification. Not absolute ground truth. Frozen for evaluation.*
