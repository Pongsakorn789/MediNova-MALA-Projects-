# User Journeys & Operational Workflows

> The PCU Staff (SHPH) workflow is implemented in the prototype. The physician and MALA case manager steps are **mockups** (display and routing); the hospital implements their real functions. Updated November 15, 2026 to match https://mala-project-2026.web.app.

## Workflow 1: Screening at the Point of Dispensing (SHPH staff)
1. **Authentication:** PCU Staff select the role "เจ้าหน้าที่ รพ.สต." and log in on a mobile device or tablet.
2. **Patient Selection (step 1 of 5):** Staff scan the HN, pick the patient from the list (search by name, HN or ID card), import a list from Excel, or add the patient by hand (name, HN / ID card number, date of birth, sex, area / village, latest eGFR, Metformin dose; the system calculates the age from the date of birth).
3. **Confirmation:** The patient card shows name, HN, age, previous risk level and latest eGFR. Staff choose "ถูกต้อง · ถัดไป", or "สแกนใหม่" if the patient is wrong.
4. **Dose Check (Part 1):** The system checks the Metformin dose against the latest eGFR (eGFR ≥ 45 and ≤ 2,000 mg/day = pass).
   - **Fail → Workflow 2.**
   - **Pass → continue.**
5. **Data Entry (steps 2–4):** Staff record today's weight (the previous weight and date are shown), confirm the height (remembered from the last visit; BMI is calculated immediately), and answer the Yes/No questions: alcohol (Q9, Q9.2.1, Q9.2.2), vomiting or diarrhea (Q10), poor intake (Q11), painkillers / anti-inflammatories / "yaa chud" (Q12), herbal medicine / supplements (Q13).
6. **Review and Save (step 5 of 5):** The review screen shows the Part 1 dose result and the Part 2 score with its reason (eGFR < 60 = 1, BMI < 23 = 2, heavy alcohol = 2; at risk when ≥ 2). Staff tap "บันทึกและประเมินความเสี่ยง".
7. **Decision Branching:**
   - **Case A: Not at risk (score < 2):** The result is saved. The system shows guidance, including the Sick Day Rule when it applies; staff advise the patient.
   - **Case B: At risk (score ≥ 2) → Workflow 3.** There is no "send to doctor" button for PCU Staff.
8. **Next Patient:** Staff tap "เสร็จสิ้น · สแกนผู้ป่วยรายถัดไป" to start the next screening.

---

## Workflow 2: Dose-Fail Escalation (SHPH → Doctor's Queue)
1. **Trigger:** The dose check (Part 1) fails right after the patient is confirmed (eGFR < 45 or dose > 2,000 mg/day).
2. **Screen to PCU Staff:** "ขนาดยาเกินเกณฑ์ · ส่งแพทย์ทันที". The MALA assessment is stopped, and the case is added to the doctor's queue ("บันทึกเข้าคิวแพทย์แล้ว").
3. **Staff Action:** Staff follow the doctor's instruction on stopping or adjusting the drug. The system records and forwards the case; it does not order the drug stopped in place of the doctor.
4. **Doctor's Queue (mockup):** The physician / multidisciplinary team see the dose-fail case. The dose order and other real functions are implemented by the hospital.

---

## Workflow 3: At-Risk Case Handover (SHPH → MALA Case Manager)
1. **Trigger:** The dose check passes and the Part 2 score is 2 or more.
2. **Routing:** The case goes to the MALA case manager queue, not to the doctor.
3. **Case Manager Screen (mockup):** The case manager logs in with the role "ผู้ดูแลเคส MALA" and sees "คิวติดตามกลุ่มเสี่ยง MALA" (marked "ผู้ดูแลเคส · ไม่ใช่คิวแพทย์") with two tabs: "แดชบอร์ด/รายงาน" and "คิวติดตาม" (with a count of waiting cases).
4. **Dashboard:** The case manager reviews dose checks (Part 1), MALA assessments (Part 2), the share at risk, the share of listed patients with a recorded result, areas with at-risk cases, and community guidance counts; a CSV can be downloaded.
5. **Follow-up:** Care planning, any LINE alert, follow-up visits and closing the case are done by the hospital and are not part of the prototype.

---

## Interactive Prototype & Diagrams
- **Live prototype:** `https://mala-project-2026.web.app`
- **Figma Interactive Prototype:** `https://www.figma.com/design/zoZGhvUfplE9LOq8hVHxZo/MALA?node-id=40-2&t=w6qH4Xm3FKStLGEs-1` (check that it matches the new flow)
- **System Diagrams:** Located in `.docs/02-design/diagrams/`. The images below were drawn for the earlier design and **need to be redrawn** for the new flow:
  1. `use-case-diagram.png` (Actors: SHPH staff, physician (mockup), MALA case manager / administrator (mockup))
  2. `sequence-diagram.png` (Patient confirmation → dose check → fail: doctor's queue; pass: weight, height, risk factors → Part 2 score → at risk: case manager queue)
  3. `data-flow-diagram.png` (Inputs: eGFR, Metformin dose, BMI, alcohol, symptoms, medicines → Part 1 dose check → Part 2 score → routing to the doctor's queue or the case manager queue)
  4. `er-diagram.png` (Database schema: Users, Patients, Screenings with the Part 1 result and the Part 2 score, queue entries for the doctor and the case manager)
