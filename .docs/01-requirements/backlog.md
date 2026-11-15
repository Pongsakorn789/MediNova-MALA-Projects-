# Product Backlog: MALA Risk Screening & Surveillance System

**Version:** 1.1 (revises the original backlog)
**Date Revised:** November 15, 2026

All user stories are derived from the domain requirements and user pain points identified at the Chiangrai Prachanukroh Hospital network, and from the design changes agreed with the mentor.

> **Scope note:** The prototype implements the **SHPH (รพ.สต.) screening flow** in full. The **physician** and **MALA case manager** screens are **mockups** only (display and routing). The hospital takes them over and implements the real functions.

---

## Changes from the original backlog

| Area | Original | v1.1 |
|---|---|---|
| Roles | Village volunteer, primary care nurse, physician | **SHPH staff**, **Physician (mockup)**, **MALA case manager / administrator (mockup, one role)** |
| Order of checks | Risk tiers (Green / Yellow / Red) from several factors | **Dose check first** (Part 1); MALA assessment (Part 2) only if the dose check passes |
| Risk output | Three tiers | Dose pass / fail, then Part 2 additive score (**at risk when score ≥ 2**) |
| Where a case goes | Red tier → physician with pop-up and sound | Dose fail → **doctor alerted immediately**; at risk → **case manager queue automatically after saving** |
| Physician actions | Dose order, audit trail, role-based access | Out of this prototype (hospital implements) |
| Community data capture | Separate mobile form for volunteers | Removed from this scope |
| Patient entry | Citizen ID fetch | Scan HN, **manual entry (HN + date of birth, age calculated)**, search, mock mode |

---

## 1. Traceability Backlog Matrix

| Story ID | User Role | User Story (As a... I want to... So that...) | Priority | Traced User Pain |
|:---:|---|---|:---:|---|
| **US-01** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want to** identify the patient by scanning the HN, typing the HN and date of birth, searching by name or ID card, or using demo mode, **so that** I can start a screening quickly even when a code cannot be scanned. | High | **Manual & Delayed Reporting:** Patient lookup relies on paper records and phone calls. |
| **US-02** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want** the system to pull the patient's latest eGFR and current Metformin dose from the recorded data, **so that** I do not need to do duplicate manual entry. | High | **Fragmented Hospital Systems:** Patient records reside across disconnected databases, requiring manual cross-checking. |
| **US-03** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want** the system to check the Metformin dose first (eGFR ≥ 45 and ≤ 2000 mg/day) and, if it fails, tell me to stop the drug, close the case and alert the doctor immediately, **so that** an unsafe dose is stopped before any further assessment. | High | **Referral Bottlenecks:** Delays in escalating patients on an unsafe Metformin dose. |
| **US-04** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want to** record today's weight, height (BMI calculated automatically), alcohol level, sick-day symptoms, and AKI / NSAID / herbal / ยาชุด factors, **so that** the MALA assessment uses complete and consistent data. | High | **Manual & Delayed Reporting:** Data collected on paper is incomplete and outdated. |
| **US-05** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want** the system to calculate the MALA Part 2 score automatically (eGFR < 60 = 1, BMI < 23 = 2, heavy alcohol = 2; at risk when ≥ 2) and show Parts 3–4 as found / not found without scoring them, **so that** triage is standardized and free of calculation errors. | High | **Lack of Quantitative Stratification:** No automated risk scoring exists, so at-risk patients go unnoticed. |
| **US-06** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want** an at-risk case to enter the MALA case manager queue automatically as soon as I save the screening, **so that** I do not have to send it by hand and the case is not lost. | High | **Referral Bottlenecks:** Manual referral steps delay follow-up. |
| **US-07** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want to** see guidance and the Sick Day Rule warning on the result screen, **so that** I can counsel patients who are not at risk right away. | Medium | **Non-Actionable Alerts:** Legacy alerts flag a risk without saying what to do next. |
| **US-08** | Physician (mockup) | **As a** physician, **I want** a dashboard that shows only dose-fail cases with an urgent alert, **so that** I see only the cases that need immediate action. | High | **Alarm Fatigue:** Doctors receive too many non-critical alerts. |
| **US-09** | MALA case manager (mockup) | **As a** MALA case manager, **I want to** see a queue of at-risk cases and open a case to view patient information, the itemized Part 2 score and the factors found, **so that** the hospital team can take over the follow-up. | High | **Community Follow-Up Gaps:** At-risk patients are hard to track after screening. |
| **US-10** | MALA case manager / administrator (mockup) | **As a** MALA case manager / administrator, **I want to** view system metrics, **so that** I can monitor screening activity and risk patterns by area. | Medium | **Lack of Visibility:** No overview of screening results and risk factors across areas. |

