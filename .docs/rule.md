# rule.md — Legal, Compliance & AI Ethics Rules
**Project:** MALA Risk Screening & Monitoring System (MRSS) — Web Application
**Company:** MediNova · **Course:** 1305493 Software Engineering Case Studies
**Phase:** DISCOVER (W1–W5) → binding rules for the BUILD phase
**Status:** Draft v3.3, revised November 16, 2026 (aligned with the Project Charter, MALA_outline_prg.xlsx, MALA_Screening_Checklist.docx, and the prototype at https://mala-project-2026.web.app, re-checked on November 16, 2026)

> **MUST / MUST NOT** = mandatory, **SHOULD** = recommended. Every backlog feature must trace back to a rule in this file (see Section 12).
> This system is a **proof-of-concept decision-support tool**, not a diagnostic device.
> **Prototype scope:** the SHPH Staff screening flow is implemented in the prototype. The physician and MALA case manager screens are **mockups**; the hospital implements their real functions. The rules for those roles apply when the hospital builds them.

---

## 1. Scope and Legal Roles

| Role | Responsible party (proposed) | Note |
|---|---|---|
| Data Controller | Chiangrai Prachanukroh Hospital (owner of patient/eGFR data) | To be confirmed with the hospital |
| Data Processor | MediNova dev team and cloud / LINE providers (and any AI provider added in the future) | A Data Processing Agreement (DPA) is required in BUILD |
| Users | SHPH Staff; physician / multidisciplinary team (mockup); MALA case manager, who is also the system administrator (mockup, one role) | |
| Data Subject | Diabetic patients on Metformin | |

- Health data is **Sensitive Data** under the Personal Data Protection Act B.E. 2562 (PDPA), Section 26.
- During DISCOVER and testing, the team uses patient data supplied by the hospital with its permission; patients' ID card numbers were removed before use (HN, name, sex, age and clinical values remain). Any other real patient data **MUST NOT** be used. The data **MUST** be used only for this course project, **MUST** be stored only in the project's controlled environment, and **MUST NOT** be posted in the repository, chat tools or external AI tools. Integration with HosXP / live hospital databases is out of scope.
- The prototype can import a patient list from an Excel file. It reads HN, name, sex, age and eGFR (and the area when the file has one). The file is read in the browser and is not sent to a backend. Only the hospital-approved data set may be imported; other real patient lists **MUST NOT** be used in the prototype or in testing.
- Test patients entered by hand (for example "ทดสอบ ระบบ", HN TEST-001) **MUST** use invented values only. They and the test runs saved with them **MUST** be removed from the prototype data before the UAT, a demo or hand-over, because the dashboard counts them.

---

## 2. Data Inventory and Minimization

Based on checklist items 1–13 and the prototype's manual-entry form. The system **MUST** collect only data necessary for screening (Data Minimization).

| # | Data | Source | Class | Why it is needed |
|---|---|---|---|---|
| 1 | HN | Scan / chosen from list / typed / Excel | Identifiable | Link to eGFR and follow-up |
| 2 | Full name | Entry / Excel / from HIS | Identifiable | Confirm patient before dispensing |
| 3–4 | Sex, age (manual entry: date of birth, from which the system calculates the age) | Entry / Excel / HIS | Personal | Alcohol thresholds differ by sex; age as a risk factor |
| 5–6 | Weight, height → BMI | Entry | Health | BMI calculation (< 23 = risk) |
| 7 | Latest eGFR | Retrieved with the patient record / Excel | Health | CPG dose check + risk score |
| 8 | Metformin dose (mg/day) | Retrieved with the record / entry | Health | Compare against CPG (Part 1) |
| 9 | Alcohol history (9, 9.2.1, 9.2.2) | Entry | Health | Risk factor |
| 10–11 | Vomiting/diarrhea; poor oral intake | Entry | Health | Sick Day Rules |
| 12–13 | NSAIDs/mixed-pill packs; herbal/supplements | Entry | Health | AKI-inducing triggers |
| 14 | Area / village of the patient and the screening SHPH | Entry / Excel / system | Personal (coarse location) | Counts by area on the case manager dashboard |

Rules:
- **MUST NOT** collect data outside this table (e.g., phone number, full address) without a reviewed clinical justification. The prototype's search and manual-entry form also accept an **ID card number**; see Section 13, item 8.
- **MUST** store direct identifiers (HN, name) separately from screening data, linked by an internal ID where feasible.
- The Screening Report and dashboard **SHOULD** show full names only to roles that need them, with masking by default. The dashboard in the prototype shows counts only, no names.

---

## 3. PDPA — Mandatory Requirements

### 3.1 Legal basis and consent
- **MUST** inform the patient of purpose, data categories, retention period, recipients and rights **before** collection (Privacy Notice, Sec. 23).
- **MUST** obtain explicit consent for health data (Sec. 19, 26) and record it in the audit trail (who / when / which text version).
- The screening form **MUST NOT** be submittable without a consent status. The prototype does not yet have a consent step (confirmed on November 16); it is required in BUILD.
- If the patient declines, the system **MUST NOT** store screening data and **MUST NOT** affect their access to medication (record only "declined").
- Any other legal basis (e.g., preventive medicine / health-service provision, Sec. 26(5)) must be confirmed by the hospital as data controller; the dev team must not interpret it alone.

### 3.2 Data subject rights (Sec. 30–36)
The system **MUST** support: access/copy, rectification, erasure, restriction, objection, and withdrawal of consent.
- The request channel and responsible person must be stated in the Privacy Notice.
- Erasure **MUST** respect medical-record retention requirements (see 3.4) and be logged as an audit event.

### 3.3 Disclosure and transfer
- Data sent to LINE / external services / cloud hosted abroad **MUST** go through a cross-border transfer assessment (Sec. 28–29) and have a DPA (Sec. 40).
- **Where cases go (current design):** a dose-fail case goes to the doctor's queue, and an at-risk case goes to the MALA case manager queue, inside the system. Both queues **MUST** require login and be limited to authorized staff.
- **LINE alert (optional, on the case manager side, may not be connected):** if the hospital connects it, the card **SHOULD** show HN + abnormal values only (no full name) and link to a case page that requires login. Group membership **MUST** be limited to authorized team members and reviewed periodically. There is no LINE button for SHPH Staff.
- **Advice service (current design):** the rule-based advice step receives only age, eGFR, Metformin dose and the risk-factor answers. It **MUST NOT** receive the patient's name or HN. The team must confirm whether this service is run by the team or by a third party; if third party, a DPA is required.
- **AI image reading (HN from patient booklet / paper form):** **not used in the current prototype.** If added later: images contain identifiers → **MUST** use a provider with a DPA, no training on our data, and delete source images per Section 8. Camera scanning of the HN needs the browser's camera permission.

### 3.4 Retention (proposal — hospital to confirm)
| Data | Period |
|---|---|
| Screening / risk results | Per the hospital's medical-record policy |
| Access logs, all roles | **≥ 90 days** (see Section 5) |
| Paper-form / HN images used for OCR (if built) | Delete after the data is saved and confirmed (max 30 days) |
| Consent records | Same as the lifetime of the related data |

### 3.5 Security and breaches (Sec. 37)
- **MUST** encrypt data in transit (TLS 1.2+) and at rest.
- **MUST** have an incident-response plan and notify the PDPC **within 72 hours** of becoming aware of a breach (if it risks individuals' rights and freedoms), and notify data subjects if the risk is high.
- **MUST NOT** write identifiers or health data to application logs, error trackers, analytics, URLs or query strings.
- **MUST** keep the CSV export on the case manager dashboard free of names and direct identifiers (or restrict it by role), and treat downloaded files as health data.

---

## 4. Role-Based Access Control (RBAC)

Least privilege — **MUST** be enforced server-side, not just by hiding UI buttons. The physician and case manager screens are mockups in the prototype; this table is the requirement for the hospital's real implementation.

| Capability | SHPH Staff | Physician / multidisciplinary team | MALA case manager / system admin (one role) | Auditor / DPO |
|---|---|---|---|---|
| Fill in screening form | ✅ | ❌ | ❌ | ❌ |
| Retrieve latest eGFR and dose by HN | ✅ (only the patient being screened) | ✅ | ❌ | ❌ |
| View the doctor's queue (dose-fail cases) | ❌ | ✅ | ❌ | Read-only |
| View the case manager queue (at-risk cases) | ❌ | ❌ | ✅ | Read-only |
| View dashboard / report and download CSV | Own unit only | All units in the network | ✅ (counts by area, no full names by default) | Read-only |
| Edit risk criteria | ❌ | ❌ | ❌ | ❌ (requires clinical review, see 7.5) |
| Manage users | ❌ | ❌ | ✅ | ❌ |
| View audit log | ❌ | ❌ | ❌ | ✅ |

- The case manager and the system administrator are **one role** in this project, so the same account can view case data and manage users. In BUILD the hospital **SHOULD** separate these permissions, or at least log every use of them.
- **MUST** use individual accounts (no shared accounts) and session timeouts.
- **SHOULD** require MFA for the physician / team and the case manager / admin.
- Patients do not log in; their rights are exercised via the request channel (3.2).
- The prototype's login page pre-fills a demo account for the SHPH role. Demo accounts **MUST NOT** exist in a production system.

---

## 5. Audit Trail and Access Logs

### 5.1 Computer Crime Act, Section 26
- **MUST** retain access logs for **every role** for at least **90 days**, in tamper-resistant storage (append-only / write-once).
- Each log entry: user ID, role, timestamp (NTP-synced, stored in UTC, displayed in Asia/Bangkok), IP/device, action, case ID / HN (as a reference code, not raw health data).

### 5.2 Feature-level Audit Trail (Feature #7)
**MUST** record who–when–what for: HN scan/confirmation, consent, eGFR retrieval, screening submission, Part 1 dose result and Part 2 score (with rule version), routing of the case (doctor's queue or case manager queue), case opening by the physician or case manager, data edits or deletions, report views and CSV downloads.
- Audit records **MUST NOT** be edited or deleted; corrections add a new record referencing the original.

### 5.3 Electronic Transactions Act
- Case handling by the physician or case manager **MUST** be recorded as reliable electronic evidence (the Charter cites Sec. 9): bound to the actor's identity, timestamped, and tamper-evident (e.g., hash chain / electronic signature).
- *Note for the team:* Sec. 9 concerns electronic signatures, while admissibility of electronic evidence is in Sec. 11 — have an expert/instructor confirm the citation before the Gate.

---

## 6. Cybersecurity

The system may connect to hospital infrastructure subject to the Cybersecurity Act B.E. 2562 (the hospital must confirm CII status).
- **MUST**: parameterized queries (anti-SQLi), output encoding (anti-XSS), CSRF protection, rate limiting, client- and server-side input validation.
- **MUST NOT** embed secrets/API keys/tokens in source code or repos; use a secret manager or environment variables.
- **MUST** scan dependencies for vulnerabilities and patch on a schedule.
- HIS integration: read-only, minimum necessary scope (eGFR, Metformin dose and basic identifiers), through a hospital-approved channel.
- **MUST** have backups with tested restore, and separate dev/test/prod environments.

---

## 7. AI Ethics and Clinical Safety

### 7.1 Principles
1. **Human-in-the-loop:** the system advises and routes cases only; dose changes, stopping drugs and diagnosis belong to physicians/pharmacists. The dose-fail screen tells staff to follow the doctor's order and says the system records and forwards the case only.
2. **Transparency:** users must see which criteria produced a result (the review screen shows the Part 1 result and the Part 2 score with its reason); any AI-generated text, if AI is added later, must be labeled ("AI generated").
3. **Do no harm:** when data is missing or contradictory, fail safe — never conclude "low risk".
4. **Fairness:** no discrimination by sex/age beyond what the clinical criteria specify.

### 7.2 Division of labor: clinical rules vs. AI
- Risk calculation (CPG dose check, risk score) **MUST** be **deterministic, rule-based** and reproducible; a language model must not decide it.
- In the current prototype, patient advice is **rule-based** and no trained AI model is used. If the team later adds AI (LLM/OCR) for reading HN or paper-form images, or for phrasing advice from approved templates, the rules in this section and in 3.3 apply.
- **MUST** have staff **confirm** the patient before use ("ถูกต้อง · ถัดไป" / "สแกนใหม่" in the prototype); low-confidence values from paper (if built) must be flagged for review.
- Advice (rule-based now; AI-generated if added later) **MUST** be guardrailed: no self-directed dose changes, no diagnosis, always include Sick Day Rules and red-flag symptoms requiring immediate hospital care; **SHOULD** use pharmacist-approved templates.
- A patient-facing chatbot (if any) is outside the MVP until it passes clinical and legal review.

### 7.3 Clinical rules used in the PoC (from MALA_outline_prg.xlsx and the live prototype)

**Order of checks:** Part 1 (dose) runs first, when the staff member confirms the patient in step 1. If it fails, the MALA assessment is skipped and the case goes to the doctor's queue. If it passes, Part 2 is calculated.

**Part 1 — eGFR vs Metformin dose (CPG table, as implemented in the prototype and chosen by the team)**
| Condition | Result |
|---|---|
| eGFR ≥ 45 and Metformin ≤ 2,000 mg/day | Pass |
| eGFR 30–44 and Metformin ≤ 1,000 mg/day | Pass ("ตาม CPG ที่ให้มา") |
| eGFR 30–44 and Metformin > 1,000 mg/day | Fail → stop the assessment, send the case to the doctor's queue immediately (tested on November 16 with eGFR 40 and 2,000 mg/day) |
| Metformin > 2,000 mg/day, or eGFR < 30 | Fail (from the CPG table; the exact screen text was not re-tested) |

The team uses this CPG table as the dose rule. The clinical team **SHOULD** still confirm the limits when the criteria are validated (see 7.4, item 5).

**Part 2 — Risk score (research)**
| Factor | Points |
|---|---|
| eGFR < 60 | 1 |
| BMI < 23 | 2 |
| Heavy alcohol use | 2 |

Total **≥ 2 = at risk of MALA** → the case goes to the MALA case manager queue (not to the doctor); advise stopping alcohol; advise Sick Day Rules. The number of "yes" answers is not the score.

**Parts 3 & 4 — Triggers (shown and recorded, not scored; Yes = advise Sick Day Rules)**
Vomiting/diarrhea (Q10), unable to eat or eating less for 1–2 days (Q11), NSAIDs/mixed-pill packs (Q12), herbal decoctions/supplements (Q13).

**Definition of "heavy alcohol use"** (checklist item 9.2): 1 standard drink = 10 g alcohol.
- Binge in a short period (9.2.1): male 5 or more drinks / female 4 or more within 2 hours.
- Regular/continuous heavy (9.2.2): male 15 or more drinks/week / female 8 or more per week. The prototype shows "ขึ้นไป" (or more); the original checklist wrote "more than", so the clinical team is to confirm the limit.
- "Yes" to either = heavy use. The prototype gave 2 points when 9.2.2 was answered Yes; the clinical team is to confirm the OR/AND logic.

### 7.4 Inconsistencies to resolve before BUILD
| # | Issue | Source | Status / Action |
|---|---|---|---|
| 1 | Slides showed Risk Score **72** vs threshold **60**, but the xlsx uses score **≥ 2** | Slide 6 vs xlsx Part 2 | **Resolved in the prototype:** it uses the score ≥ 2. Do not publish old demo numbers. |
| 2 | Slides used **4 alcohol levels**, but the checklist uses **9.1 + 9.2.1 + 9.2.2** | Slide 4 vs checklist | **Resolved in the prototype:** it uses the checklist questions. |
| 3 | Demo had 5 steps with no sex/age, Metformin dose, or symptom/medication questions | Slides 1–5 vs checklist | **Resolved in the prototype:** Metformin dose and Q10–Q13 are included, and sex sets the alcohol thresholds. |
| 4 | "AI calculates Risk Score" in slides conflicts with the deterministic principle | Slide 5 | **Resolved:** the rule engine calculates the score and advice is rule-based; the prototype uses no trained AI model |
| 5 | eGFR/score criteria not yet reviewed by clinical experts | Charter Sec. 10 | Label "PoC — not clinically validated"; do not use for real patient care until validated |
| 6 | The patient list and the patient card still show old three-level labels ("ความเสี่ยงต่ำ / เสี่ยงปานกลาง / เสี่ยงสูง", "ระดับความเสี่ยงเดิม"), and the case manager history shows "Yellow", none of which are part of Part 1 / Part 2 | Prototype (seen November 16) | Remove or rename the labels |
| 7 | Volunteer wording (อสม.) is still shown on the review screen ("บันทึกโดย อสม. ประจำหมู่ 1") and in the dashboard guidance card | Prototype (seen November 16) | Replace with "บันทึกโดย" plus the logged-in user ID, and with SHPH staff / health personnel |
| 8 | The prototype has no consent step | Prototype | Add a consent step before screening (see 3.1) |
| 9 | The advice card uses a sparkle icon that suggests AI, while the advice is rule-based (7.1 principle 2) | Prototype (seen November 16) | Use a neutral icon |
| 10 | The dose line shows the unit twice ("(500 มก.) มก./วัน"), and the dashboard denominator (30) ignores patients added by hand or by import | Prototype (seen November 16) | Correct the text and the count |
| 11 | Test data from the team's runs is counted on the dashboard | Prototype | Clear it before the UAT (see Section 1) |

### 7.5 Criteria governance and versioning
- All criteria **MUST** live in versioned configuration, not hard-coded across the codebase.
- Every calculation **MUST** record the criteria version used.
- Criteria changes require physician/pharmacist approval and an audit-trail entry.
- **MUST** have test cases covering boundaries: eGFR = 29/30/44/45/59/60, BMI = 22.9/23, dose = 1000/1001/2000/2001 mg, missing or implausible values.

---

## 8. Offline Fallback (Paper Form + Photo)

Offline save with automatic sync, and AI reading of a photographed form, have **not been seen in the prototype**. If the team builds them, these rules apply.

1. The paper form uses the same items as the checklist (1–13).
2. **MUST** obtain consent before collection, same as online (add a consent box to the form).
3. Photos **MUST** be uploaded through the logged-in system — never via personal chat/social apps, and never left in a personal phone gallery; delete after saving and confirmation.
4. Original paper is kept locked and destroyed per policy.
5. Values read by AI from paper **MUST** be confirmed by staff before risk calculation (or marked "pending review").
6. Retroactive calculation and routing use the same rules as the online channel, and the audit trail marks the entry as "paper channel" with both the actual screening time and the system entry time.

---

## 9. Risk Engine and Routing

- Routing triggers: (a) dose fails the CPG check (Part 1) → **doctor's queue immediately**; (b) Part 2 score ≥ 2 → **MALA case manager queue**.
- A dose-fail case **MUST NOT** enter the case manager queue, and an at-risk case **MUST NOT** go to the doctor.
- Queue entries **MUST** show the minimum necessary: risk level, screening site, abnormal values, suggested management (see 3.3 on omitting the name in any LINE card).
- **MUST** log the time a case was routed, who opened it and when; if an optional LINE alert is used and its delivery fails, a fallback channel must exist and SHPH staff must be notified.
- **MUST NOT** conclude "normal" if the eGFR retrieval fails → show "No eGFR data" and treat it as needing follow-up.

---

## 10. Patient-Facing Output

- Printed advice sheets (Sick Day Rules, warning symptoms) **MUST** use plain language and contain no other patient's identifiers.
- Patients must be told the result is a preliminary screening, not a diagnosis.

---

## 11. Development Rules (including AI coding agents)

- **MUST NOT** put patient data, identifiers or secrets in prompts, repos, code samples or external AI tools.
- **MUST** use only the hospital-approved data set (ID card numbers removed) or synthetic data for testing and demos, and keep it out of the repository.
- **MUST** have a human review AI-generated code, especially auth, RBAC, risk calculation and logging.
- **SHOULD** keep clinical criteria in a version-controlled file and write tests directly from xlsx Parts 1–2.
- Definition of Done for any feature touching health data: passes RBAC tests, emits an audit event, no PII in logs, consent check in place.

---

## 12. Traceability: Feature → Rules

| # | Feature (Charter Sec. 7) | Main rules |
|---|---|---|
| 1 | Smart Screening Form (5 steps, manual entry, Excel import) | 1, 2, 3.1, 3.5, 4, 7.3 |
| 2 | Screening Report and case manager dashboard (CSV export) | 2, 3.5, 4, 5.2 |
| 3 | Auto-Data Integration (eGFR and dose with the record) | 3.3, 4, 6, 9 |
| 4 | Risk Engine and routing (dose check first, Part 2 score) | 3.3, 7.2–7.5, 9 |
| 5 | Decision Support (CPG, Sick Day Rule advice) | 7.1–7.3, 10 |
| 6 | Offline Fallback (not seen in the prototype) | 3.1, 3.3, 8 |
| 7 | Audit Trail | 3.5, 5.1–5.3 |
| 8 | Doctor's queue and case manager queue (mockups) | 3.3, 4, 5.2, 9 |

Compliance-related KPIs: ≥ 80% of Metformin patients screened via the system (each with consent + audit record); ≥ 85% staff satisfaction.

---

## 13. Open Items to Confirm Before BUILD

1. Legal basis for health data and the actual data controller (Chiangrai Prachanukroh Hospital / Provincial Health Office) — confirm with the hospital.
2. Cloud and LINE providers: server location (including the hosting of the web app), DPA, and whether the advice service is run by the team or a third party. The current design uses no external AI model; if one is added, confirm whether our data may be used for model training.
3. Retention periods per the hospital's medical-record policy.
4. Hospital's CII status and HosXP integration requirements.
5. Expert validation of eGFR/risk-score criteria (including the CPG dose limits), and resolution of the inconsistencies in 7.4 (especially the heavy-alcohol logic).
6. Verify the ETA citations (Sec. 9 vs 11) and finalize the real Privacy Notice / Consent documents; add a consent step to the screening flow.
7. LINE alert: decide whether the hospital connects it and whether the card shows the patient's full name.
8. ID card number: the hospital data set had patients' ID card numbers removed. The prototype search and manual-entry form still accept an ID card number; decide whether to use HN only (preferred under data minimization) or justify collecting it.
9. The case manager and the system administrator are one role; decide whether the hospital separates user management from access to case data.
10. Whether the CSV export may contain identifiers, and who may download it.

---
