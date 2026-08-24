# Scenario 2 — Reference Threat Model, Version 2.0 (Reviewed and Corrected)

**Thesis:** *Threat Modeling of Large Language Model Integrated Applications: A Comparative Evaluation of AI-Assisted and Classical Threat Modeling Approaches.*

**Scenario:** SC-02, LLM-Integrated Secure File Management System
**Version:** 2.0 · **Supersedes:** 1.0 · **Date:** 2026-08-14 · **Status:** frozen for evaluation

## Status of this artifact

This model is **an architecture-grounded baseline threat model created through systematic threat identification.** It is **not** absolute ground truth. It is the product of one analyst applying a documented method to a documented architecture, and a different competent analyst would produce a partially different set. Its value as a baseline rests on the fact that its method, assumptions, inclusion criteria and exclusions are stated and reproducible, and that every threat is traceable to a named component, asset, data flow and trust boundary.

It is the comparison baseline for four treatments: ChatGPT-generated threats, Ollama-generated threats, the Microsoft Threat Modeling Tool, and ThreMoLIA. Because the baseline is not exhaustive, the scoring protocol retains an explicit **novel valid threat** category so that a defensible threat produced by a treatment but absent here is recorded as a finding rather than penalised as a false positive.

## Scope of this revision

Version 2.0 is a **correction pass, not a regeneration**. All 41 threats from version 1.0 are preserved with their original identifiers, architecture mapping and reasoning. No threat was added and none was removed. Changes were confined to the four mandated corrections, STRIDE normalisation, and two pairs of overlap clarifications.

---

# 1. Change Log

## 1.1 Modified threats


### S2-T-10 — Unauthorized access to backend file management functionality

| | |
|---|---|
| **Previous issue** | Threat name and reasoning implied that P2 is directly exposed externally, which the architecture does not state. |
| **Correction made** | Renamed to 'Unauthorized access to backend file management functionality'. Reasoning replaced with the mandated wording and now expresses the threat as a requirement that P2 enforce its own authentication and authorization on DF2 and DF5, explicitly noting that the architecture does not state whether P2 is reachable other than through P1. STRIDE normalized to Elevation of Privilege. |
| **Current STRIDE** | Elevation of Privilege |
| **Current severity** | High |

### S2-T-19 — Unauthorized access to storage repository due to insufficient isolation

| | |
|---|---|
| **Previous issue** | Threat name and reasoning implied that DS1 is publicly accessible, which the architecture does not state. |
| **Correction made** | Renamed to 'Unauthorized access to storage repository due to insufficient isolation'. Reasoning replaced with the mandated wording and now explicitly disclaims any assumption of public reachability, framing the threat as isolation strength between Zone B services and the Zone C storage component. STRIDE normalized to Information Disclosure. |
| **Current STRIDE** | Information Disclosure |
| **Current severity** | Critical |

### S2-T-27 — Exposure of credentials used for external LLM communication

| | |
|---|---|
| **Previous issue** | Threat named a specific 'API credential' and implied a credential management component that the architecture does not contain. |
| **Correction made** | Renamed to 'Exposure of credentials used for external LLM communication'. Reasoning replaced with the mandated wording; attack vector and affected asset generalised so that no particular credential storage implementation is assumed. STRIDE normalized to Information Disclosure. |
| **Current STRIDE** | Information Disclosure |
| **Current severity** | High |

### S2-T-39 — Provider-side retention or reuse of transmitted document context

| | |
|---|---|
| **Previous issue** | Severity of High overstated a governance and compliance exposure as though it were direct system compromise. |
| **Correction made** | Severity reduced to Medium. Reasoning replaced with the mandated wording and extended to state explicitly how the threat is distinguished from S2-T-31: S2-T-31 is the outbound act by the application, S2-T-39 is the provider's subsequent handling of what it received. |
| **Current STRIDE** | Information Disclosure |
| **Current severity** | Medium |

