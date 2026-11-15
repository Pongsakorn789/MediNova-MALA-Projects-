# Product Backlog: MALA Risk Screening & Monitoring System

**Version:** 1.1 (revises the original backlog)
**Date Revised:** November 15, 2026

All user stories are derived from the domain requirements and user pain points identified at the Chiangrai Prachanukroh Hospital network, and from the design changes agreed with the mentor. The acceptance criteria follow the prototype at https://mala-project-2026.web.app as checked on November 15, 2026.

> **Scope note:** The prototype implements the **SHPH (รพ.สต.) screening flow** in full. The **physician** and **MALA case manager** screens are **mockups** only (display and routing). The hospital takes them over and implements the real functions.

---

## Changes from the original backlog

| Area | Original | v1.1 |
|---|---|---|
| Roles | Community data collector, primary care nurse, physician | **SHPH staff**, **Physician (mockup)**, **MALA case manager / administrator (mockup, one role)** |
| Order of checks | Risk tiers (Green / Yellow / Red) from several factors | **Dose check first** (Part 1); MALA assessment (Part 2) only if the dose check passes |
| Risk output | Three tiers | Dose pass / fail, then Part 2 additive score (**at risk when score ≥ 2**) |
| Where a case goes | Red tier → physician with pop-up and sound | Dose fail → **doctor's queue immediately**; at risk → **MALA case manager queue** |
| Physician actions | Dose order, audit trail, role-based access | Out of this prototype (hospital implements) |
| Separate community data-capture form | A mobile form for field data | Not in this scope; all data is entered by SHPH staff in one flow |
| Patient identification | Citizen ID fetch | Scan HN, list of patients with search, Excel import, **manual entry (date of birth → age calculated)** |

---

## 1. Traceability Backlog Matrix

| Story ID | User Role | User Story (As a... I want to... So that...) | Priority | Traced User Pain |
|:---:|---|---|:---:|---|
| **US-01** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want to** identify the patient by scanning the HN, picking from the patient list or search box, importing a list from Excel, or entering the patient by hand with the date of birth, **so that** I can start a screening quickly even when a code cannot be scanned. | High | **Manual & Delayed Reporting:** Patient lookup relies on paper records and phone calls. |
| **US-02** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want** the system to bring the patient's latest eGFR and current Metformin dose with the record, **so that** I do not need to do duplicate manual entry. | High | **Fragmented Hospital Systems:** Patient records reside across disconnected databases, requiring manual cross-checking. |
| **US-03** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want** the system to check the Metformin dose first (eGFR ≥ 45 and ≤ 2000 mg/day) and, if it fails, stop the assessment and send the case to the doctor's queue immediately, **so that** an unsafe dose reaches the doctor before any further assessment. | High | **Referral Bottlenecks:** Delays in escalating patients on an unsafe Metformin dose. |
| **US-04** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want to** record today's weight, height (BMI calculated automatically), the alcohol questions, and the symptom and medicine questions (vomiting or diarrhea, poor intake, painkillers or ยาชุด, herbal medicine), **so that** the MALA assessment uses complete and consistent data. | High | **Manual & Delayed Reporting:** Data collected on paper is incomplete and outdated. |
| **US-05** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want** the system to calculate the MALA Part 2 score automatically and show it with its reason on the review screen (eGFR < 60 = 1, BMI < 23 = 2, heavy alcohol = 2; at risk when ≥ 2), with the symptom and medicine answers shown but not scored, **so that** triage is standardized and free of calculation errors. | High | **Lack of Quantitative Stratification:** No automated risk scoring exists, so at-risk patients go unnoticed. |
| **US-06** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want** an at-risk case to go to the MALA case manager queue after I save the screening, **so that** I do not have to send it by hand and the case is not lost. | High | **Referral Bottlenecks:** Manual referral steps delay follow-up. |
| **US-07** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want to** see guidance and the Sick Day Rule warning on the result screen, **so that** I can counsel patients who are not at risk right away. | Medium | **Non-Actionable Alerts:** Legacy alerts flag a risk without saying what to do next. |
| **US-08** | Physician (mockup) | **As a** physician, **I want** a queue that holds the dose-fail cases sent from the PCU, **so that** I see the cases that need immediate action. | High | **Alarm Fatigue:** Doctors receive too many non-critical alerts. |
| **US-09** | MALA case manager (mockup) | **As a** MALA case manager, **I want to** see a queue of at-risk cases ("คิวติดตาม"), **so that** the hospital team can take over the follow-up. | High | **Community Follow-Up Gaps:** At-risk patients are hard to track after screening. |
| **US-10** | MALA case manager / administrator (mockup) | **As a** MALA case manager / administrator, **I want to** view a dashboard and download a CSV report of screening results and risk patterns by area, **so that** I can monitor screening activity. | Medium | **Lack of Visibility:** No overview of screening results and risk factors across areas. |

Role note: the MALA case manager and the system administrator are **one role**. The case manager **receives cases and views the dashboard**; care planning, LINE alerts, follow-up and closing the case are done by the hospital and are outside this prototype.

---

## 2. Acceptance Criteria (Definition of Done)

