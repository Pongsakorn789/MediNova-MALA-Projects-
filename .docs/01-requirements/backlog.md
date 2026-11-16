# Product Backlog: MALA Risk Screening & Monitoring System

**Version:** 1.3 (revises the original backlog)
**Date Revised:** November 16, 2026

All user stories are derived from the domain requirements and user pain points identified at the Chiangrai Prachanukroh Hospital network, and from the design changes agreed with the mentor. The acceptance criteria follow the prototype at https://mala-project-2026.web.app.

> **Scope note:** The prototype implements the **SHPH (รพ.สต.) screening flow** in full. The **physician** and **MALA case manager** screens are **mockups** only (display and routing). The hospital takes them over and implements the real functions.
>
> **Verification note:** On November 16, 2026 the SHPH screens (steps 1–5, the dose-fail screen and the result screen after saving) and the case manager screens (dashboard and queue) were re-checked on the live site. The physician screen and the exact messages for eGFR below 30 and for a dose above 2,000 mg/day at eGFR ≥ 45 were not re-tested and are marked "to be verified".

---

## Changes from the original backlog

| Area | Original | v1.2 |
|---|---|---|
| Roles | Community data collector, primary care nurse, physician | **SHPH staff**, **Physician (mockup)**, **MALA case manager / administrator (mockup, one role)** |
| Order of checks | Risk tiers (Green / Yellow / Red) from several factors | **Dose check first** (Part 1, CPG table, confirmed by the team); MALA assessment (Part 2) only if the dose check passes |
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
| **US-03** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want** the system to check the Metformin dose first against the CPG limit for the patient's eGFR and, if it fails, stop the assessment and send the case to the doctor's queue immediately, **so that** an unsafe dose reaches the doctor before any further assessment. | High | **Referral Bottlenecks:** Delays in escalating patients on an unsafe Metformin dose. |
| **US-04** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want to** record today's weight, height (BMI calculated automatically), the alcohol questions, and the symptom and medicine questions (vomiting or diarrhea, poor intake, painkillers or ยาชุด, herbal medicine), **so that** the MALA assessment uses complete and consistent data. | High | **Manual & Delayed Reporting:** Data collected on paper is incomplete and outdated. |
| **US-05** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want** the system to calculate the MALA Part 2 score automatically and show it with its reason on the review screen (eGFR < 60 = 1, BMI < 23 = 2, heavy alcohol = 2; at risk when ≥ 2), with the symptom and medicine answers shown but not scored, **so that** triage is standardized and free of calculation errors. | High | **Lack of Quantitative Stratification:** No automated risk scoring exists, so at-risk patients go unnoticed. |
| **US-06** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want** an at-risk case to go to the MALA case manager queue after I save the screening, **so that** I do not have to send it by hand and the case is not lost. | High | **Referral Bottlenecks:** Manual referral steps delay follow-up. |
| **US-07** | SHPH staff (รพ.สต.) | **As an** SHPH staff member, **I want to** see guidance and the Sick Day Rule warning on the result screen, **so that** I can counsel patients who are not at risk right away. | Medium | **Non-Actionable Alerts:** Legacy alerts flag a risk without saying what to do next. |
| **US-08** | Physician (mockup) | **As a** physician, **I want** a queue that holds the dose-fail cases sent from the SHPH, **so that** I see the cases that need immediate action. | High | **Alarm Fatigue:** Doctors receive too many non-critical alerts. |
| **US-09** | MALA case manager (mockup) | **As a** MALA case manager, **I want to** see a queue of at-risk cases ("คิวติดตาม"), **so that** the hospital team can take over the follow-up. | High | **Community Follow-Up Gaps:** At-risk patients are hard to track after screening. |
| **US-10** | MALA case manager / administrator (mockup) | **As a** MALA case manager / administrator, **I want to** view a dashboard and download a CSV report of screening results and risk patterns by area, **so that** I can monitor screening activity. | Medium | **Lack of Visibility:** No overview of screening results and risk factors across areas. |

Role note: the MALA case manager and the system administrator are **one role**. The case manager **receives cases and views the dashboard**; care planning, LINE alerts, follow-up and closing the case are done by the hospital and are outside this prototype.

---

## 2. Acceptance Criteria (Definition of Done)

The screening flow has 5 steps: 1 scan HN, 2 weight, 3 height, 4 risk factors, 5 review. The screen title is "คัดกรองความเสี่ยง Metformin ณ จุดจ่ายยา".