### S2-T-31 — Disclosure of sensitive document content to the external LLM provider

| | |
|---|---|
| **Previous issue** | Overlap with S2-T-39 was implied but not stated, risking both being coded as one concern. |
| **Correction made** | Reasoning extended with an explicit distinction: S2-T-31 is the outbound transmission risk controllable at P3 and P4; S2-T-39 is the provider-side retention risk that the platform cannot control technically. No change to name, severity or mapping. |
| **Current STRIDE** | Information Disclosure |
| **Current severity** | High |

### S2-T-11 — Broken object-level authorization allowing access to another user's file

| | |
|---|---|
| **Previous issue** | Distinction from S2-T-12 was implicit, creating a duplication risk during coding. |
| **Correction made** | Renamed to 'Broken object-level authorization allowing access to another user's file'. Reasoning extended to state the distinction explicitly: the check is performed but is not bound to the requested object. STRIDE normalized to Information Disclosure. |
| **Current STRIDE** | Information Disclosure |
| **Current severity** | Critical |

### S2-T-12 — File retrieval performed without a completed authorization check

| | |
|---|---|
| **Previous issue** | Distinction from S2-T-11 was implicit, creating a duplication risk during coding. |
| **Correction made** | Renamed to 'File retrieval performed without a completed authorization check'. Reasoning extended to state the distinction explicitly: the permission model may be correct but no valid decision exists when the DF8 lookup is issued. STRIDE remains Elevation of Privilege. |
| **Current STRIDE** | Elevation of Privilege |
| **Current severity** | Critical |

## 1.2 STRIDE normalisation

The STRIDE field previously contained compound values for 17 threats. Each now contains exactly one primary category drawn from the six permitted values. Where a threat has a genuine secondary effect, that effect is retained in the Security Impact field rather than in the STRIDE field, so no analytical content was lost.

| Threat ID | Previous STRIDE value | Normalised value | Basis for selecting the primary category |
|---|---|---|---|
| S2-T-01 | Elevation of Privilege / Tampering | **Elevation of Privilege** | Code execution in the application environment is a privilege gain; tampering with stored content is the means, not the outcome. |
| S2-T-09 | Tampering / Elevation of Privilege | **Tampering** | The act is manipulation of metadata and permission attributes; the privilege consequence is recorded in the impact field. |
| S2-T-10 | Spoofing / Elevation of Privilege | **Elevation of Privilege** | The outcome is interaction with protected operations the actor is not entitled to invoke. |
| S2-T-11 | Information Disclosure / Elevation of Privilege | **Information Disclosure** | The realised outcome is reading another user's document; the authorization defect is the mechanism. |
| S2-T-14 | Tampering / Elevation of Privilege | **Tampering** | The act is manipulation of the meaning of the authorization query carried on DF6. |
| S2-T-15 | Tampering / Spoofing | **Tampering** | The act is alteration of the decision carried on DF7. |
| S2-T-19 | Information Disclosure / Tampering | **Information Disclosure** | The dominant outcome is exposure of stored files and metadata. |
| S2-T-20 | Tampering / Denial of Service | **Tampering** | The act is unauthorized alteration or destruction of stored objects. |
| S2-T-23 | Information Disclosure / Tampering | **Information Disclosure** | The dominant outcome on DF1, DF4 and DF10 is observation of document content in transit. |
| S2-T-24 | Tampering / Elevation of Privilege | **Elevation of Privilege** | Script executing in the victim's authenticated browser context acts with the victim's privileges. |
| S2-T-27 | Information Disclosure / Spoofing | **Information Disclosure** | The act is exposure of authentication material. |
| S2-T-28 | Elevation of Privilege / Information Disclosure | **Elevation of Privilege** | The defect is an absent authorization consultation on the assistant path, mapped consistently with S2-T-12. |
| S2-T-34 | Tampering / Elevation of Privilege | **Elevation of Privilege** | Mapped consistently with S2-T-24: rendered active content executes with the victim's privileges. |
| S2-T-35 | Tampering (extended interpretation, long form) | **Tampering** | Extended interpretation: integrity of generated information. Field now contains the single category only. |
| S2-T-36 | Spoofing / Tampering | **Spoofing** | The response impersonates authoritative guidance issued by the platform. |
| S2-T-37 | Tampering (extended interpretation, long form) | **Tampering** | Extended interpretation: reliance on unverified generated content. Field now contains the single category only. |
| S2-T-38 | Spoofing / Tampering | **Spoofing** | The endpoint or model the platform believes it is communicating with is impersonated or substituted. |

