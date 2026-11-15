# User Journeys & Operational Workflows

> The SHPH staff workflow is implemented in the prototype. The physician and MALA case manager steps are **mockups** (display and routing); the hospital implements their real functions. The workflows below follow the Activity Diagram in `.docs/02-design/4 Diagrams/`. Updated November 16, 2026.

> **Note on step order:** the Activity Diagram runs the Part 1 dose check after "Press Save and Assess Risk". On the live prototype (https://mala-project-2026.web.app, checked November 16, 2026) the dose check runs right after the staff member confirms the patient in step 1, before the weight step. Align the diagram or the prototype before the demo.

## Workflow 1: Screening at the Point of Dispensing (SHPH staff)
1. **Login:** SHPH staff select the role "เจ้าหน้าที่ รพ.สต.", enter the ID and password, and the system verifies the credentials and role.
2. **Scan next patient?** If yes, continue. If no, the staff member logs out and the screening session ends.
3. **Identify the patient (step 1 of 5):** Staff scan the QR / barcode HN, or search and select the patient from the list, import a patient list from Excel (HN, name, sex, age, eGFR; processed on the device, not sent to a backend), or add the patient manually (name, HN / ID card number, date of birth, sex, area / village, latest eGFR, Metformin dose; the age is calculated from the date of birth).
4. **Verify patient information:** The patient card shows name, HN, age, previous risk level and latest eGFR. Staff choose "ถูกต้อง · ถัดไป", or "สแกนใหม่" if the patient is wrong.
5. **Record weight and height (steps 2–3):** Staff enter today's weight (the previous weight and date are shown) and the height. The system calculates the BMI.
6. **Answer the risk factor questionnaire (step 4):** Alcohol (Q9, Q9.2.1, Q9.2.2), vomiting or diarrhea (Q10), poor intake (Q11), painkillers / anti-inflammatories / "yaa chud" (Q12), herbal medicine / supplements (Q13).
7. **Review and save (step 5 of 5):** Staff review the entered data and press "บันทึกและประเมินความเสี่ยง" (Save and Assess Risk).
8. **Part 1 — Dose check:** The system checks the Metformin dose against the CPG limit for the latest eGFR (at most 2,000 mg/day at eGFR 45 or higher; at most 1,000 mg/day at eGFR 30–44). Is the dose within criteria?
   - **No → Workflow 2.**
   - **Yes → Part 2.**
9. **Part 2 — MALA risk score:** eGFR < 60 = 1, BMI < 23 = 2, heavy alcohol = 2. Is the score ≥ 2?
   - **Yes → the case is sent to the case manager queue (Workflow 3), and the flow continues below.**
   - **No → the flow continues below.**
10. **Sick Day Rule and advice:** The system checks the Sick Day Rule and generates rule-based patient advice (no trained AI model), then saves the screening history.
11. **Acknowledge the result:** Staff acknowledge the Part 1–2 results and the patient advice, and counsel the patient.
12. **Next patient:** Staff tap "เสร็จสิ้น · สแกนผู้ป่วยรายถัดไป" and return to "Scan next patient?".

---

## Workflow 2: Dose-Fail Escalation (SHPH → Physician Queue → SHPH)
1. **Trigger:** Part 1 fails (the dose is above the CPG limit for the patient's eGFR, for example eGFR 30–44 with more than 1,000 mg/day, or more than 2,000 mg/day).
2. **Screen to SHPH staff:** "ขนาดยาเกินเกณฑ์ · ส่งแพทย์ทันที" with the reason. The MALA assessment (Part 2) is skipped and the case is sent to the physician queue ("บันทึกเข้าคิวแพทย์แล้ว"). The screening history is saved.
3. **Physician / Multidisciplinary Team (mockup):** View the Metformin dose review queue → review and adjust the dose → send the treatment order.
4. **Dose order back to SHPH:** The system sends the dose order back, and SHPH staff receive the dose order from the physician.
5. **Staff action:** Staff follow the doctor's instruction on stopping or adjusting the drug. The system records and forwards the case; it does not order the drug stopped in place of the doctor. Then staff return to "Scan next patient?".

---

## Workflow 3: At-Risk Case Handover (SHPH → MALA Case Manager)
1. **Trigger:** Part 1 passes and the Part 2 score is 2 or more.
2. **Routing:** The case enters the MALA case manager queue (case manager queue, not the physician queue).
3. **Case Manager Screen (mockup):** The case manager logs in with the role "ผู้ดูแลเคส MALA" and sees "คิวติดตามกลุ่มเสี่ยง MALA" (marked "ผู้ดูแลเคส · ไม่ใช่คิวแพทย์") with two tabs: "แดชบอร์ด/รายงาน" and "คิวติดตาม" (with a count of waiting cases).
4. **Follow-up queue:** The case manager views the follow-up queue, can view the patient history and details, accepts the case, follows up with the patient, and marks the case as done (end of case follow-up).
5. **Dashboard and report:** The case manager views the dashboard and report (Part 1–2 statistics, areas with at-risk cases, community guidance counts) and downloads the CSV file, then logs out.
6. **Out of the prototype:** The real care plan, any LINE alert, follow-up visits and closing the case are done by the hospital.

---

## Interactive Prototype & Diagrams
- **Live prototype:** `https://mala-project-2026.web.app`
- **Figma Interactive Prototype:** `https://www.figma.com/design/zoZGhvUfplE9LOq8hVHxZo/MALA?node-id=40-2&t=w6qH4Xm3FKStLGEs-1` (check that it matches the new flow)
- **System Diagrams:** Located in `.docs/02-design/4 Diagrams/`:
  1. `1. System Context Diagram.png` — the MALA Risk Screening & Monitoring System (MRSS) with four external entities: Excel File / QR HN (sends the patient list: HN, name, sex, age, eGFR), SHPH Staff (sends login data, HN, weight, height, risk factors; receives Part 1–2 results, Sick Day Rule, patient advice and the dose order from the physician), MALA Case Manager (receives at-risk cases with score ≥ 2 and screening history, Part 1–2 statistics and the CSV file; sends accept case, mark as done and request report), Physician / Multidisciplinary Team (receives "Metformin dose not within criteria"; sends the dose adjustment order).
  2. `2. Use Case Daigram.jpg` — actors SHPH Staff, Physician / Multidisciplinary Team and MALA Case Manager (mockups for the last two), inside the MRSS boundary: login and logout, scan / search patient HN, import patient list from Excel, add patient manually, record weight and height, answer the risk factor questionnaire, save screening, assess risk (Part 1–2 and Sick Day Rule), send case to the physician queue (dose not within criteria) or the case manager queue (score ≥ 2), view result and patient advice, view the dose review queue, review and adjust dose, send order back to SHPH, view the follow-up queue, view patient history and details, accept case, close case, view dashboard / report and download CSV.
  3. `3. System Architecture Diagram.png` — users on client devices; a web app presentation layer (login and role selection, 5-step screening wizard with camera QR / barcode scanner and Excel parser, MALA case queue and dashboard, physician dose review queue); application logic (authentication and role-based access, BMI calculator, risk engine for Part 1 dose per CPG, Part 2 score and Sick Day Rules, rule-based patient advice generator, case routing, report and CSV export); hosting at mala-project-2026.web.app (HTTPS); and a data layer (users and roles, physician review queue, MALA follow-up queue, screening history, patients with HN, name, age, sex, eGFR and Metformin dose).
  4. `4. Activity Daiagram.jpg` — swimlanes for SHPH Staff, the MALA System, the Physician / Multidisciplinary Team and the MALA Case Manager, following Workflows 1–3 above.
