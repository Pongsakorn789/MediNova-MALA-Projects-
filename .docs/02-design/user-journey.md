# User Journeys & Operational Workflows

## Workflow 1: Community Data Capture (VHV → SHPH)
1. **Authentication:** Village Health Volunteer logs in via mobile or tablet.
2. **Patient Selection:** Volunteer selects an assigned diabetic patient within their designated community zone.
3. **Data Logging:** Volunteer records current weight and toggles risk checklist flags (Alcohol intake: Yes/No, Dehydration/Diarrhea: Yes/No).
4. **Submission:** Data is submitted and instantly enqueued in the local Sub-district Health Promoting Hospital (SHPH) review queue.

---

## Workflow 2: Primary Triage & Clinical Escalation (SHPH → Hospital Physician)
1. **Review:** Primary Care Nurse logs in on a workstation and opens the Smart Surveillance Dashboard.
2. **Automated Processing:**
   - The Risk Engine queries the hospital database to fetch the latest eGFR and height.
   - The engine calculates BMI and determines the comprehensive risk tier.
3. **Decision Branching:**
   - **Case A: Low Risk (Green Tier):** The system renders standard CPG recommendations. The nurse advises the patient accordingly, records consultation notes, and closes the case.
   - **Case B: High Risk (Red Tier):** The system flags a critical hazard and activates the "Escalate to Physician" action.
4. **Physician Alert:** An audible alert and high-priority pop-up notification appear on the central hospital doctor's terminal.
5. **Clinical Action:** The physician inspects renal trends, BMI, and acute triggers, logs a Metformin dosage adjustment/discontinuation command, and syncs the updated plan back to the primary care clinic.

---

## Interactive Prototype & Diagrams
- **Figma Interactive Prototype:** `[https://www.figma.com/design/zoZGhvUfplE9LOq8hVHxZo/MALA?node-id=40-2&t=5CtfCJJbjr6rpx3z-1]`
- **System Diagrams:** Located in `.docs/02-design/diagrams/`:
  1. `use-case-diagram.png` (Actors: VHV, SHPH Nurse, Central Hospital Physician, Central HIS)
  2. `sequence-diagram.png` (End-to-end data submission, processing, and escalation lifecycle)
  3. `data-flow-diagram.png` (Input streams: BMI, eGFR, lifestyle factors → Risk Engine → Tier outputs)
  4. `er-diagram.png` (Database schema: Users, Patients, Screenings, AlertLogs, ClinicalActions)
