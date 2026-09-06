# Core Feature List

### 1. Smart Surveillance Dashboard
- Consolidated monitoring table for diabetic patients under Metformin treatment.
- Dynamic color coding based on risk tiers (Green = Low Risk, Yellow = Moderate Risk, Red = High Risk / Critical).
- Source attribution tracking displaying data origin (which volunteer or clinic submitted the screening).

### 2. Automated Data Integration Engine
- Automated integration with Central Hospital Information Systems (HIS) via secure RESTful APIs.
- Automatic retrieval of static/slow-changing metrics: Patient Height and the most recent renal function lab results (eGFR / Serum Creatinine).
- Active medication profile ingestion (Metformin daily dosage and concurrent nephrotoxic agents).

### 3. Smart Screening Mobile Form
- Lightweight, responsive data-entry interface optimized for mobile/tablet use by field volunteers.
- Automated Body Mass Index (BMI) computation upon weight input.
- Binary risk checklist covering temporary dehydration factors (acute diarrhea, severe vomiting) and alcohol consumption frequency.

### 4. Risk Engine & Smart Escalation
- Rule-based algorithmic evaluation combining renal function (eGFR), metabolic index (BMI), current dosage, and acute dehydrating factors.
- Multi-channel critical alert system trigger: Real-time visual pop-ups and audible alerts dispatched directly to the attending physician's clinical terminal upon detecting Red Tier risk.

### 5. Clinical Decision Support System (CDSS) & CPG Integration
- Embedded Clinical Practice Guidelines (CPG) rendered on the primary care evaluation screen for low-to-moderate risk profiles.
- Standardized guidance facilitating non-physician medical staff to provide preventative advice without unnecessary hospital transfers.
