# Compliance & Legal Requirements Specification (rule.md)
*Traced from Week 2 System Constraints and Legal Requirements*

---

## 1. Role-Based Access Control (RBAC) Specification
Access permissions are strictly partitioned according to clinical responsibilities to prevent data leakage and maintain clinical integrity:

| Role | Permitted Access | Restricted Access |
|---|---|---|
| **Village Health Volunteer (อสม.)** | - Read: Basic profile of assigned zone patients.<br>- Create/Update: Dynamic field vitals (weight, dehydration checklist, alcohol flags). | - Strictly forbidden from viewing hospital diagnostic history, prescription records, and specialized laboratory data (e.g., eGFR, Serum Creatinine). |
| **Primary Care Nurse (รพ.สต.)** | - Read: Zone patient records, integrated lab values (eGFR), active medications, and CPG guidelines.<br>- Create: Clinical screening assessments and escalation triggers. | - Strictly forbidden from modifying or countermanding Metformin prescription regimens directly in the system. |
| **Attending Physician (แพทย์ รพ.ศูนย์)** | - Read: Full medical histories, lab profiles, and screening logs of escalated high-risk cases.<br>- Create/Update: Official prescription modification directives (Metformin dosage adjustment or temporary cessation). | - General routine non-escalated screening logs without clinical referral. |

---

## 2. Personal Data Protection Act (PDPA) & Health Data Privacy
- **Processing Sensitive Health Data (Section 26, PDPA B.E. 2562):** Renal parameters, medication history, and diabetic diagnostic logs constitute sensitive personal health data. System processing is lawful under the legal bases of **Vital Interests** (preventing acute fatal toxicity) and **Public Health Provision** (Section 26(5)(b)).
- **Principle of Data Minimization:** Field volunteers access only the bare minimum data fields required for physical screening. Diagnostic codes and full pharmacy records remain masked.
- **Audit Trails & Non-Repudiation:** The system maintains immutable audit logs capturing:
  - User ID, Role, and Source IP Address
  - Exact Timestamp
  - Operation Performed (`READ_LABS`, `ESCALATE_ALERT`, `UPDATE_PRESCRIPTION`)
  - Audit logs are retained securely for a minimum of 5 years in compliance with healthcare record retention standards.

---

## 3. Data Transmission & Cryptographic Standards
- **Data in Transit:** All inter-system API exchanges between primary care web clients, volunteer mobile apps, and central hospital databases are strictly encrypted via **TLS 1.3**.
- **Data at Rest:** All sensitive personal identifiers (Citizen ID, full name) and health records are encrypted at rest using **AES-256**.
- **Session Management:** Automatic session timeout after 15 minutes of inactivity on shared clinical terminals to mitigate unauthorized physical access.

---

## 4. Software as a Medical Device (SaMD) & Decision Support Governance
- **Clinical Decision Support Status:** The embedded Risk Engine functions strictly as a Clinical Decision Support System (CDSS) for risk stratification and triage; it does not independently formulate medical diagnoses.
- **Human-in-the-Loop Requirement:** Algorithmic calculations cannot directly alter electronic prescriptions. Every pharmaceutical dosage reduction, suspension, or clinical directive must be explicitly confirmed by a licensed medical practitioner possessing valid medical license credentials.
