# MALA Risk Screening & Monitoring System
An integrated clinical decision support and risk screening platform for Metformin-Associated Lactic Acidosis (MALA) in diabetic patients, developed in collaboration with Chiangrai Prachanukroh Hospital.

---

## 1. Problem Statement
Type 2 diabetes patients undergoing Metformin therapy face severe health risks—specifically Metformin-Associated Lactic Acidosis (MALA), a life-threatening complication triggered when impaired renal function (reduced eGFR), acute dehydration, or heavy alcohol consumption is present. 

Currently, the healthcare network faces critical operational challenges:
- **Manual & Fragmented Data Verification:** Patient data is scattered across multiple hospital information systems (HIS) and primary care databases, requiring laborious manual cross-referencing that delays timely interventions.
- **Lack of Quantitative Risk Stratification:** Legacy alert systems lack clear, multidimensional risk scoring (e.g., combining dynamic lifestyle factors with static hospital lab results).
- **Non-Actionable Alerts & Alarm Fatigue:** Existing notifications merely flag risk without providing actionable clinical pathways (Clinical Practice Guidelines: CPG) based on severity.
- **Delayed Community Follow-ups:** Patients in remote communities who do not visit hospital centers frequently are tracked too late when acute complications have already developed.

---

## 2. Target Users
1. **Village Health Volunteers (VHV / อสม.):** Frontline field workers responsible for capturing dynamic community-level data (e.g., current weight, acute dehydration status, alcohol consumption) via mobile devices.
2. **Sub-district Health Promoting Hospital Staff (SHPH / รพ.สต. / Primary Care Nurses):** Primary healthcare gatekeepers responsible for triaging patients, reviewing integrated lab parameters, applying CPG recommendations, and escalating high-risk cases.
3. **Hospital Physicians / Endocrinologists (รพ.ศูนย์):** Clinical decision-makers who review critical escalations and execute immediate Metformin dosage modifications or discontinuation.

---

## 3. User Requirements & Validation (Derived from Domain Requirements)
Based on field requirements and stakeholder analysis conducted with Chiangrai Prachanukroh Hospital network, requirements have been validated across 3 primary user groups:

| Stakeholder Group | Core Operational Pain Point | Validated Solution Requirement |
|---|---|---|
| **Village Health Volunteers (VHV / อสม.)** | Complex reporting methods lead to delayed reporting and errors in the field. Volunteers need an ultra-streamlined workflow without clinical calculations. | **Smart Mobile Screening:** Lightweight interface requiring only current weight and binary checklists (dehydration, alcohol intake). Automated BMI computation. |
| **Sub-district Health Promoting Hospital (SHPH / รพ.สต.)** | Data is scattered across multiple hospital systems. Cross-referencing serum creatinine and eGFR manually is time-consuming and error-prone. | **Auto-Data Integration & Triage:** Automatic fetching of patient height and latest eGFR from central hospital records; multi-tier risk dashboard (Green/Yellow/Red). |
| **Sub-district Health Promoting Hospital (SHPH / รพ.สต.)** | Alerts lack actionable next steps, causing uncertainty whether to escalate or handle locally. | **CPG Integration:** Standard Clinical Practice Guidelines embedded within low-risk screens so primary staff can resolve cases locally. |
| **Hospital Physicians (รพ.ศูนย์)** | Alarm fatigue caused by undifferentiated notifications; critical high-risk MALA cases are not prioritized urgently. | **High-Priority Escalation:** Targeted real-time audible and pop-up notifications restricted strictly to critical (Red Tier) patients. |
| **Hospital Physicians (รพ.ศูนย์)** | Fragmented clinical context when reviewing prescription risks remotely. | **Closed-Loop Action:** Direct access to renal trends and lifestyle triggers with a one-click Metformin dosage adjustment log synced back to primary care. |