Role note: the MALA case manager and the system administrator are **one role**. The case manager **receives and opens cases only**; care planning, LINE alerts, follow-up and closing the case are done by the hospital and are outside this prototype.

---

## 2. Acceptance Criteria (Definition of Done)

- **US-01 (Patient identification):**
  - Step 1 offers camera scan, manual entry, search by name or ID card, and demo mode.
  - Manual entry takes the HN and the date of birth; the system calculates the age and shows it.
  - An unknown HN, or a date of birth that is in the future or invalid, is rejected.
  - The user can confirm the patient or choose "สแกนใหม่" without carrying over the wrong patient.

- **US-02 (Auto-pull of clinical data):**
  - The latest eGFR and current Metformin dose appear on the confirmation card with no manual entry.

- **US-03 (Dose check first):**
  - **Pass:** eGFR ≥ 45 **and** Metformin ≤ 2000 mg/day, and the flow continues to the MALA assessment.
  - **Fail:** eGFR < 45 **or** dose > 2000 mg/day. The screen shows "stop the drug", the MALA assessment is skipped, the case is closed, and the doctor is alerted immediately.
  - eGFR < 30 is flagged as the most severe.
  - A dose-fail case never appears in the case manager queue.

- **US-04 (Data entry):**
  - Weight is entered with the numeric keypad and shows the change from the previous weight.
  - Height is entered and BMI is calculated automatically.
  - Alcohol has 4 levels: none / occasional / regular / heavy.
  - Sick-day symptoms (dehydration / diarrhea) and AKI / NSAID / herbal / ยาชุด are recorded as present or absent.
  - A review screen shows all entered data before saving.

- **US-05 (MALA Part 2 score):**
  - Score = (eGFR < 60 → 1) + (BMI < 23 → 2) + (heavy alcohol → 2); **score ≥ 2 = at risk**.
  - Parts 3 and 4 are displayed as "พบ / ไม่พบ" and do not change the score.
  - Contributing factors are listed on the result screen.

- **US-06 (Routing of at-risk cases):**
  - An at-risk case appears in the case manager queue with status "รอดำเนินการ" right after saving, without any button press.
  - It does **not** appear on the physician dashboard.
  - The SHPH screen has no "send to doctor" button, no doctor report and no LINE button.

- **US-07 (Guidance):**
  - A case that is not at risk is saved and shows guidance; it is not queued for the case manager.
  - When sick-day symptoms are present, the "กฎวันป่วย (Sick Day Rule)" warning card is shown.
  - "เสร็จสิ้น · สแกนผู้ป่วยรายถัดไป" returns to Step 1 with no data left from the previous patient.

- **US-08 (Physician mockup):**
  - The dashboard "ภาพรวมการเฝ้าระวัง MALA" lists only dose-fail cases, with an urgent alert and an unread notification.
  - At-risk cases do not appear.

- **US-09 (Case manager mockup):**
  - Login with the role "ผู้ดูแลเคส MALA" opens the case queue.
  - Opening a case shows patient information, the itemized Part 2 score and the factors found.
  - Opening a case does not send anything to the doctor.

- **US-10 (Dashboard metrics, mockup):**
  - Shows the number of dose screenings and the number and % of dose-fail cases.
  - Shows the number of MALA assessments and the % at risk.
  - Shows NSAID / herbal / ยาชุด use by area, counted both by patient address and by screening รพ.สต.
  - "% of patients the system helped" uses the team's working definition (dose-fail cases where the doctor adjusted or stopped the drug ÷ all dose-fail cases) because the mentor left the exact definition open.

---

## 3. Out of Scope for the Prototype (Hospital Implements)

- Dose adjustment / discontinuation order and its audit trail
- Digital signature
- Audible alarm and alarm settings
- Role-based access control
- Full patient list with search and filters on the physician screen
- Real LINE connection (optional, may not be connected)
- Case manager follow-up: care plan, follow-up, closing the case
