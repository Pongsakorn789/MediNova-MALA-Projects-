# Core Feature List

### 1. Screening Report & Source Tracking
- Consolidated table of all screenings (see report columns), with filters.
- Each record shows the screening site (PCU), the PCU Staff who entered it, and the time.
  
### 2. Data Integration & AI Services
- Retrieve the latest eGFR (with test date) and patient name from the hospital database by HN.
- OCR to read the HN from the record book or card (with staff confirmation) and to read paper screening forms uploaded after an outage.
- AI-generated individualized advice from the entered risk factors.

### 3. Smart Screening Web Application
- Mobile-friendly five-step flow for PCU Staff: scan HN, weight, height, alcohol use, review and submit.
- Automatic BMI calculation; height remembered after first entry.
- Yes/No checklist: alcohol (heavy drinking in a short period, regular heavy drinking), vomiting or diarrhea, poor oral intake, NSAIDs or "yaa chud", herbal remedies or supplements.
### 4. Risk Engine & Alert
- CPG dose check: eGFR ≥45 → ≤2,000 mg/day, 30–44 → ≤1,000 mg/day, <30 → contraindicated.
- Risk score: eGFR <60 (1), BMI <23 (2), heavy alcohol (2); score ≥2 means at risk.
- Alert card to the multidisciplinary team's LINE group when the dose is inappropriate or the score is ≥2, with "Open case" and "Acknowledged" buttons.

### 5. Clinical Decision Support & Audit Trail
- Individualized advice on screen and as a printable sheet, including the Sick Day Rule.
- Every screening, risk result, and alert is stored with who and when, for retrospective review.

### 6. Offline Fallback
- Paper form with the same items, photographed and uploaded when the connection returns.