**Note on STRIDE and AI-specific threats.**

> Some LLM-specific threats require extended interpretation because classical STRIDE was designed for traditional software systems.

Two threats are mapped under extended interpretation, and each states this in its reasoning field. **S2-T-35** (hallucinated document summaries) and **S2-T-37** (excessive trust in assistant output) are mapped to *Tampering*, applied to the integrity of generated information and to reliance on unverified generated content respectively. In neither case is a boundary crossed illegitimately, is a data flow modified by an attacker, or does any component behave other than as engineered, yet the security outcome is negative. This mapping difficulty is itself a reportable finding about the limits of applying STRIDE alone to LLM-integrated systems.

## 1.3 Overlap and duplication review

Two pairs were examined for redundancy. Both were **retained as distinct**, with the distinction now stated explicitly inside each reasoning field so that coders cannot inadvertently treat one candidate threat as covering both.

| Pair | Decision | Basis for the distinction |
|---|---|---|
| S2-T-11 vs S2-T-12 | **Both retained** | S2-T-11: the authorization check is performed but is not bound to the specific object identified in the DF5 request, so a decision valid for the requester is applied to an object they do not own. S2-T-12: the object-level permission model may be entirely correct, but no valid decision exists when P2 issues the DF8 lookup across TB2. Different mechanisms, different remediations. |
| S2-T-31 vs S2-T-39 | **Both retained** | S2-T-31: the risk arises when the application transmits sensitive information outward on DF13 across TB4, and is controllable by the platform through context minimisation at P3 and P4. S2-T-39: the risk arises because the external provider may retain and process information it has already received, which the platform cannot control by technical means. Different origin, different available control. |

Three merges performed during the original construction remain in force and are recorded here for completeness: context over-selection is folded into S2-T-31; cost harvesting is folded into S2-T-41; regurgitation to other users is folded into S2-T-39.

## 1.4 Removed threats

**None.** No threat in version 1.0 failed the applicability or traceability tests, so no removals were required. All 41 identifiers remain in use and none has been retired or reassigned.

Every threat was re-tested against the inclusion criterion — that it must trace to at least one named component, one named asset, one named data flow and one named trust boundary — and all 41 passed. This was verified programmatically rather than by inspection.

---

# 2. Quality Check Results

All seven checks were executed programmatically against the final model.

| # | Check | Result |
|---|---|---|
| 1 | No threat assumes unavailable components | **Pass.** Every component, data flow and trust boundary token appearing in any field was validated against the architecture inventory (P1–P4, DS1–DS2, EE1–EE2, DF1–DF16, TB1–TB4). No out-of-inventory references. |
| 2 | No threat assumes the AI assistant can modify files | **Pass.** No threat attributes a write, modification or deletion operation to P3 or to the model. |
| 3 | No threat assumes the LLM can bypass authorization | **Pass.** S2-T-28 is the sole threat concerning the assistant and authorization, and it is framed as an *absence of a depicted authorization consultation on the P3 path*, not as the model overriding a control that exists. |
| 4 | No threat assumes storage is publicly accessible | **Pass.** The only occurrences of public-reachability language are explicit denials, most notably in the corrected S2-T-19, which states that the architecture does not describe DS1 as publicly reachable and that the threat makes no such assumption. |
| 5 | Traditional and LLM-specific threats clearly separated | **Pass.** The AI/LLM Specific flag is binary and aligns exactly with the primary category: all 14 LLM-Specific threats are flagged Yes, and all 27 threats in the three traditional categories are flagged No. |
| 6 | Severity ratings realistic | **Pass.** Recalibrated where required — see 2.1. |
| 7 | Duplicates merged or clearly differentiated | **Pass.** Two pairs examined and retained with explicit in-field distinctions; three earlier merges remain in force; no duplicate identifiers or threat names. |

