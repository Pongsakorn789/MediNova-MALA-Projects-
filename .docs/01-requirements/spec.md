# Specification: MALA Risk Screening & Monitoring System (MRSS)

**Team:** MediNova
**Version:** 1.0 (draft)
**Prototype:** https://mala-project-2026.web.app
**Sources:** `backlog.md` (v1.3), `ai-logic.md`, `Test_Plan_MediNova.md` (v1.3)

> **AI Disclosure:** AI helped draft this specification from the team's backlog, AI logic note and Test Plan. Requirements are copied or summarised from those files. The team must check them against the prototype and code before submission. Items marked **[CONFIRM]** are not verified.

---

## 1. Purpose

Screen every diabetic patient who receives Metformin at a Primary Care Unit (PCU, รพ.สต.) for the risk of metformin-associated lactic acidosis (MALA), at the moment of dispensing. Today the PCU staff cannot see the patient's eGFR and collect no other risk data, so patients receive Metformin with no screening.

**Goal:** every Metformin dispensing through a PCU is screened and recorded in a way that can be reviewed afterwards.

## 2. Scope

| Role | Status in the prototype |
|---|---|
| **เจ้าหน้าที่ รพ.สต.** (SHPH staff / PCU Staff) | Fully implemented: the screening flow |
| **แพทย์ / ทีมสหวิชาชีพ** (Physician / team) | Mockup (display and routing only) |
| **ผู้ดูแลเคส MALA** (MALA case manager / administrator, one role) | Mockup (queue, dashboard, CSV); oversees Metformin dose use [CONFIRM: see note below] |

There is no volunteer role. The screening is done only by เจ้าหน้าที่ รพ.สต.

> **[CONFIRM] Dose control by the case manager:** the team states that ผู้ดูแลเคส MALA views the dashboard and controls Metformin dose use. The backlog and Test Plan describe only a queue, a dashboard, a CSV export and a "จัดการแล้ว" button for this role, and say that care planning and follow-up are done by the hospital. Decide whether dose control is (a) a function of the prototype, in which case add a requirement and acceptance criteria, or (b) a responsibility of the hospital outside the prototype, in which case say so here. The system itself never orders a dose change in place of the doctor.

The hospital takes over the physician and case manager screens and implements the real functions.

**Out of scope:** dose order and audit trail, digital signature, audible alarm, role-based access control, full patient list with filters on the physician screen, real LINE connection (optional, may not be connected), case manager follow-up (care plan, follow-up, closing the case).

## 3. Users

| Role | Needs | Stories |
|---|---|---|
| เจ้าหน้าที่ รพ.สต. | Identify the patient, see eGFR and dose, screen in 1–2 minutes, advise the patient | US-01 to US-07 |
| Physician (mockup) | Receive dose-fail cases | US-08 |
| ผู้ดูแลเคส MALA (mockup) | Receive at-risk cases, view dashboard, export CSV, oversee Metformin dose use [CONFIRM] | US-09, US-10 |

## 4. Business rules

The system applies the rules in a fixed order: Part 1 first, then Part 2 only if Part 1 passes.

### 4.1 Part 1: Metformin dose check (source: CPG table)

| Condition | Result |
|---|---|
| eGFR < 30 | Contraindicated: stop the assessment; the screen says to stop Metformin and inform the doctor |
| eGFR 30–44 and dose <= 1,000 mg/day | Pass |
| eGFR 30–44 and dose > 1,000 mg/day | Fail: doctor reviews the dose |
| eGFR >= 45 and dose <= 2,000 mg/day | Pass |
| eGFR >= 45 and dose > 2,000 mg/day | Fail: doctor reviews the dose |
| Dose cannot be read | Check the current dose |

On fail: the MALA assessment stops and the case is added to the **doctor's queue** immediately. The system only records and forwards; staff follow the doctor's order.

### 4.2 Part 2: MALA risk score (source: research)

| Factor | Points |
|---|---|
| eGFR < 60 | 1 |
| BMI > 0 and BMI < 23 | 2 |
| Heavy alcohol use (heavy drinking in a short time, or high weekly amount) | 2 |

