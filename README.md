# MALA Risk Screening & Monitoring System
A digital platform designed to assess and monitor the risk of Metformin-Associated Lactic Acidosis (MALA) in diabetic patients with Chronic Kidney Disease (CKD). The system ensures seamless collaboration between Village Health Volunteers (VHVs) via a Mobile Web App, Health Center Staff via Tablets/Web, and Hospital Doctors. It utilizes an AI-driven Risk Engine to analyze health data (eGFR), behavior, and medication history to provide real-time risk alerts and clinical decision support.

---
# MALA Risk Screening & Alert System

Metformin-Associated Lactic Acidosis (MALA) screening at the point of dispensing.

Presented by Warangkana Ngandee, Drug Information Center, Chiangrai Prachanukroh Hospital.

## Overview

A web application that lets **PCU Staff** (Primary Care Unit Staff, เจ้าหน้าที่ รพ.สต.) screen every diabetic patient who receives Metformin, at the moment of dispensing.

The system pulls the patient's latest eGFR from the hospital database, combines it with a few items the staff member enters (weight, height, alcohol use, symptoms, medication habits), calculates a risk result immediately, and alerts the hospital's multidisciplinary team when risk exceeds the defined threshold. Every screening is stored in a central database so it can be audited later.

## Contents

- [Problem](#problem)
- [Target users](#target-users)
- [Approach](#approach)
- [Key features](#key-features)
- [Screening criteria](#screening-criteria)
- [Screening questionnaire and report fields](#screening-questionnaire-and-report-fields)
- [System workflow](#system-workflow)
- [Expected benefits](#expected-benefits)
- [Cost and rollout](#cost-and-rollout)
- [Data protection](#data-protection)
- [Open items](#open-items)

## Problem

Hospitals send Metformin to Primary Care Units (PCUs, รพ.สต.) so that stable, uncomplicated diabetic patients can collect it close to home and reduce hospital crowding. In this process, the staff who dispense the drug at the PCU **cannot see the patient's eGFR** and **do not collect other risk-screening data** (weight, height, alcohol use) that help predict lactic acidosis. Patients therefore receive Metformin without any risk screening.

MALA is a severe complication of Metformin use and is costly to treat: roughly 30,000–90,000+ THB per case in public hospitals and 100,000–300,000+ THB per case in private hospitals, depending on ICU stay and the number of dialysis sessions.

**Current process:** Hospital ships Metformin → PCU receives and dispenses → patient receives the drug with no risk screening.

## Target users

1. **Diabetic patients taking Metformin**: patients in the PCU catchment area who receive Metformin continuously, especially higher-risk groups such as the elderly and those with reduced kidney function.
2. **PCU Staff: Primary Care Unit Staff (เจ้าหน้าที่ รพ.สต.)**, the primary users of the web application. They dispense the drug and meet patients directly, so they perform the screening, record the data, and give the individualized advice the app recommends.
3. **Hospital multidisciplinary team**: physicians, pharmacists, and nurses responsible for monitoring and for clinical decisions when a high-risk alert is raised.

## Approach

PCU Staff record each patient's risk factors at the point of dispensing through the web application. Data goes to a central database, the system calculates risk against standard criteria, and an automatic alert is sent when risk exceeds the threshold.

**Goal:** every time Metformin is dispensed through a PCU, risk is screened and recorded systematically, completely, and in a way that can be reviewed afterwards.

## Key features

### Step-by-step screening form (mobile-friendly)

A five-step flow designed for one-handed use, with large buttons and one question per screen:

1. **Scan HN**: the camera reads the HN from the patient's record book or card; the app asks "Is this HN correct?" before filling it in, reducing typing errors.
2. **Weight**: numeric keypad, showing the previous weight so staff can see the trend.
3. **Height**: entered once and reused at later visits; BMI is calculated automatically.
4. **Alcohol use**: chosen from clearly defined levels instead of free text, so data is comparable across all PCUs.
5. **Review and submit**: one summary screen. After submission the system saves the record and calculates the score within seconds. The whole process should take no more than 1–2 minutes per patient.

### Automatic eGFR retrieval

The latest eGFR (with its test date) is pulled from the hospital database by HN, so staff do not need to open another system or enter the value.

### Risk engine (AI) and result screen

On submission the system combines hospital data (eGFR) with the staff-entered data and:

- checks the Metformin dose against the CPG (see [Metformin dose vs. eGFR](#metformin-dose-vs-egfr-source-cpg)),
- calculates the MALA risk score (see [Risk score](#risk-score-source-research)),
- shows the score with the main contributing factors, and
- generates **individualized advice** from the entered risk factors, which staff can read to the patient or print as a take-home sheet. Advice emphasizes: avoid alcohol, the **Sick Day Rule** (stop the drug and contact the PCU when vomiting, diarrhea, or poor intake), warning symptoms that require going to the hospital immediately, adequate fluids, avoiding NSAIDs and unregulated medicines, and a repeat kidney-function check.

### Automatic alert to the multidisciplinary team

When the risk exceeds the threshold, or the Metformin dose is inappropriate for the eGFR, the system sends an alert card to the team's LINE group. The card shows the patient, screening site and time, eGFR, BMI and weight, alcohol use, and the system's suggested action (for example, review the Metformin dose and repeat eGFR within 4 weeks). It includes **"Open case"** and **"Acknowledged"** buttons so the team can see who is handling it. Every alert is stored in the central database. This closes the communication gap between the PCU and the hospital within minutes, while the patient is still at the PCU.

When risk is normal, the result is simply saved to the database, follow-up continues on the usual schedule, and the PCU Staff give the advice the app recommends.

### Offline fallback

If the internet is unavailable, staff fill in a paper form with the same items. When the connection returns, they photograph the form and upload it; the AI reads the form, records the data, and calculates the score and alert exactly as with on-screen entry.

### Report

A tabular report of all screenings, with the columns listed in [Report columns](#report-columns).

## Screening criteria

### Metformin dose vs. eGFR (source: CPG)

| eGFR (mL/min/1.73 m²) | Maximum Metformin dose |
| --- | --- |
| ≥ 45 | ≤ 2,000 mg/day |
| 30–44 | ≤ 1,000 mg/day |
| < 30 | Contraindicated, do not use |

- **Criterion:** dose not appropriate per CPG.
- **Management:** consult the physician to adjust the dose.
- **System action:** automatic alert to the responsible physician. An eGFR below 30 should always be raised as the highest-priority alert.

### Risk score (source: research)

| Factor | Points |
| --- | --- |
| eGFR < 60 | 1 |
| BMI < 23 | 2 |
| Heavy alcohol use | 2 |

- **Criterion:** total score ≥ 2 means the patient is at risk of MALA.
- **Management:** close monitoring; adjust the dose or stop Metformin; advise stopping alcohol; advise the Sick Day Rule.
- **System action:** store the at-risk patient's data for clinical management and prompt the screener to advise the Sick Day Rule.

### Triggering factors (source: research)

| Factor | Criterion | Management | System action |
| --- | --- | --- | --- |
| History of vomiting or diarrhea | Yes | Advise the Sick Day Rule | Prompt the screener to advise the Sick Day Rule |
| Recent poor oral intake / temporary undernutrition | Yes | Advise the Sick Day Rule | Prompt the screener; optional chatbot that gives advice or acts as a consultant when the patient is unwell |

### Factors that can induce acute kidney injury (source: evidence-based)

| Factor | Criterion | Management | System action |
| --- | --- | --- | --- |
| Use of NSAID painkillers or "yaa chud" | Yes | Advise the Sick Day Rule | Prompt the screener; record the data to support local management of inappropriate health-product use |
| Herbal remedies or dietary supplements | Yes | Advise the Sick Day Rule | Same as above |

## Screening questionnaire and report fields

### Questions

The same items are used on the paper form and in the web app.

| # | Question | Answer |
| --- | --- | --- |
| 1 | HN (Chiangrai Prachanukroh Hospital) | Scanned / entered |
| 2 | Full name | Pulled after HN confirmation |
| 3 | Sex | Male / Female |
| 4 | Age (years) | Number |
| 5 | Weight (kg) | Number |
| 6 | Height (cm) | Number (reused after first entry) |
| 7 | Latest eGFR in HIS | Pulled automatically |
| 8 | Current Metformin dose (mg/day) | Number |
| 9.1 | Does the patient drink alcohol? | Yes / No |
| 9.2.1 | Heavy drinking in a short period? (Male > 5 standard drinks, Female > 4 standard drinks, within 2 hours) | Yes / No |
| 9.2.2 | Regular or continuous heavy drinking? (Male > 15 standard drinks/week, Female > 8 standard drinks/week) | Yes / No |
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
| Dose not appropriate | System (dose rule) |
| eGFR < 60 | System |
| BMI < 23 | System |
| Alcohol use | Q9.2.1 / Q9.2.2 |
| At risk of MALA | System (risk score) |
| Dehydration (vomiting/diarrhea) | Q10 |
| Poor oral intake | Q11 |
| NSAIDs | Q12 |
| Herbal / supplements | Q13 |
| Screening site | System |

## System workflow

1. **Screening**: PCU Staff scan the HN and enter weight, height, alcohol use, and the symptom and medication questions at the point of dispensing.
2. **Central database**: the screening record (with who entered it and when) is saved for every patient.
3. **Risk assessment**: the system fetches the latest eGFR, checks the dose against the CPG, and calculates the score against the standard criteria.
4. **Alert or record**: if risk exceeds the threshold, an alert goes to the LINE group and the team opens a case; if risk is normal, the result is saved and staff give the individualized advice shown on screen.
5. **Follow-up**: the PCU and the hospital see the same data and follow up the patient accordingly.

## Expected benefits

1. **Prevent severe complications**: screen for MALA risk from the start, before harm occurs.
2. **Complete, auditable data**: every dispensing has a screening record in the central database for continuing care planning.
3. **Timely automatic alerts**: less manual assessment for staff; risk is flagged as soon as it is found.
4. **Connected data across units**: the PCU and the hospital see the same dataset, reducing communication errors.

## Cost and rollout

- **Running cost:** server rental, domain, and AI token usage, in the range of thousands of baht, compared with MALA treatment costs of hundreds of thousands of baht per case.
- **Phase 1:** pilot at PCUs within Mueang district, Chiang Rai.
- **Next:** extend the system to the whole provincial network.

## Data protection

The system handles identifiable patient data and sends patient names to a messaging group, so it should follow the Personal Data Protection Act (PDPA) and hospital policy: encrypted storage and transfer, access limited to authorized staff, and a log of every entry and alert (who and when). Confirm with the hospital whether patient names and HN may be posted in LINE groups, or whether the alert should show only HN or a masked name.

## Open items

1. **Score scale.** The criteria workbook scores 0–5 with an alert at ≥ 2, while the demo slides show a score of 72 against a threshold of 60. This document follows the workbook; the demo screens need to be changed to match, or a conversion to a 0–100 scale defined.
2. **Definition of "heavy alcohol use" (2 points).** The workbook gives no exact rule. Suggested: Yes to Q9.2.1 or Q9.2.2. The demo's four-level alcohol selector should be aligned with Q9.1 to 9.2.2.
3. **Missing demo fields.** The demo screens do not yet include Metformin dose (Q8, needed for the CPG rule), age and sex, or Questions 10 to 13.
4. **eGFR range wording.** The demo labels the range 30–45; the CPG range used here is 30–44.
5. **Incidence figure.** The slide states 137–871 MALA cases per 100,000 people per year, which looks high compared with the usual literature; verify the source before presenting.
6. **Previous draft features removed.** The earlier draft described village health volunteers, a three-colour dashboard, physician orders with one-click dose logging, and audible pop-up alerts. None of these are in the requirements and they were removed; they can return as future phases if wanted.
