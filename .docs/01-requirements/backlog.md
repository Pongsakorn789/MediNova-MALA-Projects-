# Product Backlog: MALA Risk Screening & Surveillance System

All user stories are strictly derived from and traced back to domain requirements and user pain points identified at Chiangrai Prachanukroh Hospital network.

---

## 1. Traceability Backlog Matrix

| Story ID | User Role | User Story (As a... I want to... So that...) | Priority | Traced User Pain |
|:---:|---|---|:---:|---|
| **US-01** | Village Health Volunteer (อสม.) | **As a** VHV, **I want to** submit a patient's current weight and binary risk checklist (acute dehydration, alcohol use) via a lightweight mobile form, **so that** community field data is rapidly synced to the clinic without paper delays. | High | **Manual & Delayed Reporting:** Field data relies on slow paper forms, leading to outdated patient records at primary health units. |
| **US-02** | Primary Care Nurse (รพ.สต.) | **As a** Primary Care Nurse, **I want** the system to automatically fetch height and the latest renal function labs (eGFR) from the central hospital HIS, **so that** I do not need to perform duplicate manual data entry. | High | **Fragmented Hospital Systems:** Patient records reside across disconnected hospital databases, requiring laborious manual cross-checking. |
| **US-03** | Primary Care Nurse (รพ.สต.) | **As a** Primary Care Nurse, **I want** the Risk Engine to compute BMI and evaluate risk tiers (Green / Yellow / Red) automatically, **so that** triage is standardized and immune to human calculation errors. | High | **Lack of Quantitative Stratification:** No automated multidimensional risk scoring currently exists, leaving high-risk cases unnoticed. |
| **US-04** | Primary Care Nurse (รพ.สต.) | **As a** Primary Care Nurse, **I want to** view embedded Clinical Practice Guidelines (CPG) on the evaluation screen for low-risk cases, **so that** I can provide immediate lifestyle counseling and resolve the encounter locally. | Medium | **Non-Actionable Alerts:** Legacy alerts simply flag that a risk exists without offering actionable clinical steps on what to do next. |
| **US-05** | Primary Care Nurse (รพ.สต.) | **As a** Primary Care Nurse, **I want to** trigger an immediate emergency escalation (visual pop-up + sound alert) to attending physicians for Red Tier cases, **so that** acute interventions occur before fatal complications arise. | High | **Referral Bottlenecks:** Delays in escalating critical patients who are at acute risk of life-threatening MALA acidosis. |
| **US-06** | Hospital Physician (รพ.ศูนย์) | **As an** Attending Physician, **I want to** review high-risk patient profiles and execute a Metformin dosage adjustment/cessation order directly, **so that** updated prescriptions sync back seamlessly to primary care. | High | **Alarm Fatigue & Broken Loops:** Doctors face notification fatigue from non-critical alerts and lack an integrated channel to log prescription adjustments. |
| **US-07** | Primary Care Nurse (รพ.สต.) | **As a** Primary Care Nurse, **I want to** monitor a centralized surveillance dashboard tracking screening origins (which volunteer collected the data), **so that** we can track screening coverage for non-attending diabetic patients. | Medium | **Community Follow-Up Gaps:** Difficult to track whether patients who miss hospital appointments have been actively monitored in the field. |

---

## 2. Acceptance Criteria (Definition of Done)

- **US-01 (Community Data Capture):**
  - Mobile view renders responsive inputs with large tap targets.
  - Weight input automatically calculates and validates BMI bounds.
  - Binary switches for: (1) Temporary dehydration / diarrhea, (2) Alcohol consumption.
  - Submission queues the record in the assigned sub-district clinic within 2 seconds.

- **US-02 & US-03 (Data Integration & Risk Stratification):**
  - System calls HIS integration endpoint using Citizen ID to retrieve latest `eGFR` and `height`.
  - Risk Stratification:
    - **Green (Low Risk):** Normal eGFR, no acute lifestyle risk factors.
    - **Yellow (Moderate Risk):** Borderline eGFR or isolated moderate lifestyle factor.
    - **Red (High Risk / Critical):** Severely reduced eGFR, high Metformin dose, or presence of acute dehydration/alcoholism.

- **US-04 & US-05 (Clinical Decision Support & Escalation):**
  - Green Tier displays verified CPG counseling directives.
  - Red Tier generates an audible prompt and browser modal pop-up on the physician portal.

- **US-06 (Closed-Loop Prescription Action):**
  - Physician portal restricts medication adjustments strictly to certified physician logins (Role-Based Access Control).
  - Adjustments (`REDUCE_DOSE`, `DISCONTINUE`) record an audit trail log and sync back to the clinic dashboard.
