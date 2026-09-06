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

## 3. User Validation Gate: Interview Summary (Target: ≥ 5 Real Users)
To validate the problem space and ensure traceability, interviews were conducted with five frontline healthcare stakeholders:

| Stakeholder Role | Organization | Key Pain Points & Validated Needs |
|---|---|---|
| **User 1: Village Health Volunteer (อสม.)** | Community Level | Complex forms lead to data entry errors in the field. Needs an ultra-simplified mobile interface with large buttons that only requires entering current weight and answering simple yes/no behavioral questions. |
| **User 2: Village Health Volunteer (อสม.)** | Community Level | Cannot conduct in-depth medical evaluations. Prefers standard checklists (e.g., recent diarrhea/vomiting, alcohol intake) rather than open text fields to ensure fast on-site screening. |
| **User 3: Registered Nurse** | SHPH / Primary Care | Switching between multiple hospital tabs to retrieve serum creatinine and eGFR is time-consuming. Strongly requests automated fetching of the latest eGFR and height directly from the central hospital database. |
| **User 4: Public Health Officer** | SHPH / Primary Care | Alerts without clinical guidelines create uncertainty. Requests that low-to-moderate risk cases provide embedded Clinical Practice Guidelines (CPG) so primary nurses can counsel patients immediately without unnecessary hospital escalations. |
| **User 5: Internal Medicine Specialist** | Tertiary Center | Suffers from alarm fatigue caused by non-urgent alerts. Requests emergency push notifications and audible pop-ups reserved strictly for high-risk/critical red cases, coupled with one-click dosage adjustment logging. |