**Score >= 2 = at risk.** The number of "yes" answers is not the score. Vomiting or diarrhea, poor intake, painkillers / ยาชุด and herbal remedies (Q10–Q13) are shown but **not scored**.

### 4.3 Routing

| Result | Destination |
|---|---|
| Part 1 fails | Doctor's queue, immediately |
| Part 1 passes, score >= 2 | MALA case manager queue, automatically after saving (not the doctor) |
| Part 1 passes, score < 2 | Saved with guidance; not queued |

The SHPH screen has no "send to doctor" button, no doctor report and no LINE button.

### 4.4 Guidance (rule-based)

The backend selects fixed advice text for each matching condition. It does **not** use a trained AI model and does not generate text with a language model. The endpoint name `/api/evaluate-mala-ai` is a legacy name; the API returns `guidanceType: "rule-based"`.

| Condition | Advice |
|---|---|
| eGFR < 45 | Doctor reviews suitability and dose of Metformin |
| Heavy or chronic alcohol use | Avoid heavy drinking; consult health personnel |
| Vomiting / diarrhea, or fasting | Contact the PCU / doctor for Sick Day Rule advice |
| Painkillers / anti-inflammatories / ยาชุด | Consult doctor or pharmacist with the medicine list |
| Herbal remedies or supplements | Tell health personnel and check the product |
| None of the above | General advice and tell health personnel when ill |

## 5. Functional requirements

| ID | Requirement | Story |
|---|---|---|
| FR-01 | Identify the patient by camera scan of the HN, by choosing from the list (30 test patients) with search by name, HN, ID card number or Patient ID, by Excel import, or by manual entry. | US-01 |
| FR-02 | Manual entry takes name, HN / ID card number, date of birth (age calculated), sex, area / village, latest eGFR and Metformin dose per day. Save is enabled only when required fields are filled. | US-01 |
| FR-03 | Excel import reads HN, name, sex, age and eGFR in the browser (no backend). Missing data (date of birth, weight, height, dose) is entered afterwards. | US-01 |
| FR-04 | Show the patient card (name, HN, age, previous risk level, latest eGFR) with "ถูกต้อง · ถัดไป" and "สแกนใหม่". | US-01 |
| FR-05 | Use the recorded eGFR and Metformin dose without manual entry (dose is entered by hand for Excel imports). | US-02 |
| FR-06 | Run the Part 1 dose check when the patient is confirmed; on fail show "ขนาดยาเกินเกณฑ์ · ส่งแพทย์ทันที", stop the assessment and add the case to the doctor's queue. | US-03 |
| FR-07 | Weight by numeric keypad, with the previous weight and date shown. Height is remembered, BMI is calculated immediately. | US-04 |
| FR-08 | Alcohol questions Q9, Q9.2.1 and Q9.2.2 with thresholds for the patient's sex; symptom and medicine questions Q10–Q13 (Yes / No). | US-04 |
| FR-09 | Review screen before saving: patient, eGFR, weight, height, BMI, Part 1 result with reason, Part 2 score with reason, all answers. | US-04, US-05 |
| FR-10 | Calculate the Part 2 score automatically and show it with the scoring rule. Show the number of "yes" answers separately, stating that it is not the score. | US-05 |
| FR-11 | After saving, send an at-risk case to the case manager queue and show the banner "ส่งเข้าคิวผู้ดูแลเคส MALA แล้ว". | US-06 |
| FR-12 | Result screen with four cards (Part 1 dose, Part 2 score, Part 3 Sick Day Rules, Part 4 AKI triggers / medicine review), the patient advice card and the Sick Day Rule warning when vomiting or diarrhea is present. | US-07 |
| FR-13 | "เสร็จสิ้น · สแกนผู้ป่วยรายถัดไป" returns to step 1 with no data left from the previous patient. | US-07 |
| FR-14 | Store every screening with who entered it and when. | all |
| FR-15 | Physician screen lists dose-fail cases only (mockup; to be verified). | US-08 |
| FR-16 | Case manager screen "คิวติดตามกลุ่มเสี่ยง MALA": queue tabs "รอดำเนินการ" and "จัดการแล้ว", search, case detail with history, "จัดการแล้ว" button (display only). | US-09 |
| FR-17 | Dashboard: Part 1 counts, Part 2 counts and share at risk, "ผู้ป่วยที่มีผลบันทึก", areas with at-risk cases, painkiller and herbal counts, and "ดาวน์โหลด CSV". | US-10 |