## 2.1 Severity calibration

One severity was changed in this revision: **S2-T-39 from High to Medium**, on the basis that provider-side retention represents privacy, governance and compliance exposure rather than direct system compromise, and is realised through provider policy rather than through an exploitable defect in the architecture.

The two calibration rules established during construction remain in force and were re-verified:

- **The assistant holds no authority**, so injection threats S2-T-29 and S2-T-30 remain High rather than Critical; their direct impact is bounded by disclosure of prompt content and manipulation of returned text.
- **Except where a chain escapes that bound.** S2-T-34 remains Critical because rendering attacker-controlled active content in the authenticated browser context reaches full session compromise, and S2-T-28 remains Critical because it reaches other users' document contents. Neither requires the model to hold any privilege.

---

# 3. Final Statistics


**Total threats: 41**
**Traditional threats: 27**
**AI/LLM-specific threats: 14**

| Classification | Count | Share |
|---|---|---|
| Traditional (AI/LLM Specific = No) | 27 | 65.9 % |
| AI/LLM-specific (AI/LLM Specific = Yes) | 14 | 34.1 % |
| **Total** | **41** | **100.0 %** |

- **Traditional:** S2-T-01, S2-T-02, S2-T-03, S2-T-04, S2-T-05, S2-T-06, S2-T-07, S2-T-08, S2-T-09, S2-T-10, S2-T-11, S2-T-12, S2-T-13, S2-T-14, S2-T-15, S2-T-16, S2-T-17, S2-T-18, S2-T-19, S2-T-20, S2-T-21, S2-T-22, S2-T-23, S2-T-24, S2-T-25, S2-T-26, S2-T-27
- **AI/LLM-specific:** S2-T-28, S2-T-29, S2-T-30, S2-T-31, S2-T-32, S2-T-33, S2-T-34, S2-T-35, S2-T-36, S2-T-37, S2-T-38, S2-T-39, S2-T-40, S2-T-41

## 3.1 By primary category

| Category | Total | Traditional | AI/LLM-specific |
|---|---|---|---|
| File Upload Security | 10 | 10 | 0 |
| Authorization | 8 | 8 | 0 |
| Data Security | 9 | 9 | 0 |
| LLM-Specific | 14 | 0 | 14 |
| **Total** | **41** | **27** | **14** |

## 3.2 By severity

| Severity | Traditional | AI/LLM-specific | Total |
|---|---|---|---|
| Critical | 5 | 2 | 7 |
| High | 15 | 5 | 20 |
| Medium | 7 | 7 | 14 |
| **Total** | **27** | **14** | **41** |

## 3.3 By STRIDE category (single primary category per threat)

| STRIDE category | Threat IDs | Count |
|---|---|---|
| Spoofing | S2-T-18, S2-T-36, S2-T-38 | 3 |
| Tampering | S2-T-02, S2-T-03, S2-T-04, S2-T-05, S2-T-08, S2-T-09, S2-T-14, S2-T-15, S2-T-16, S2-T-20, S2-T-29, S2-T-30, S2-T-35, S2-T-37 | 14 |
| Repudiation | S2-T-25 | 1 |
| Information Disclosure | S2-T-11, S2-T-17, S2-T-19, S2-T-21, S2-T-22, S2-T-23, S2-T-27, S2-T-31, S2-T-32, S2-T-33, S2-T-39 | 11 |
| Denial of Service | S2-T-06, S2-T-07, S2-T-26, S2-T-40, S2-T-41 | 5 |
| Elevation of Privilege | S2-T-01, S2-T-10, S2-T-12, S2-T-13, S2-T-24, S2-T-28, S2-T-34 | 7 |
| **Total** | | **41** |

