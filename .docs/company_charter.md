# Company Charter for Software Engineering Case Studies

**Project Title:** MALA Risk Screening System (MRSS) — a web application
**Company:** MediNova
**Course:** 1305493 Software Engineering Case Studies

## Team Members

| Name | Student ID | Role |
|---|---|---|
| Thanakit Chupviroj | 6631503020 | AI Lead |
| Pongsakorn Chaikittiporn | 6631503114 | Tech Lead |
| Phorn Potjanasuj | 6631503029 | QA / Test |
| Maneenat Wantanasri | 6631503035 | Designer |

---

## 1. Purpose

This document defines the scope, goals, and direction of the MALA Risk Screening web application project, ensuring the team, course instructor, and network partners, namely Primary Care Units (PCUs, รพ.สต.) and Chiangrai Prachanukroh Hospital, share a common understanding before design and development begin.

## 2. Problem Statement

Diabetic patients on Metformin are at risk of Metformin-Associated Lactic Acidosis (MALA), particularly those with low eGFR, older age, or comorbidities. Hospitals send Metformin to PCUs so that stable diabetic patients can collect it close to home, but PCU Staff dispense it without seeing the patient's eGFR and without collecting other risk data (weight, height, alcohol use), so patients receive Metformin with no risk screening. Data from fiscal years 2023–2025 in Mueang Chiang Rai district shows that 58% of MALA cases originated from primary care units.

### Pain Points

| Item | Detail |
|---|---|
| Pain 1 | No dedicated tool exists to measure MALA risk, only general purpose record systems not designed for this specific screening. |
| Pain 2 | PCU Staff cannot see the patient's latest eGFR at the point of dispensing, and no weight, height, or alcohol-use data is collected, so Metformin is dispensed without any risk information. |
| Pain 3 | Because the data is missing, no risk screening takes place at dispensing, high-risk patients are identified too late, and treatment is often delayed. |

## 3. Objectives

1. Build a MALA risk-screening web application that connects PCU Staff and the hospital multidisciplinary team (physicians, pharmacists, nurses) into one workflow, so that every Metformin dispensing at a PCU is screened and recorded.
2. Reduce time and workload for frontline screening by PCU Staff, with a form that takes no more than 1–2 minutes per patient.
3. Enable faster high-risk case alerts to the multidisciplinary team compared to the current referral process.
4. Design the system to be compliant with data protection and cybersecurity law from day one.

## 4. Scope

**In scope (DISCOVER phase, W1–W5)**

- Requirements gathering through interviews with 3 real user groups
- Product backlog with MoSCoW prioritization
- Design draft: feature list, user journey, prototype (Figma), 4 diagrams
- Legal & Compliance documentation (`rule.md`)

**Out of scope (deferred to BUILD phase)**

- Actual production code
- Integration with real hospital database systems (e.g., HosXP)
- Testing with real users in live working environments

## 5. Target Users / Stakeholders

| Group | Role | Pain point | What they need |
|---|---|---|---|
| Diabetic patients on Metformin | Patients in the PCU catchment area who receive Metformin continuously | Receive Metformin with no risk screening | Screening at every dispensing and individualized advice (Sick Day Rule, alcohol, NSAIDs) |
| PCU Staff: Primary Care Unit Staff (เจ้าหน้าที่ รพ.สต.) | Primary users: dispense the drug and perform the screening | Cannot see eGFR; no screening data collected at dispensing | Auto eGFR retrieval + risk calculation + CPG-based advice |
| Hospital multidisciplinary team (physicians, pharmacists, nurses) | Monitor and make clinical decisions on high-risk alerts | Learns of high-risk cases late | Instant alerts with decision-support data |
| Course Instructor | Project evaluator | Needs evidence from real users | Evidence log, backlog traced to real pain points |

## 6. Proposed Solution

The MALA Risk Screening web application connects one continuous workflow: PCU Staff scan the HN and enter screening data at dispensing → the system auto-retrieves the latest eGFR and assesses risk → normal risk: advice shown and result saved / high risk: alert sent to the multidisciplinary team.

### One Core Workflow

PCU Staff scan the HN and confirm → system pulls the latest eGFR → staff enter weight, height, alcohol use, and the symptom and medication questions → submit → system checks the dose against the CPG and calculates the risk score → normal: result saved and individualized advice shown / over threshold or inappropriate dose: alert card sent to the team's LINE group → team opens the case and acts.

## 7. Key Features

| # | Feature | Detail |
|---|---|---|
| 1 | Smart Screening Form | Scan HN, enter weight and height (BMI calculated automatically), alcohol use, and Yes/No symptom and medication questions |
| 2 | Screening Report | Table of all screenings with risk result and screening site, for review |
| 3 | Auto-Data Integration | Auto-retrieves the latest eGFR from the hospital DB by HN |
| 4 | Risk Engine & Smart Alert | Checks Metformin dose against eGFR (CPG), calculates the risk score (eGFR, BMI, alcohol), and alerts the team's LINE group when over threshold |
| 5 | Decision Support (CPG) | Individualized advice and Sick Day Rule guidance from the entered risk factors |
| 6 | Offline Fallback | Paper form with the same items, photographed and read by AI when the internet returns |
| 7 | Audit Trail | Every screening, risk result, and alert is recorded with who and when, for later review |

## 8. Success Criteria / KPIs

### Metric (before / after)

- **Before:** High-risk MALA patients were identified late, as PCUs dispensed Metformin without eGFR or screening data.
- **After:** The system retrieves eGFR automatically, calculates risk at dispensing, and alerts the team right away for high-risk cases.
- **Metric:** Number of MALA patients identified and treated in time (before vs after).

| Area | Target | Metric |
|---|---|---|
| Process | ≥ 80% | Metformin patients screened via the system |
| Outcome | < 30% | Proportion of MALA cases from primary care reduced |
| Quality | ≥ 85% | Staff satisfaction with the system |

## 9. Timeline / Milestones

| Week | Activity |
|---|---|
| W1 | Company Charter set, 5 roles defined, initial pain points |
| W2 | AI Ethics/PDPA/IT Law covered → `rule.md` written |
| W3 | Requirements → backlog via AI agents & skills |
| W4 | Build prototype (Figma) + complete 15 user interviews |
| W5 (Sep 8) | User Validation Gate — submit Proposal, Backlog, Design draft, Compliance |
| Post-W5 | BUILD phase begins (if gate passed) |

## 10. Risks & Constraints

- The eGFR thresholds and risk scoring criteria have not yet been reviewed by clinical experts and may require revision.
- Feasibility of integrating with the existing hospital database (e.g., HosXP) is not yet confirmed.
- Limited time to complete 15 real user interviews within the deadline.
- Limited team size may require members to cover more than one role.

## 11. Assumptions

- Assumes PCUs have sufficient devices (computer/mobile) and internet access to use the web app, with a paper form and later photo upload as the fallback when the internet is down.
- Assumes the network partner (Chiangrai Prachanukroh Hospital) is willing to provide data and cooperate in prototype testing.
- Assumes the screening criteria used during the DISCOVER phase is a proof-of-concept demonstration, not an immediately deployable diagnostic tool.

## 12. Legal & Compliance

As the system handles health data classified as sensitive data under PDPA, it is designed to be legally compliant from the start (full detail in `rule.md`).

- **PDPA** — Consent obtained before data collection; only necessary data stored; patients can access/correct/delete their own data; role-based access limits enforced.
- **Computer Crime Act §26** — Access logs for every role retained for at least 90 days.
- **Electronic Transactions Act §9** — Alert acknowledgements and case handling recorded as legally valid electronic evidence.
