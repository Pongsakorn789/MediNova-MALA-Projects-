# rule.md — Legal, Compliance & AI Ethics Rules
**Project:** MALA Risk Screening System (MRSS) — Web Application
**Company:** MediNova · **Course:** 1305493 Software Engineering Case Studies
**Phase:** DISCOVER (W1–W5) → binding rules for the BUILD phase
**Status:** Draft v2 (aligned with the Project Charter, MALA_outline_prg.xlsx, MALA_Screening_Checklist.docx and the demo slides)

> **MUST / MUST NOT** = mandatory, **SHOULD** = recommended. Every backlog feature must trace back to a rule in this file (see Section 12).
> This system is a **proof-of-concept decision-support tool**, not a diagnostic device.

---

## 1. Scope and Legal Roles

| Role | Responsible party (proposed) | Note |
|---|---|---|
| Data Controller | Chiangrai Prachanukroh Hospital (owner of patient/eGFR data) | To be confirmed with the hospital |
| Data Processor | MediNova dev team and cloud / AI / LINE providers | A Data Processing Agreement (DPA) is required in BUILD |
| Users | PCU Staff; hospital multidisciplinary team (physicians, pharmacists, nurses) | |
| Data Subject | Diabetic patients on Metformin | |

- Health data is **Sensitive Data** under the Personal Data Protection Act B.E. 2562 (PDPA), Section 26.
- During DISCOVER, real patient data **MUST NOT** be used; use synthetic data only. Integration with HosXP / live hospital databases is out of scope.

---

## 2. Data Inventory and Minimization

Based on checklist items 1–13. The system **MUST** collect only data necessary for screening (Data Minimization).

| # | Data | Source | Class | Why it is needed |
|---|---|---|---|---|
| 1 | HN | Scan / manual entry | Identifiable | Link to eGFR and follow-up |
| 2 | Full name | Entry / from HIS | Identifiable | Confirm patient before dispensing |
| 3–4 | Sex, age | Entry / HIS | Personal | Alcohol thresholds differ by sex; age as a risk factor |
| 5–6 | Weight, height → BMI | Entry | Health | BMI calculation (< 23 = risk) |
| 7 | Latest eGFR | Auto-retrieved from HIS | Health | CPG dose check + risk score |
| 8 | Metformin dose (mg) | Entry | Health | Compare against CPG |
| 9 | Alcohol history (9.1, 9.2.1, 9.2.2) | Entry | Health | Risk factor |
| 10–11 | Vomiting/diarrhea; poor oral intake | Entry | Health | Sick Day Rules |
| 12–13 | NSAIDs/mixed-pill packs; herbal/supplements | Entry | Health | AKI-inducing triggers |

Rules:
- **MUST NOT** collect data outside this table (e.g., national ID, address, phone) without a reviewed clinical justification.
- **MUST** store direct identifiers (HN, name) separately from screening data, linked by an internal ID where feasible.
- The Screening Report **SHOULD** show full names only to roles that need them, with masking by default.

---

## 3. PDPA — Mandatory Requirements

### 3.1 Legal basis and consent
- **MUST** inform the patient of purpose, data categories, retention period, recipients and rights **before** collection (Privacy Notice, Sec. 23).
- **MUST** obtain explicit consent for health data (Sec. 19, 26) and record it in the audit trail (who / when / which text version).
- The screening form **MUST NOT** be submittable without a consent status.
- If the patient declines, the system **MUST NOT** store screening data and **MUST NOT** affect their access to medication (record only "declined").
- Any other legal basis (e.g., preventive medicine / health-service provision, Sec. 26(5)) must be confirmed by the hospital as data controller; the dev team must not interpret it alone.

### 3.2 Data subject rights (Sec. 30–36)
The system **MUST** support: access/copy, rectification, erasure, restriction, objection, and withdrawal of consent.
- The request channel and responsible person must be stated in the Privacy Notice.
- Erasure **MUST** respect medical-record retention requirements (see 3.4) and be logged as an audit event.