Because each threat now carries exactly one primary category, these counts sum to the total, which was not the case in version 1.0. Tampering and Information Disclosure dominate, which is a structural property of a platform whose assets are stored documents and which exports document content across an uncontrolled boundary by design.

## 3.4 By threat source

| Threat Source | Count | Share |
|---|---|---|
| STRIDE-derived | 8 | 19.5 % |
| OWASP Application Security | 12 | 29.3 % |
| OWASP LLM Security | 10 | 24.4 % |
| Research-based LLM Threat | 2 | 4.9 % |
| Architecture-specific analysis | 9 | 22.0 % |
| **Total** | **41** | **100.0 %** |

The **9 architecture-specific threats** — S2-T-01, S2-T-05, S2-T-08, S2-T-09, S2-T-12, S2-T-19, S2-T-22, S2-T-28, S2-T-40 — remain the most diagnostic items in the evaluation, because they cannot be produced by pattern-matching a framework to a component list. Three are justified by *absent* elements: S2-T-28 (no depicted flow from P3 to P2 or DS2), S2-T-12 (no enforced ordering between the DF7 decision and the DF8 lookup), and in part S2-T-05 (no ownership check depicted on the DF3 write path). Performance on this subset should be reported separately.

---

# 4. Corrected Reference Threat Model

The full model is provided in `Scenario2_Reference_Threat_Model_v2.xlsx` and `Scenario2_Reference_Threat_Model_v2.csv`, using exactly the required columns:

`Threat ID` · `Threat Name` · `Threat Source` · `Affected Component` · `Affected Asset` · `Affected Data Flow` · `Affected Trust Boundary` · `Attack Vector or Exploitation Path` · `Security Impact` · `Evidence / Reasoning` · `AI/LLM Specific` · `STRIDE Category` · `Severity`

A fourteenth column, `Primary Category`, is appended for filtering during analysis and may be dropped without affecting the required schema.

## 4.1 Threat index

