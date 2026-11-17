# MALA Risk Screening & Monitoring System (MRSS)

Metformin-Associated Lactic Acidosis (MALA) screening at the point of dispensing.

Presented by MediNova.

**Version:** 1.1 (revised November 15, 2026) · Prototype: https://mala-project-2026.web.app

## Overview

A web application that lets **PCU Staff** (Primary Care Unit Staff, เจ้าหน้าที่ รพ.สต.) screen every diabetic patient who receives Metformin, at the moment of dispensing.

The system holds each patient's latest eGFR and current Metformin dose and **checks the dose first**. If the dose is unsafe, the screening stops, the case is sent to the doctor's queue immediately, and the MALA assessment is skipped. If the dose is safe, the staff member enters weight, height and a few answers (alcohol use, symptoms, medicines), and the system calculates the MALA risk score. A patient at risk is placed in the **MALA case manager** queue. Every screening is stored so it can be audited later.

> **Prototype scope:** the PCU Staff screening flow is fully implemented. The **physician** and **MALA case manager** screens are **mockups** (display and routing); the hospital takes them over and implements the real functions.

## Contents

- [Problem](#problem)
- [Target users](#target-users)
- [Approach](#approach)
- [Key features](#key-features)
- [Screening criteria](#screening-criteria)
- [Screening questionnaire and report fields](#screening-questionnaire-and-report-fields)
- [System workflow](#system-workflow)
- [Case manager dashboard](#case-manager-dashboard)
- [Expected benefits](#expected-benefits)
- [Cost and rollout](#cost-and-rollout)
- [Data protection](#data-protection)
- [Testing](#testing)
- [Open items](#open-items)

## Problem

Hospitals send Metformin to Primary Care Units (PCUs, รพ.สต.) so that stable, uncomplicated diabetic patients can collect it close to home and reduce hospital crowding. In this process, the staff who dispense the drug at the PCU **cannot see the patient's eGFR** and **do not collect other risk-screening data** (weight, height, alcohol use) that help predict lactic acidosis. Patients therefore receive Metformin without any risk screening.

MALA is a severe complication of Metformin use and is costly to treat: roughly 30,000–90,000+ THB per case in public hospitals and 100,000–300,000+ THB per case in private hospitals, depending on ICU stay and the number of dialysis sessions. *Source: briefing slides provided by the hospital mentor at the start of the project (see [Sources of figures](#sources-of-figures)).* Thai case reports also describe hemodialysis (2 to 6 sessions) and hospital stays of 6 to 12 days per patient [1][2]; they report no cost in baht.

**Current process:** Hospital ships Metformin → PCU receives and dispenses → patient receives the drug with no risk screening.

## Target users

1. **Diabetic patients taking Metformin**: patients in the PCU catchment area who receive Metformin continuously, especially higher-risk groups such as the elderly and those with reduced kidney function.
2. **PCU Staff: Primary Care Unit Staff (เจ้าหน้าที่ รพ.สต.)**, the primary users of the web application. They dispense the drug and meet patients directly, so they perform the screening, record the data, and give the individualized advice the app recommends.
3. **Physician / multidisciplinary team (mockup):** receives every **dose-fail** case in the doctor's queue. The real functions (dose orders and so on) are implemented by the hospital.
4. **MALA case manager (mockup):** receives the at-risk cases and sees the dashboard and report ("คิวติดตามกลุ่มเสี่ยง MALA", marked "ผู้ดูแลเคส · ไม่ใช่คิวแพทย์"). The case manager is also the system administrator (one role). Follow-up after receiving a case is done by the hospital.

## Approach

PCU Staff record each patient's risk factors at the point of dispensing through the web application. The system applies the criteria in a fixed order: **first the dose check (Part 1), then the MALA risk score (Part 2)**. Each result is routed to the right place.

**Goal:** every time Metformin is dispensed through a PCU, risk is screened and recorded systematically, completely, and in a way that can be reviewed afterwards.

## Key features

### Step-by-step screening form (mobile-friendly)

A five-step flow designed for one-handed use, with large buttons and one question group per screen:

1. **Identify the patient** (step 1 of 5): scan the HN with the camera, pick the patient from the list (search by name, HN or ID card), import a patient list from an Excel file, or add a patient by hand. The manual form takes name, HN / ID card number, **date of birth (the system calculates the age)**, sex, area / village, latest eGFR and Metformin dose per day. The app shows the patient card (name, HN, age, previous risk level, latest eGFR) and asks "ถูกต้อง · ถัดไป" or "สแกนใหม่" before continuing.
   **Dose check (Part 1)** runs right after the patient is confirmed. If it fails, the flow stops here (see [Part 1](#part-1-metformin-dose-check-source-cpg)).
2. **Weight** (step 2): numeric keypad, showing the previous weight and its date so staff can see the trend.
3. **Height** (step 3): remembered from the last visit; BMI is calculated immediately.
4. **Risk factors** (step 4): alcohol questions (Q9, Q9.2.1, Q9.2.2, with thresholds for the patient's sex) and the symptom and medicine questions (Q10–Q13), all answered Yes / No.
5. **Review and save** (step 5): one summary screen showing the patient, eGFR, weight, height, BMI, the Part 1 dose result, the **Part 2 score with its reason**, and all answers. After "บันทึกและประเมินความเสี่ยง" the system saves the record. The whole process should take no more than 1–2 minutes per patient.

### Automatic eGFR and dose

The latest eGFR (with its test date) and the current Metformin dose come with the patient record, so staff do not need to open another system or type the values.

### Risk engine and result screen

- **Part 1, dose check:** eGFR ≥ 45 **and** Metformin ≤ 2,000 mg/day = pass. Otherwise = fail (see below).
- **Part 2, MALA risk score:** eGFR < 60 = 1 point, BMI < 23 = 2 points, heavy alcohol use = 2 points. **Score ≥ 2 = at risk.** The number of "yes" answers is not the score.
- **Symptom and medicine answers (Q10–Q13)** are shown with the result. They are **not scored**.
- The result gives **individualized advice** that staff can read to the patient. Advice emphasizes: avoid alcohol, the **Sick Day Rule** (stop the drug and contact the PCU when vomiting, diarrhea, or poor intake), warning symptoms that require going to the hospital immediately, adequate fluids, avoiding NSAIDs and unregulated medicines, and a repeat kidney-function check.

### Routing of results

| Result | What the system does |
| --- | --- |
| **Dose check fails** | Screen shows "ขนาดยาเกินเกณฑ์ · ส่งแพทย์ทันที": the **MALA assessment is stopped** and the case is **added to the doctor's queue right away** ("บันทึกเข้าคิวแพทย์แล้ว"). The system records and forwards the case only; it does not order the drug stopped in place of the doctor, and staff follow the doctor's instruction. |
| **Dose passes, score ≥ 2 (at risk)** | The case goes to the **MALA case manager queue** ("คิวติดตาม"), not to the doctor. The PCU Staff screen has no "send to doctor" button and no doctor report. |
| **Dose passes, score < 2** | The result is saved, follow-up continues on the usual schedule, and staff give the advice shown on screen. |

### LINE alert

The LINE alert is **optional** and is handled on the case manager side by the hospital. It may not be connected in the prototype, and there is no LINE button for PCU Staff.

### Offline fallback (to be verified)

If the internet is unavailable, staff would fill in a paper form with the same items and enter it when the connection returns. Offline save with automatic sync, and AI reading of a photographed form, have not been seen in the prototype.

### Report

A tabular report of all screenings, with the columns listed in [Report columns](#report-columns). The case manager dashboard has a "ดาวน์โหลด CSV" button.

## Screening criteria

### Part 1: Metformin dose check (source: CPG)

| Condition | Result |
| --- | --- |
| eGFR ≥ 45 **and** Metformin ≤ 2,000 mg/day | **Pass**, continue to the MALA assessment |
| eGFR < 45 **or** Metformin > 2,000 mg/day | **Fail** |

- **On fail:** stop the assessment, send the case to the doctor's queue immediately, and follow the doctor's instruction on the dose.
- An eGFR below 30 is the most severe case (the screen says that below 30 the drug should be stopped and the doctor informed).

### Part 2: MALA risk score (source: research)

| Factor | Points |
| --- | --- |
| eGFR < 60 | 1 |
| BMI < 23 | 2 |
| Heavy alcohol use | 2 |

- **Criterion:** total score ≥ 2 means the patient is at risk of MALA.
- **Management:** close monitoring; adjust the dose or stop Metformin; advise stopping alcohol; advise the Sick Day Rule.
- **System action:** the case goes to the MALA case manager queue, and the screener is prompted to advise the Sick Day Rule.

### Triggering factors (source: research) — shown, not scored

| Factor | Criterion | Management | System action |
| --- | --- | --- | --- |
| History of vomiting or diarrhea | Yes | Advise the Sick Day Rule | Prompt the screener to advise the Sick Day Rule |
| Recent poor oral intake / temporary undernutrition | Yes | Advise the Sick Day Rule | Same as above |

### Factors that can induce acute kidney injury (source: evidence-based) — shown, not scored

| Factor | Criterion | Management | System action |
| --- | --- | --- | --- |
| Use of NSAID painkillers or "yaa chud" | Yes | Advise the Sick Day Rule | Prompt the screener; record the data to support local management of inappropriate health-product use |
| Herbal remedies or dietary supplements | Yes | Advise the Sick Day Rule | Same as above |

## Screening questionnaire and report fields

### Questions

The same items are used on the paper form and in the web app.

| # | Question | Answer |
| --- | --- | --- |
| 1 | HN (Chiangrai Prachanukroh Hospital) | Scanned / chosen from the list / typed |
| 2 | Full name | Pulled after HN confirmation |
| 3 | Sex | Male / Female |
| 4 | Age (years) | Pulled with the record; in manual entry calculated from the date of birth |
| 5 | Weight (kg) | Number |
| 6 | Height (cm) | Number (reused after first entry) |
| 7 | Latest eGFR in HIS | Pulled automatically |
| 8 | Current Metformin dose (mg/day) | Pulled from the recorded data |
| 9 | Does the patient drink alcohol? | Yes / No |
| 9.2.1 | Heavy drinking in a short period? (Male ≥ 5 standard drinks, Female ≥ 4 standard drinks, within 2 hours) | Yes / No |
| 9.2.2 | Regular or continuous heavy drinking? (Male ≥ 15 standard drinks/week, Female ≥ 8 standard drinks/week) | Yes / No |
| 10 | Recent vomiting or diarrhea? | Yes / No |
| 11 | Unable to eat, or eating much less, for 1–2 days in a row? | Yes / No |
| 12 | Takes painkillers, anti-inflammatories, or "yaa chud"? | Yes / No |
| 13 | Takes herbal decoctions, herbs, or dietary supplements? | Yes / No |

Questions 9.2.1 and 9.2.2 are supported by a standard-drink conversion table (1 standard drink = 10 g alcohol), for example one small can of beer (330 mL), one 30 mL shot of spirits, or one glass of wine (100–120 mL), plus common package sizes for beer, white spirits, whisky, wine, and local liquor.

### Report columns

| Column | Source |
| --- | --- |
| Screening date | System |
| HN | Q1 |
| Name | Q2 |
| Sex | Q3 |
| Age | Q4 |
| BMI | Calculated from Q5 and Q6 |
| eGFR | Q7 |
| Metformin dose | Q8 |
| Dose check result (pass / fail) | System (Part 1) |
| eGFR < 60 | System |
| BMI < 23 | System |
| Alcohol use | Q9 / Q9.2.1 / Q9.2.2 |
| Part 2 score and at risk of MALA | System |
| Dehydration (vomiting/diarrhea) | Q10 |
| Poor oral intake | Q11 |
| NSAIDs | Q12 |
| Herbal / supplements | Q13 |
| Routed to | System (doctor queue / case manager queue / none) |
| Screening site | System |

## System workflow

1. **Identify the patient**: PCU Staff scan the HN, pick the patient from the list, import a list, or add the patient by hand, then confirm the patient.
2. **Dose check (Part 1)**: the system uses the eGFR and Metformin dose on record and applies the CPG rule.
   - **Fail:** the MALA assessment stops and the case goes to the doctor's queue immediately.
   - **Pass:** continue.
3. **Data entry**: staff enter weight, height, alcohol use, and the symptom and medicine answers.
4. **MALA assessment (Part 2)**: the system calculates the score; the review screen shows it with its reason. Score ≥ 2 = at risk.
5. **Routing**: an at-risk case goes to the MALA case manager queue; a case that is not at risk is saved and staff give the advice shown on screen.
6. **Follow-up**: the hospital (physician and case manager) takes over the follow-up of the cases it receives; these screens are mockups in the prototype.
7. **Central database**: every screening record (with who entered it and when) is saved.

## Case manager dashboard

The case manager screen "คิวติดตามกลุ่มเสี่ยง MALA" has two tabs: "แดชบอร์ด/รายงาน" and "คิวติดตาม" (with a count of waiting cases). The dashboard shows (mockup):

- **ตรวจขนาดยา · Part 1:** number of dose checks, how many passed and how many were sent to the doctor for review
- **ประเมินความเสี่ยง · Part 2:** number of assessments and how many scored as at risk
- **สัดส่วน Part 2 ที่เข้าเกณฑ์เสี่ยง:** share at risk (for example 9 of 10 = 90%)
- **ผู้ป่วยที่มีผลบันทึก:** share of the listed patients with a recorded result (for example 12 of 30 = 40%)
- **พื้นที่ที่พบเคสเข้าเกณฑ์เสี่ยง:** area with the most at-risk cases, then by village
- **Community guidance:** how many times painkillers / anti-inflammatories and herbal / supplements were reported
- The main indicators count only Part 1 and Part 2; nausea / vomiting is not included. A "ดาวน์โหลด CSV" button exports the data.

## Expected benefits

1. **Prevent severe complications**: screen for MALA risk from the start, before harm occurs.
2. **Unsafe doses stopped early**: the dose check runs first, so a dose-fail patient is not assessed any further and the doctor receives the case at once.
3. **Complete, auditable data**: every dispensing has a screening record in the central database for continuing care planning.
4. **Automatic routing**: less manual work for staff; at-risk cases reach the case manager without an extra step.
5. **Connected data across units**: the PCU and the hospital see the same dataset, reducing communication errors.

## Cost and rollout

- **Running cost:** server rental and domain. The system uses fixed rules and no AI model, so there is no AI usage cost. The team has not measured the running cost in baht.
- **Phase 1:** pilot at PCUs within Mueang district, Chiang Rai.
- **Next:** extend the system to the whole provincial network.

## Data protection

The system handles identifiable patient data, so it should follow the Personal Data Protection Act (PDPA) and hospital policy: encrypted storage and transfer, access limited to authorized staff, and a log of every entry and alert (who and when). If the LINE alert is connected, confirm with the hospital whether patient names and HN may be posted in LINE groups, or whether the alert should show only HN or a masked name.

## Testing

- **Test plan:** [Test_Plan_MediNova.md](Test_Plan_MediNova.md) (version 1.3, revised November 16, 2026). This round covers the PCU Staff role only; the physician and case manager screens are checked only for what they display and where cases arrive.
- **UAT results:** [UAT_Results_MediNova.md](UAT_Results_MediNova.md). Participants are identified by codes U01–U09 only; most are proxy users, not PCU Staff. The sessions were held mostly on November 13, 2026, before the November 15–16 prototype update, so the results describe the earlier flow, not the final one. See the file for the limitations.
- **SUS / TAM:** [sus_tam_results.md](sus_tam_results.md), with the anonymised raw responses in [sus_tam_responses.csv](sus_tam_responses.csv).
- **Demo account:** the login page of the prototype pre-fills a demo PCU Staff account. It is for the prototype only and must not exist in a production system (see [.docs/rule.md](.docs/rule.md), Section 4).

## Open items

1. **Metformin dose rule for eGFR 30–44.** The CPG table allowed up to 1,000 mg/day at eGFR 30–44. The agreed design fails the dose check whenever eGFR < 45, while the dose-fail screen says that below 30 the drug should be stopped. Confirm the intended rule with the mentor.
2. **Definition of "heavy alcohol use" (2 points).** The prototype scored 2 points when regular heavy drinking (Q9.2.2) was answered Yes. Confirm with the mentor that Yes to Q9.2.1 or Q9.2.2 is the intended rule.
3. **Old three-level labels.** The patient list still shows "ความเสี่ยงต่ำ / เสี่ยงปานกลาง / เสี่ยงสูง" from an earlier version; these labels are not part of the Part 1 / Part 2 flow and should be removed or renamed.
4. **Volunteer wording on screen.** The review screen still shows "บันทึกโดย อสม. ประจำหมู่ 1" and one dashboard guidance text refers to อสม.; replace with PCU Staff wording.
5. **% of patients the system helped.** The mentor left the definition open; the dashboard currently shows "ผู้ป่วยที่มีผลบันทึก" (share of listed patients with a recorded result) instead.
6. **Offline fallback and camera OCR.** Not yet confirmed in the prototype.
7. **Incidence figure.** The slide states 137–871 MALA cases per 100,000 people per year, which looks high compared with the usual literature; verify the source before presenting.
8. **Features not in this prototype.** The three-colour tier dashboard, the physician's one-click dose logging, and audible pop-up alerts are not part of this prototype; the hospital may add them later.

## Sources of figures

| Figure | Source |
| --- | --- |
| MALA treatment cost: 30,000–90,000+ THB (public) and 100,000–300,000+ THB (private) per case | Briefing slides from the hospital mentor (Drug Information Center, Chiangrai Prachanukroh Hospital), provided at the start of the project. Not independently verified by the team. |
| Hemodialysis sessions and hospital stay in Thai MALA cases | References [1] and [2] below |

## References

1. Duangmatphon K. (2021). การพยาบาลผู้ป่วย Metformin associated lactic acidosis ที่รับการรักษาโดยการฟอกเลือดด้วยเครื่องไตเทียม: กรณีศึกษา 2 ราย. *Maha Sarakham Hospital Journal*, 17(3), 13–22. https://he02.tci-thaijo.org/index.php/MKHJ/article/view/244303 (hemodialysis 6 and 2 sessions; hospital stay 12 and 6 days)
2. Soottiprapar U. (2023–2024). การพยาบาลผู้ป่วย metformin associated lactic acidosis (MALA) ที่ได้รับการรักษาด้วยการฟอกเลือดด้วยเครื่องไตเทียม กรณีศึกษา 2 ราย. *Academic Journal of Mahasarakham Provincial Public Health Office*, 8(15). https://thaidj.org/index.php/AJMP/article/download/15462/12251 (2 hemodialysis sessions each; hospital stay 10 days each)