- **US-01 (Patient identification):**
  - Step 1 of 5 ("สแกน HN ผู้ป่วย") offers camera scan (the browser asks for camera permission), a search link ("ค้นหาด้วยชื่อ HN หรือเลขบัตรแทน") that opens the patient list with a search box (name, HN, ID card number or Patient ID), Excel import, and manual entry ("+ ไม่พบรายชื่อ? เพิ่มผู้ป่วยด้วยการกรอกมือ").
  - The patient list shows the number of patients (30 in the prototype data), each with name, HN and age.
  - **Excel import:** the system reads HN, name, sex, age and eGFR from the file. If the file has an area column the area is used per row, otherwise the "backup area / SHPH" typed on the screen is used for the whole file. Data the file does not have (date of birth, previous area, weight, height, Metformin dose) must be entered before assessment. The file is read in the browser and is not sent to a backend.
  - **Manual form:** name, HN / ID card number, date of birth, sex, area / village, latest eGFR and Metformin dose per day; the system calculates the age from the date of birth and the save button is enabled only when the required fields are filled.
  - The patient card shows name, HN, age, previous risk level ("ระดับความเสี่ยงเดิม") and latest eGFR, and the user can confirm ("ถูกต้อง · ถัดไป") or choose "สแกนใหม่".

- **US-02 (eGFR and dose with the record):**
  - The latest eGFR is shown on the patient card, and the Metformin dose on record is used by the dose check without manual entry. When a patient is imported from Excel, the dose is entered by hand.

- **US-03 (Dose check first, CPG table):**
  - The check runs when the user confirms the patient in step 1.
  - **Pass:**
    - eGFR ≥ 45 and Metformin ≤ 2,000 mg/day, or
    - eGFR 30–44 and Metformin ≤ 1,000 mg/day.
    - The flow continues to step 2 (weight). On the review screen Part 1 reads "ขนาดยาอยู่ในเกณฑ์" with the reason, for example "eGFR 30–44 และขนาดยาไม่เกิน 1,000 มก./วัน ตาม CPG ที่ให้มา".
  - **Fail:** the screen shows "ขนาดยาเกินเกณฑ์ · ส่งแพทย์ทันที" with the reason (for example "eGFR 30–44 แต่ขนาดยา 2,000 มก./วัน ซึ่งเกินเกณฑ์ไม่เกิน 1,000 มก./วัน; แจ้งแพทย์ผู้รับผิดชอบทบทวน"), the MALA assessment is stopped ("หยุดการประเมินความเสี่ยง MALA ไว้ก่อน"), and the case is added to the doctor's queue ("บันทึกเข้าคิวแพทย์แล้ว").
  - The screen tells staff to follow the doctor's order about stopping or adjusting the drug; the system records and forwards the case only.
  - A dose-fail case does not enter the case manager queue.
  - Not re-tested: the exact messages for eGFR below 30 and for a dose above 2,000 mg/day at eGFR ≥ 45 (to be verified). Both are expected to fail under the CPG table.

- **US-04 (Data entry):**
  - Step 2: weight uses a numeric keypad and shows the previous weight and its date (for example "ครั้งก่อน 58 กก. (12 มิ.ย. 2569)").
  - Step 3: height is entered once and remembered ("กรอกครั้งเดียวใช้ได้ตลอด"), and BMI is calculated immediately.
  - Step 4, alcohol: Q9 (drinks or not), and when yes, a question about heavy drinking in a short time (9.2.1) and a question about regular or continuous heavy drinking (9.2.2), with the threshold for the patient's sex shown (for example female: 4 standard drinks or more within 2 hours; 8 or more per week). 1 standard drink = 10 g of pure alcohol.
  - Step 4, symptoms and triggers: Q10 vomiting or diarrhea, Q11 unable to eat or eating less for 1–2 days, Q12 painkillers / anti-inflammatories / ยาชุด, Q13 herbal medicine / supplements, each Yes / No.
  - Step 5 ("ตรวจทานก่อนบันทึก") shows all entered data before saving.

- **US-05 (MALA Part 2 score):**
  - Score = (eGFR < 60 → 1) + (BMI < 23 → 2) + (heavy alcohol → 2); **score ≥ 2 = at risk**.
  - The review screen shows the Part 1 result and the Part 2 score with its reason (for example "1 คะแนน · ต่ำกว่าเกณฑ์เสี่ยง" or "4 คะแนน · เข้าเกณฑ์เสี่ยง") and the scoring line "eGFR <60 = 1 · BMI <23 = 2 · ดื่มหนัก = 2 · ตั้งแต่ 2 คะแนนถือว่าเสี่ยง".
  - A yellow summary line above the details shows how many answers are "yes" (for example "มีคำตอบใช่ 7 ข้อ") and says that this number is not the Part 2 score; the score is read by the criteria below it.
  - Q10–Q13 are shown as answers and do not change the score.