| ID | Threat Name | STRIDE | AI/LLM | Severity | Source |
|---|---|---|---|---|---|
| S2-T-01 | Malicious file upload leading to server-side code execution | Elevation of Privilege | No | Critical | Architecture-specific analysis |
| S2-T-02 | File type validation bypass through declared-property manipulation | Tampering | No | High | OWASP Application Security |
| S2-T-03 | Malware upload and distribution to other platform users | Tampering | No | High | OWASP Application Security |
| S2-T-04 | Path traversal during the storage write operation | Tampering | No | High | OWASP Application Security |
| S2-T-05 | Overwriting another user's stored file through identifier collision | Tampering | No | High | Architecture-specific analysis |
| S2-T-06 | Resource exhaustion in file validation through crafted content | Denial of Service | No | Medium | OWASP Application Security |
| S2-T-07 | Storage exhaustion through unbounded upload volume | Denial of Service | No | Medium | STRIDE-derived |
| S2-T-08 | Time-of-check to time-of-use gap between validation and storage | Tampering | No | Medium | Architecture-specific analysis |
| S2-T-09 | Metadata tampering to assign attacker-chosen ownership or permissions | Tampering | No | High | Architecture-specific analysis |
| S2-T-10 | Unauthorized access to backend file management functionality | Elevation of Privilege | No | High | OWASP Application Security |
| S2-T-11 | Broken object-level authorization allowing access to another user's file | Information Disclosure | No | Critical | OWASP Application Security |
| S2-T-12 | File retrieval performed without a completed authorization check | Elevation of Privilege | No | Critical | Architecture-specific analysis |
| S2-T-13 | Vertical privilege escalation through permission record manipulation | Elevation of Privilege | No | High | OWASP Application Security |
| S2-T-14 | Injection into the authorization verification query | Tampering | No | Critical | STRIDE-derived |
| S2-T-15 | Tampering with the authorization decision in transit | Tampering | No | High | STRIDE-derived |
| S2-T-16 | Unauthorized modification of permission records at rest | Tampering | No | High | STRIDE-derived |
| S2-T-17 | File identifier enumeration through response differentiation | Information Disclosure | No | Medium | OWASP Application Security |
| S2-T-18 | Identity spoofing on the authenticated file access request | Spoofing | No | High | STRIDE-derived |
| S2-T-19 | Unauthorized access to storage repository due to insufficient isolation | Information Disclosure | No | Critical | Architecture-specific analysis |
| S2-T-20 | Unauthorized modification or deletion of stored files | Tampering | No | High | STRIDE-derived |
| S2-T-21 | Disclosure of stored documents through absent protection at rest | Information Disclosure | No | High | OWASP Application Security |
| S2-T-22 | Excessive file metadata disclosure in retrieval responses | Information Disclosure | No | Medium | Architecture-specific analysis |
| S2-T-23 | Interception of document content in transit across the user boundary | Information Disclosure | No | High | STRIDE-derived |
| S2-T-24 | Stored cross-site scripting through uploaded file content | Elevation of Privilege | No | High | OWASP Application Security |
| S2-T-25 | Insufficient logging of file access and authorization events | Repudiation | No | Medium | OWASP Application Security |
| S2-T-26 | Denial of service against the File Management Service | Denial of Service | No | Medium | STRIDE-derived |
| S2-T-27 | Exposure of credentials used for external LLM communication | Information Disclosure | No | High | OWASP Application Security |
| S2-T-28 | Unauthorized document context assembly on the assistant path | Elevation of Privilege | Yes | Critical | Architecture-specific analysis |
| S2-T-29 | Direct prompt injection through the user question | Tampering | Yes | High | OWASP LLM Security |
| S2-T-30 | Indirect prompt injection through uploaded document content | Tampering | Yes | High | OWASP LLM Security |
| S2-T-31 | Disclosure of sensitive document content to the external LLM provider | Information Disclosure | Yes | High | OWASP LLM Security |
| S2-T-32 | Cross-user context leakage within the assistant pipeline | Information Disclosure | Yes | Medium | Research-based LLM Threat |
| S2-T-33 | Extraction of assistant instructions through elicitation | Information Disclosure | Yes | Medium | OWASP LLM Security |
| S2-T-34 | Insecure handling of generated output rendered in the browser | Elevation of Privilege | Yes | Critical | OWASP LLM Security |
| S2-T-35 | Hallucinated document summaries and explanations | Tampering | Yes | Medium | OWASP LLM Security |
| S2-T-36 | Delivery of attacker-authored instructions through assistant responses | Spoofing | Yes | High | Research-based LLM Threat |
| S2-T-37 | Excessive trust in assistant output regarding document contents | Tampering | Yes | Medium | OWASP LLM Security |
| S2-T-38 | Compromise or substitution of the external LLM provider endpoint | Spoofing | Yes | High | OWASP LLM Security |
| S2-T-39 | Provider-side retention or reuse of transmitted document context | Information Disclosure | Yes | Medium | OWASP LLM Security |
| S2-T-40 | Loss of assistant availability through external provider disruption | Denial of Service | Yes | Medium | Architecture-specific analysis |
| S2-T-41 | Unbounded consumption of the assistant processing path | Denial of Service | Yes | Medium | OWASP LLM Security |

---

*Scenario 2 reference threat model, version 2.0. Architecture-grounded baseline created through systematic threat identification. Not absolute ground truth. Frozen for evaluation.*