**Flow (5 steps):** 1 patient -> 2 weight -> 3 height -> 4 risk factors -> 5 review and save. Target: no more than 1–2 minutes per patient.

## 6. Data

**API:** `POST /api/evaluate-mala-ai` (JSON from the result step)

| Field | Meaning |
|---|---|
| `age` | Age in years (received, not used by the advice rules) |
| `egfr` | eGFR value |
| `metforminDose` | Dose in mg/day (received, not used by the advice rules) |
| `hasBingeDrinking` | Heavy drinking in a short time |
| `hasChronicAlcohol` | High weekly alcohol amount |
| `hasVomitingDiarrhea` | Vomiting or diarrhea |
| `hasFasting` | Fasting or eating much less |
| `hasPainkillers` | Painkillers / anti-inflammatories / ยาชุด |
| `hasHerbsSupplements` | Herbal remedies or supplements |

**Report columns (CSV):** screening date, HN, name, sex, age, BMI, eGFR, Metformin dose, dose check result, eGFR < 60, BMI < 23, alcohol use, Part 2 score and at-risk flag, vomiting/diarrhea, poor intake, NSAIDs, herbal/supplements, routed to, screening site.

**Code:** `src/utils/metforminGuideline.ts` (Part 1), `src/utils/riskEngine.ts` (Part 2), `backend/main.py` (advice).

## 7. Non-functional requirements

- **Device:** mobile-friendly, usable with one hand, large buttons, one question group per screen.
- **Speed:** a screening should take no more than 1–2 minutes per patient. (UAT median for the full screening was about 100 seconds, see `UAT_Results_MediNova.md`.)
- **Traceability:** rule-based results can be traced back to the input data and the code rules.
- **Data protection:** identifiable patient data; follow PDPA and hospital policy; restricted access and a log of entries. The prototype has no consent step yet.
- **Test data:** tests use invented patients only.

## 8. Acceptance and testing

Acceptance criteria for each story are in `backlog.md`, Section 2. Test cases TC01–TC36 and UAT scenarios UAT01–UAT07 are in `Test_Plan_MediNova.md`. Exit criteria: task success >= 70% per task, SUS >= 68, TAM >= 3.5 / 5, no open Critical bugs.

## 9. Known issues and open items

| # | Item |
|---|---|
| 1 | Old three-level labels ("ความเสี่ยงต่ำ / ปานกลาง / สูง") still on the patient list and card. |
| 2 | The label "Yellow" appears in the case manager history, and `ai-logic.md` says the code still returns `Yellow` / `Green` levels. These are not part of the Part 1 / Part 2 flow. [CONFIRM: rename or remove in the final prototype.] |
| 3 | Wording from an earlier role is still on the review screen ("บันทึกโดย …") and one dashboard guidance card. Replace it with the logged-in user ID (for example SHPH001) and "เจ้าหน้าที่ รพ.สต." or health personnel. |
| 4 | "มก." shown twice in the Metformin line; sparkle icon on the advice card although advice is rule-based. |
| 5 | Dashboard denominator is fixed at 30 and ignores patients added by hand or by Excel. |
| 6 | No consent step before screening. |
| 7 | Definition of "heavy alcohol use" (2 points): the code scores 2 for either binge or chronic drinking. [CONFIRM with the mentor.] |
| 8 | Exact messages for eGFR below 30 and for a dose above 2,000 mg/day at eGFR >= 45 not re-tested; offline save and camera scan not confirmed. |
| 9 | The rules need review by a clinical owner before use with real patients. Results are screening support, not a diagnosis or a treatment order. |