### 3.3 Disclosure and transfer
- Data sent to LINE / AI services / cloud hosted abroad **MUST** go through a cross-border transfer assessment (Sec. 28–29) and have a DPA (Sec. 40).
- **LINE alert card (important):** the demo slides show full name + HN + eGFR in the LINE group. In BUILD the card **SHOULD** show HN + abnormal values only (no full name) and link to a case page that requires login. Group membership **MUST** be limited to authorized team members and reviewed periodically.
- **AI image reading (HN from patient booklet / paper form):** images contain identifiers → **MUST** use a provider with a DPA, no training on our data, and delete source images per Section 8.

### 3.4 Retention (proposal — hospital to confirm)
| Data | Period |
|---|---|
| Screening / risk results | Per the hospital's medical-record policy |
| Access logs, all roles | **≥ 90 days** (see Section 5) |
| Paper-form / HN images used for OCR | Delete after the data is saved and confirmed (max 30 days) |
| Consent records | Same as the lifetime of the related data |

### 3.5 Security and breaches (Sec. 37)
- **MUST** encrypt data in transit (TLS 1.2+) and at rest.
- **MUST** have an incident-response plan and notify the PDPC **within 72 hours** of becoming aware of a breach (if it risks individuals' rights and freedoms), and notify data subjects if the risk is high.
- **MUST NOT** write identifiers or health data to application logs, error trackers, analytics, URLs or query strings.

---

## 4. Role-Based Access Control (RBAC)

Least privilege — **MUST** be enforced server-side, not just by hiding UI buttons.

| Capability | PCU Staff | Multidisciplinary team | System admin | Auditor / DPO |
|---|---|---|---|---|
| Fill in screening form | ✅ | ❌ | ❌ | ❌ |
| Retrieve latest eGFR by HN | ✅ (only the patient being screened) | ✅ | ❌ | ❌ |
| View screening report | Own unit only | All units in the network | ❌ (no health data) | Read-only |
| Acknowledge / open alert case | ❌ | ✅ | ❌ | ❌ |
| Edit risk criteria | ❌ | ❌ | ❌ | ❌ (requires clinical review, see 7.5) |
| Manage users | ❌ | ❌ | ✅ | ❌ |
| View audit log | ❌ | ❌ | ❌ | ✅ |

- **MUST** use individual accounts (no shared accounts) and session timeouts.
- **SHOULD** require MFA for the multidisciplinary team and admins.
- Patients do not log in; their rights are exercised via the request channel (3.2).

---

## 5. Audit Trail and Access Logs

### 5.1 Computer Crime Act, Section 26
- **MUST** retain access logs for **every role** for at least **90 days**, in tamper-resistant storage (append-only / write-once).
- Each log entry: user ID, role, timestamp (NTP-synced, stored in UTC, displayed in Asia/Bangkok), IP/device, action, case ID / HN (as a reference code, not raw health data).

### 5.2 Feature-level Audit Trail (Feature #7)
**MUST** record who–when–what for: HN scan/confirmation, consent, eGFR retrieval, screening submission, risk result (with rule version), alert dispatch, alert acknowledgement/case opening, data edits or deletions, report views.
- Audit records **MUST NOT** be edited or deleted; corrections add a new record referencing the original.

### 5.3 Electronic Transactions Act
- Alert acknowledgements and case handling **MUST** be recorded as reliable electronic evidence (the Charter cites Sec. 9): bound to the actor's identity, timestamped, and tamper-evident (e.g., hash chain / electronic signature).
- *Note for the team:* Sec. 9 concerns electronic signatures, while admissibility of electronic evidence is in Sec. 11 — have an expert/instructor confirm the citation before the Gate.

---

## 6. Cybersecurity

The system may connect to hospital infrastructure subject to the Cybersecurity Act B.E. 2562 (the hospital must confirm CII status).
- **MUST**: parameterized queries (anti-SQLi), output encoding (anti-XSS), CSRF protection, rate limiting, client- and server-side input validation.
- **MUST NOT** embed secrets/API keys/tokens in source code or repos; use a secret manager or environment variables.
- **MUST** scan dependencies for vulnerabilities and patch on a schedule.
- HIS integration: read-only, minimum necessary scope (eGFR and basic identifiers), through a hospital-approved channel.
- **MUST** have backups with tested restore, and separate dev/test/prod environments.

---

## 7. AI Ethics and Clinical Safety

### 7.1 Principles
1. **Human-in-the-loop:** the system advises and alerts only; dose changes, stopping drugs and diagnosis belong to physicians/pharmacists.
2. **Transparency:** users must see which criteria produced a result; AI-generated text must be labeled ("AI generated").
3. **Do no harm:** when data is missing or contradictory, fail safe — never conclude "low risk".
4. **Fairness:** no discrimination by sex/age beyond what the clinical criteria specify.

### 7.2 Division of labor: clinical rules vs. AI
- Risk calculation (CPG dose check, risk score) **MUST** be **deterministic, rule-based** and reproducible; a language model must not decide it.
- AI (LLM/OCR) may only: read HN/paper-form images, and phrase advice from approved templates/knowledge base.
- **MUST** have staff **confirm** AI-read values before use (e.g., "Is this HN correct?" as in the slides); low-confidence values from paper must be flagged for review.
- AI-generated advice **MUST** be guardrailed: no self-directed dose changes, no diagnosis, always include Sick Day Rules and red-flag symptoms requiring immediate hospital care; **SHOULD** use pharmacist-approved templates.
- A patient-facing chatbot (if any) is outside the MVP until it passes clinical and legal review.

### 7.3 Clinical rules used in the PoC (from MALA_outline_prg.xlsx)

**Part 1 — eGFR vs Metformin dose (CPG)**
| eGFR (mL/min/1.73 m²) | Rule |
|---|---|
| ≥ 45 | ≤ 2000 mg/day |
| 30–44 | ≤ 1000 mg/day |
| < 30 | Contraindicated |

Inappropriate dose → consult physician to adjust + **automatic alert to the responsible physician (LINE group of the multidisciplinary team)**.

**Part 2 — Risk score (research)**
| Factor | Points |
|---|---|
| eGFR < 60 | 1 |
| BMI < 23 | 2 |
| Heavy alcohol use | 2 |

Total **≥ 2 = at risk of MALA** → record for close monitoring / adjust dose or stop Metformin; advise stopping alcohol; advise Sick Day Rules.

**Parts 3 & 4 — Triggers (Yes = advise Sick Day Rules and record)**
Vomiting/diarrhea, unable to eat for 1–2 days, NSAIDs/mixed-pill packs, herbal decoctions/supplements.

**Definition of "heavy alcohol use"** (checklist item 9.2): 1 standard drink = 10 g alcohol.
- Binge in a short period (9.2.1): male > 5 drinks / female > 4 drinks within 2 hours.
- Regular/continuous heavy (9.2.2): male > 15 drinks/week / female > 8 drinks/week.
- "Yes" to either = heavy use (clinical team to confirm OR/AND logic).

### 7.4 Inconsistencies to resolve before BUILD
| # | Issue | Source | Action |
|---|---|---|---|
| 1 | Slides show Risk Score **72** vs threshold **60**, but the xlsx uses score **≥ 2** | Slide 6 vs xlsx Part 2 | Pick one model; do not publish demo numbers that don't match the real rules |
| 2 | Slides use **4 alcohol levels** (none / occasional / regular / heavy), but the checklist uses **9.1 + 9.2.1 + 9.2.2** | Slide 4 vs checklist | Align the UI to the checklist, or define the mapping to "heavy" |
| 3 | Demo has 5 steps with no sex/age, Metformin dose, or symptom/medication questions (items 8, 10–13) | Slides 1–5 vs checklist | Add steps to the prototype so the CPG check and Sick Day Rules work |
| 4 | "AI calculates Risk Score" in slides conflicts with the deterministic principle | Slide 5 | Say "rule engine calculates"; AI only for OCR / phrasing advice |
| 5 | eGFR/score criteria not yet reviewed by clinical experts | Charter Sec. 10 | Label "PoC — not clinically validated"; do not use on real patients until validated |

### 7.5 Criteria governance and versioning
- All criteria **MUST** live in versioned configuration, not hard-coded across the codebase.
- Every calculation **MUST** record the criteria version used.
- Criteria changes require physician/pharmacist approval and an audit-trail entry.
- **MUST** have test cases covering boundaries: eGFR = 29/30/44/45/59/60, BMI = 22.9/23, dose = 1000/1001/2000/2001 mg, missing or implausible values.

---

## 8. Offline Fallback (Paper Form + Photo)

1. The paper form uses the same items as the checklist (1–13).
2. **MUST** obtain consent before collection, same as online (add a consent box to the form).
3. Photos **MUST** be uploaded through the logged-in system — never via personal chat/social apps, and never left in a personal phone gallery; delete after saving and confirmation.
4. Original paper is kept locked and destroyed per policy.
5. Values read by AI from paper **MUST** be confirmed by staff before risk calculation (or marked "pending review").
6. Retroactive calculation and alerts use the same rules as the online channel, and the audit trail marks the entry as "paper channel" with both the actual screening time and the system entry time.

---

## 9. Risk Engine and Smart Alert

- Alert triggers: (a) dose inappropriate per CPG, or (b) risk score reaches the threshold.
- Card content **MUST** be the minimum necessary: risk level, screening site, abnormal values, suggested management, "Open case" / "Acknowledge" buttons (see 3.3 on omitting the name).
- **MUST** log delivery success/failure, who acknowledged and when; if LINE delivery fails, a fallback channel must exist and PCU staff must be notified.
- **MUST NOT** conclude "normal" if the eGFR retrieval fails → show "No eGFR data" and treat it as needing follow-up.

---

## 10. Patient-Facing Output

- Printed advice sheets (Sick Day Rules, warning symptoms) **MUST** use plain language and contain no other patient's identifiers.
- Patients must be told the result is a preliminary screening, not a diagnosis.

---

## 11. Development Rules (including AI coding agents)

- **MUST NOT** put real patient data, identifiers or secrets in prompts, repos, code samples or external AI tools.
- **MUST** use synthetic data that cannot be traced to real people for testing/demos.
- **MUST** have a human review AI-generated code, especially auth, RBAC, risk calculation and logging.
- **SHOULD** keep clinical criteria in a version-controlled file and write tests directly from xlsx Parts 1–2.
- Definition of Done for any feature touching health data: passes RBAC tests, emits an audit event, no PII in logs, consent check in place.

---

## 12. Traceability: Feature → Rules

| # | Feature (Charter Sec. 7) | Main rules |
|---|---|---|
| 1 | Smart Screening Form | 2, 3.1, 3.5, 4, 7.3 |
| 2 | Screening Report | 2, 4, 5.2 |
| 3 | Auto-Data Integration (eGFR from HIS) | 3.3, 4, 6, 9 |
| 4 | Risk Engine & Smart Alert | 3.3, 7.2–7.5, 9 |
| 5 | Decision Support (CPG) | 7.1–7.3, 10 |
| 6 | Offline Fallback | 3.1, 3.3, 8 |
| 7 | Audit Trail | 3.5, 5.1–5.3 |

Compliance-related KPIs: ≥ 80% of Metformin patients screened via the system (each with consent + audit record); ≥ 85% staff satisfaction.

---

## 13. Open Items to Confirm Before BUILD

1. Legal basis for health data and the actual data controller (Chiangrai Prachanukroh Hospital / Provincial Health Office) — confirm with the hospital.
2. Cloud/AI/LINE providers: server location, DPA, whether our data may be used for model training.
3. Retention periods per the hospital's medical-record policy.
4. Hospital's CII status and HosXP integration requirements.
5. Expert validation of eGFR/risk-score criteria, and resolution of the inconsistencies in 7.4.
6. Verify the ETA citations (Sec. 9 vs 11) and finalize the real Privacy Notice / Consent documents.
7. LINE card: decide whether to show the patient's full name.

---
*This document is a project-team guideline for an academic project and is not legal advice. Have a legal expert / the hospital's DPO review it before real-world use.*
