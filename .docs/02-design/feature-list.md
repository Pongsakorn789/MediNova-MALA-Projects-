# Core Feature List

> The SHPH staff screening flow is implemented in the prototype. The doctor's queue and the case manager screens (features 1 and 7) are **mockups**; the hospital implements their real functions. Updated November 16, 2026 to match https://mala-project-2026.web.app.

### 1. Case Manager Dashboard, Report & Source Tracking (mockup)
- Dashboard for the MALA case manager: dose checks (Part 1: total, passed, sent to the doctor), MALA assessments (Part 2: total, at risk, share at risk), share of listed patients with a recorded result, areas with at-risk cases, and community guidance counts (painkillers / anti-inflammatories, herbal / supplements).
- Report of all screenings (see report columns) with a CSV download.
- Each record shows the screening site (SHPH), the SHPH staff who entered it, and the time.

### 2. Data Integration & Rule-Based Advice
- The latest eGFR and the current Metformin dose come with the patient record, so staff do not type them. Patients can be chosen from a list (search by name, HN, ID card number or Patient ID), imported from an Excel file, or added by hand.
- **Excel import:** the system reads HN, name, sex, age and eGFR (and the area when the file has an area column; otherwise the backup area typed on the screen is used). Date of birth, weight, height and the Metformin dose are not in the file and are entered before assessment. The file is processed in the browser and is not sent to a backend.
- Camera scan of the QR code or barcode on the patient card or record book to find the HN (staff confirm the patient). Reading the HN from a photo with AI and reading paper screening forms after an outage are planned and not seen in the prototype.
- Individualized advice from the entered risk factors. The advice is **rule-based** (fixed rules, no trained AI model) and never changes a dose.

### 3. Smart Screening Web Application
- Mobile-friendly five-step flow for SHPH staff: 1 identify the patient, 2 weight, 3 height, 4 risk factors, 5 review and save.
- Step 1 offers camera scan, the patient list with search, Excel import, and manual entry (name, HN / ID card number, date of birth with the age calculated by the system, sex, area / village, latest eGFR, Metformin dose). The staff member confirms the patient ("ถูกต้อง · ถัดไป") or chooses "สแกนใหม่".
- Automatic BMI calculation; the previous weight and its date are shown.
- Yes/No checklist: alcohol (drinks or not; heavy drinking in a short period; regular heavy drinking, with thresholds by sex), vomiting or diarrhea, poor oral intake, painkillers / anti-inflammatories / "yaa chud", herbal remedies or supplements.
- Review screen ("ตรวจทานก่อนบันทึก") with the Part 1 dose result and the Part 2 score with its reason before saving.

### 4. Risk Engine & Routing
- **Part 1, dose check first (CPG table):** the dose passes when it is at most 2,000 mg/day at eGFR 45 or higher, or at most 1,000 mg/day at eGFR 30–44; otherwise it fails. On fail the MALA assessment is stopped and the case goes to the doctor's queue immediately. The system records and forwards the case; it does not order the drug stopped in place of the doctor.
- **Part 2, risk score:** eGFR < 60 (1), BMI < 23 (2), heavy alcohol (2); score ≥ 2 means at risk. The symptom and medicine answers are shown, not scored.
- **Routing:** dose-fail → doctor's queue; at risk → MALA case manager queue; not at risk → saved with guidance.
- An optional LINE alert is handled on the case manager side by the hospital and may not be connected; SHPH staff have no LINE button.

### 5. Clinical Decision Support & Screening History
- Rule-based advice on screen, including the Sick Day Rule; a printable sheet is planned.
- Every screening is saved with its Part 1 result, Part 2 score and routing for retrospective review. A full who-and-when audit trail is planned and not yet seen in the prototype.

### 6. Offline Fallback (not yet seen in the prototype)
- Paper form with the same items, photographed and uploaded when the connection returns.
- Offline save with automatic sync when the connection returns.

### 7. Doctor's Queue & Case Manager Queue (mockups)
- Doctor's queue ("คิวแพทย์"): receives every dose-fail case. The physician reviews and adjusts the dose and sends the order back to SHPH; the real dose order and other doctor functions are implemented by the hospital.
- Case manager queue ("คิวติดตาม", marked "ผู้ดูแลเคส · ไม่ใช่คิวแพทย์"): receives at-risk cases; the tab shows a count of waiting cases. The case manager can view the queue and patient history, accept a case and mark it as done. The case manager is also the system administrator (one role). The real follow-up (care plan, LINE alert, closing the case) is done by the hospital.