- **US-06 (Routing of at-risk cases):**
  - After saving ("บันทึกและประเมินความเสี่ยง"), an at-risk case goes to the case manager queue and does **not** go to the doctor.
  - The result screen shows a banner "ส่งเข้าคิวผู้ดูแลเคส MALA แล้ว" and says the case is not sent to the doctor's queue from the MALA score alone.
  - The SHPH screen has no "send to doctor" button, no doctor report and no LINE button.

- **US-07 (Guidance):**
  - A case that is not at risk is saved with guidance and is not queued for the case manager.
  - Guidance is **rule-based** (fixed rules, no trained AI model). It covers reviewing eGFR and dose, advice for heavy drinking, advice during illness or fasting, and the use of painkillers or herbal medicine.
  - When vomiting or diarrhea is present, the Sick Day Rule warning is shown.
  - "เสร็จสิ้น · สแกนผู้ป่วยรายถัดไป" returns to step 1 with no data left from the previous patient.
  - The result screen after saving (checked on November 16) shows four cards: Part 1 dose check (dose on record and eGFR), Part 2 score with the score of each criterion, Part 3 Sick Day Rules, and Part 4 AKI triggers / medicine review. A "คำแนะนำสำหรับผู้ป่วย" card follows, then the routing banner.

- **US-08 (Physician mockup):**
  - The physician screen lists dose-fail cases; at-risk cases do not appear there. To be verified.

- **US-09 (Case manager mockup):**
  - Login with the role "ผู้ดูแลเคส MALA" opens "คิวติดตามกลุ่มเสี่ยง MALA" marked "ผู้ดูแลเคส · ไม่ใช่คิวแพทย์", with the tabs "แดชบอร์ด/รายงาน" and "คิวติดตาม" (with a count of waiting cases).
  - The queue has the tabs "รอดำเนินการ" and "จัดการแล้ว", a search box, and for each case an expandable detail view, the screening history with time, and a "จัดการแล้ว" button that moves the case to the handled tab. This is display only; care planning and follow-up stay with the hospital. Checked on November 16.

- **US-10 (Dashboard, mockup):**
  - Shows dose checks (Part 1): total, passed, and sent to the doctor.
  - Shows MALA assessments (Part 2): total, how many at risk, and the share at risk (for example 3 of 3 = 100%).
  - Shows "ผู้ป่วยที่มีผลบันทึก": share of listed patients with a recorded result (for example 4 of 30 = 13%). The mentor left the definition of "% of patients the system helped" open.
  - Shows the areas with at-risk cases (most cases first, then by village) and community guidance counts for painkillers / anti-inflammatories and herbal / supplements.
  - The main indicators count only Part 1 and Part 2; nausea / vomiting is not included.
  - A "ดาวน์โหลด CSV" button exports the data. Checked on November 16.
  - The counts on the dashboard include every test run kept in the prototype. Clear the test data before UAT (see Section 3).

---

## 3. Known Issues to Fix on the Prototype

Checked on the live site on November 16, 2026. Fix these before the UAT on November 17 where possible.

- **Volunteer wording (อสม.) still shown in two places:** the review screen (step 5) shows "บันทึกโดย อสม. ประจำหมู่ 1" (replace with "บันทึกโดย" plus the logged-in user ID, for example SHPH001), and the dashboard guidance card for painkillers / anti-inflammatories says "ปรึกษา อสม. หรือบุคลากรสุขภาพ" (replace with SHPH staff or health personnel).
- **Old tier label:** the screening history on the case manager screen still shows the label "Yellow" from the old three-level system. Remove or rename it.
- **Old three-level labels:** the patient list and the patient card still show "ความเสี่ยงต่ำ / เสี่ยงปานกลาง / เสี่ยงสูง" and "ระดับความเสี่ยงเดิม", which are not part of the Part 1 / Part 2 flow.
- **Duplicate unit:** the Metformin line reads "1 เม็ด/วัน (500 มก.) มก./วัน" with "มก." twice.
- **Sparkle icon on the advice card:** the "คำแนะนำสำหรับผู้ป่วย" card uses a sparkle icon that suggests AI, but the advice is rule-based. Use a neutral icon.
- **Dashboard denominator:** "จาก 30 รายชื่อในระบบ" always uses 30 and ignores patients added by hand or by Excel import.
- **No consent step** before screening.
- **Test data in the prototype:** test patients (for example TEST-001, TEST-A, TEST-B) and the team's test runs are counted on the dashboard. Clear all of them before the UAT so that the numbers show only real test sessions.

---

## 4. Out of Scope for the Prototype (Hospital Implements)

- Dose adjustment / discontinuation order and its audit trail
- Digital signature
- Audible alarm and alarm settings
- Role-based access control
- Full patient list with search and filters on the physician screen
- Real LINE connection (optional, may not be connected)
- Case manager follow-up: care plan, follow-up, closing the case