- **US-01 (Patient identification):**
  - Step 1 of 5 offers camera scan, a list of patients with a search box (name, HN or ID card), Excel import, and manual entry.
  - The manual form takes name, HN / ID card number, date of birth, sex, area / village, latest eGFR and Metformin dose per day; the system calculates the age from the date of birth.
  - The patient card shows name, HN, age, previous risk level and latest eGFR, and the user can confirm ("ถูกต้อง · ถัดไป") or choose "สแกนใหม่".

- **US-02 (eGFR and dose with the record):**
  - The latest eGFR is shown on the patient card, and the Metformin dose on record is used by the dose check without manual entry.

- **US-03 (Dose check first):**
  - **Pass:** eGFR ≥ 45 **and** Metformin ≤ 2000 mg/day, and the flow continues to step 2 (weight).
  - **Fail:** the screen shows "ขนาดยาเกินเกณฑ์ · ส่งแพทย์ทันที", the MALA assessment is stopped, and the case is added to the doctor's queue ("บันทึกเข้าคิวแพทย์แล้ว").
  - The system records and forwards the case only; it does not order the drug stopped in place of the doctor.
  - A dose-fail case does not enter the case manager queue.

- **US-04 (Data entry):**
  - Weight uses a numeric keypad and shows the previous weight and its date.
  - Height is remembered from the last visit and BMI is calculated immediately.
  - Alcohol: Q9 (drinks or not), and when yes, Q9.2.1 (heavy drinking within a short time) and Q9.2.2 (regular heavy drinking), with thresholds shown for the patient's sex.
  - Q10 vomiting or diarrhea, Q11 unable to eat or eating less for 1–2 days, Q12 painkillers / anti-inflammatories / ยาชุด, Q13 herbal medicine / supplements, each Yes / No.
  - A review screen shows all entered data before saving.

- **US-05 (MALA Part 2 score):**
  - Score = (eGFR < 60 → 1) + (BMI < 23 → 2) + (heavy alcohol → 2); **score ≥ 2 = at risk**.
  - The review screen shows the Part 1 result, the Part 2 score with its reason (for example "4 คะแนน · เข้าเกณฑ์เสี่ยง"), and states that the number of "yes" answers is not the score.
  - Q10–Q13 are shown as answers and do not change the score.

- **US-06 (Routing of at-risk cases):**
  - After saving, an at-risk case goes to the case manager queue and does **not** go to the doctor.
  - The SHPH screen has no "send to doctor" button, no doctor report and no LINE button.
  - Exact queue contents and status wording are to be verified on the prototype.

- **US-07 (Guidance):**
  - A case that is not at risk is saved with guidance and is not queued for the case manager.
  - Guidance is **rule-based** (fixed rules, no trained AI model). It covers reviewing eGFR and dose, advice for heavy drinking, advice during illness or fasting, and the use of painkillers or herbal medicine.
  - When vomiting or diarrhea is present, the Sick Day Rule warning is shown.
  - "เสร็จสิ้น · สแกนผู้ป่วยรายถัดไป" returns to step 1 with no data left from the previous patient.

- **US-08 (Physician mockup):**
  - The physician screen lists dose-fail cases; at-risk cases do not appear there.

- **US-09 (Case manager mockup):**
  - Login with the role "ผู้ดูแลเคส MALA" opens "คิวติดตามกลุ่มเสี่ยง MALA" marked "ผู้ดูแลเคส · ไม่ใช่คิวแพทย์", with the tabs "แดชบอร์ด/รายงาน" and "คิวติดตาม" (with a count of waiting cases).

- **US-10 (Dashboard, mockup):**
  - Shows dose checks (Part 1): total, passed, and sent to the doctor.
  - Shows MALA assessments (Part 2): total, how many at risk, and the share at risk (for example 9 of 10 = 90%).
  - Shows "ผู้ป่วยที่มีผลบันทึก": share of listed patients with a recorded result (for example 12 of 30 = 40%). The mentor left the definition of "% of patients the system helped" open.
  - Shows the areas with at-risk cases (most cases first, then by village) and community guidance counts for painkillers / anti-inflammatories and herbal / supplements.
  - The main indicators count only Part 1 and Part 2; nausea / vomiting is not included.
  - A "ดาวน์โหลด CSV" button exports the data.

---

## 3. Known Issues to Fix on the Prototype

- The review screen shows "บันทึกโดย อสม. ประจำหมู่ 1" and one dashboard guidance text refers to อสม.; replace with PCU Staff wording.
- The patient list still shows the old three-level labels ("ความเสี่ยงต่ำ / เสี่ยงปานกลาง / เสี่ยงสูง"), which are not part of the new Part 1 / Part 2 flow.
- Confirm the dose rule for eGFR 30–44 (the dose-fail screen says that below 30 the drug should be stopped; the agreed design fails any eGFR below 45).

---

## 4. Out of Scope for the Prototype (Hospital Implements)

- Dose adjustment / discontinuation order and its audit trail
- Digital signature
- Audible alarm and alarm settings
- Role-based access control
- Full patient list with search and filters on the physician screen
- Real LINE connection (optional, may not be connected)
- Case manager follow-up: care plan, follow-up, closing the case
