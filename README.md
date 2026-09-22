# MALA Risk Screening & Monitoring System
A digital platform designed to assess and monitor the risk of Metformin-Associated Lactic Acidosis (MALA) in diabetic patients with Chronic Kidney Disease (CKD). The system ensures seamless collaboration between Village Health Volunteers (VHVs) via a Mobile Web App, Health Center Staff via Tablets/Web, and Hospital Doctors. It utilizes an AI-driven Risk Engine to analyze health data (eGFR), behavior, and medication history to provide real-time risk alerts and clinical decision support.

---

## 1. Problem Statement
Type 2 diabetes patients undergoing Metformin therapy face severe health risks—specifically Metformin-Associated Lactic Acidosis (MALA), a life-threatening complication. This is typically triggered when impaired renal function (reduced eGFR), acute dehydration, or heavy alcohol consumption is present.

Currently, the healthcare network faces critical operational challenges

Manual & Fragmented Data Verification: Patient data is scattered across multiple hospital information systems (HIS) and primary care databases, requiring laborious manual cross-referencing.
Lack of Quantitative Risk Stratification: Legacy alert systems lack clear, multidimensional risk scoring that combines dynamic lifestyle factors with hospital lab results.
Non-Actionable Alerts & Alarm Fatigue: Existing notifications merely flag risk without providing actionable clinical pathways (CPG).
Delayed Community Follow-ups: Patients in remote communities are often tracked too late when acute complications have already developed.

---

## 2. Target Users
1. Village Health Volunteers (VHV / อสม.): Frontline field workers responsible for capturing dynamic community-level data (e.g., current weight, acute dehydration status, alcohol consumption) via mobile devices.
2. Health Center Staff: Nurses and public health workers managing walk-in screenings and medication dispensaries.
3. Doctors & Multidisciplinary Teams: Hospital physicians responsible for clinical interventions and medication adjustments.

---
## 3.Key Features
1. Smart Web-Based Screening Form: A user-friendly web interface requiring only weight and height inputs (with auto-calculated BMI) and responses to risk factor screening questions, such as alcohol consumption history, diarrhea/vomiting symptoms, and the use of painkillers or herbal medicines.
2. Auto-Data Integration: An HN scanning system that automatically retrieves the latest eGFR values and medication history from the central hospital database.
3. Risk Engine & Smart Alert: Calculates a Risk Score immediately upon data entry and displays the results on a color-coded Dashboard (Green/Yellow/Red).
4. Clinical Decision Support (CPG Integration): Synthesizes personalized recommendations for low to moderate-risk patients, enabling Health Center staff to manage and advise patients immediately on-site.
5. High-Priority Escalation: Sends alerts to the multidisciplinary team's LINE group specifically for critical (Red) patients, allowing doctors to review and directly log dosage adjustments.

---

## 4.System Workflow
1. Screening: VHVs or Health Center staff input vital signs and risk behaviors into the web app.
2. AI Analysis: The system pulls recent eGFR data, computes the MALA Risk Score, and categorizes the patient.
3. Alert & Intervention: High-risk profiles trigger an instant LINE alert. The doctor reviews the AI Summary and submits a Physician Order (maintain, reduce, or hold medication).
4. Monitoring: The Health Center receives the intervention protocol, prints personalized advice, and manages patient follow-up.

---

## 5. User Requirements & Validation
| Stakeholder Group | Core Operational Pain Point | Validated Solution Requirement |
| --- | --- | --- |
| **Village Health Volunteers (VHV / อสม.)** | Complex reporting methods lead to delayed reporting and errors in the field. Volunteers need an ultra-streamlined workflow without clinical calculations. | **Smart Web-Based Screening:** Lightweight interface requiring only current weight and binary checklists (dehydration, alcohol intake). Automated BMI computation. |
| **Sub-district Health Promoting Hospital (SHPH / รพ.สต.)** | Data is scattered across multiple hospital systems. Cross-referencing serum creatinine and eGFR manually is time-consuming and error-prone. | **Auto-Data Integration & Triage:** Automatic fetching of patient height and latest eGFR from central hospital records; multi-tier risk dashboard (Green/Yellow/Red). |
| **Sub-district Health Promoting Hospital (SHPH / รพ.สต.)** | Alerts lack actionable next steps, causing uncertainty whether to escalate or handle locally. | **CPG Integration:** Standard Clinical Practice Guidelines embedded within low-risk screens so primary staff can resolve cases locally. |
| **Hospital Physicians (รพ.ศูนย์)** | Alarm fatigue caused by undifferentiated notifications; critical high-risk MALA cases are not prioritized urgently. | **High-Priority Escalation:** Targeted real-time audible and pop-up notifications restricted strictly to critical (Red Tier) patients. |
| **Hospital Physicians (รพ.ศูนย์)** | Fragmented clinical context when reviewing prescription risks remotely. | **Closed-Loop Action:** Direct access to renal trends and lifestyle triggers with a one-click Metformin dosage adjustment log synced back to primary care. |

---

## 6. Legal & Compliance
1. PDPA / Data Privacy: All Protected Health Information (PHI) is encrypted and processed in strict accordance with the Personal Data Protection Act.
2. Role-Based Access Control (RBAC): System enforces strict data minimization. VHVs only access necessary field data, while sensitive clinical records are restricted to nurses and physicians.
3. Audit Trail: Every data entry, risk calculation, and physician order is time-stamped and logged to ensure complete clinical accountability.
