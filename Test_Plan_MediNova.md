# Test Plan — MRSS (MALA Risk Screening & Monitoring System)
Sub-district Health-Promoting Hospital (SHPH / รพ.สต.) Risk Screening Web Application

**Version:** 1.1 (revises v1.0 of October 28, 2026)
**Date Revised:** November 15, 2026
**UAT Date:** Tuesday, November 17, 2026 (the instructor tests the system; replaces November 4 in v1.0)
**System under test (prototype):** mala-project-2026.web.app

**AI Disclosure:** AI was used to help draft the structure, tables, and initial test-case wording of this document, based on the team's backlog, proposal, and workflow documentation. The v1.1 revisions were drafted with AI from the team's notes on the design changes agreed with the mentor. All content must be reviewed by the team.

| Role | Name | Student ID |
|---|---|---|
| Product Owner | Pongsakorn Chaikittiporn | 6631503114 |
| UX/UI Designer | Maneenat Wantanasri | 6631503035 |
| AI Lead | Thanakit Chupviroj | 6631503020 |
| QA / Test Lead | Phorn Potjanasuj | 6631503029 |

*Team of 4 — no separate Tech Lead; development ownership is shared across the team, led by the Product Owner.*

> ✅ **Status of v1.1:** the screen details below were checked against the live prototype (mala-project-2026.web.app) on November 15, 2026 for the SHPH screening flow (login, patient selection, manual-entry form, dose-fail result, weight, height, risk factors, review screen) and for the case manager dashboard screen. Items marked "to verify" were not reachable in that check (physician screens, the case manager queue tab contents, the result screen after saving, offline mode). Each test case must still be marked Pass/Fail by actually running it.

---

## Changes from v1.0

| Area | v1.0 | v1.1 |
|---|---|---|
| Login roles | 2 (SHPH staff, Physician/team) | **3**: SHPH staff, Physician/team, **MALA case manager (ผู้ดูแลเคส MALA)** |
| Order of checks | 5-step wizard, then a numeric Risk Score (cutoff 60) | **Dose check (Part 1) comes first.** MALA assessment (Part 2) runs only if the dose check passes. |
| Dose check fails | Not a separate path | Show "stop the drug", **skip the MALA assessment, close the case, alert the doctor immediately** |
| MALA at risk | SHPH staff manually press "ส่งเคสเข้าคิวแพทย์" and a separate LINE button | Case goes to the **MALA case manager queue**, **not to the doctor**. The SHPH "send to doctor" button and doctor report are removed from this path. |
| LINE alert | SHPH staff button | Moved to the case manager; optional and may be unconnected (handled by the hospital, out of this plan's scope) |
| Risk output | Score with threshold 60 | Part 2 additive score; **at risk when score ≥ 2**. The symptom and medicine answers (Q10–Q13) are shown but not scored. |
| Physician dashboard | All escalated cases | Only **dose-fail** (urgent) cases |
| Step 1 patient entry | Camera scan, search, mock mode | Camera scan, a list of 30 test patients with search by name or HN, **Excel import**, and **manual entry** (form fields: name, HN / ID card number, date of birth with the age calculated by the system, sex, area / village, latest eGFR, Metformin dose per day). No separate mock mode button was found. |
| Dashboard metrics | 3 physician stat cards | Case manager dashboard: dose checks (Part 1: passed / sent to the doctor), MALA assessments (Part 2) and how many scored as at risk, share of Part 2 at risk, share of listed patients with a recorded result, areas with at-risk cases, and NSAID / herbal counts. Includes a CSV download. |
| Physician and case-manager screens | Full functions tested | **Mockups only.** The hospital takes them over and implements the real functions. Only display and routing are checked. |
| UAT scope this round | SHPH and Physician | **SHPH staff only** |
| Schedule | Oct 27 – Nov 18, UAT on Nov 4 | UAT on **Nov 17, 2026**; preparation Nov 15–16 |

---

## 1. Scope

Verify that the SHPH staff role can complete the MALA screening workflow correctly under the new routing, that the Risk Engine checks the dose first and then computes MALA risk as specified, and that cases reach the correct destination (doctor for dose-fail, MALA case manager for at-risk). The physician and case-manager screens are **mockups** that the hospital will take over and implement, so they are only checked for what they display and where cases arrive. The **user acceptance test (UAT) in this round covers the SHPH role only**. Compare task outcomes before and after using the evaluation design in Section 5.

> **Current design:** the system has **three** login roles — "เจ้าหน้าที่ รพ.สต." (SHPH staff), "แพทย์ / ทีมสหวิชาชีพ" (Physician/team), and "ผู้ดูแลเคส MALA" (MALA case manager).

**In scope — SHPH screening flow**
- Login screen with role selector plus username (e.g. `SHPH001`) and password fields
- Step 1 — identify the patient: camera scan of HN, choose from the list of 30 test patients or search by name / HN / ID card, import a patient list from Excel, or **manual entry** (name, HN / ID card number, date of birth — the system calculates the age — sex, area / village, latest eGFR, Metformin dose per day). The chosen patient is confirmed with "ถูกต้อง · ถัดไป" or changed with "สแกนใหม่".
- **Dose check (Part 1)** right after the patient is confirmed, using the eGFR and Metformin dose already recorded for the patient: eGFR ≥ 45 **and** Metformin ≤ 2000 mg/day = pass
  - **Fail:** "stop the drug" instruction, MALA assessment skipped, case closed, doctor alerted immediately
  - **Pass:** continue to the MALA assessment
- Data entry for the MALA assessment (5 steps in total: 1 patient, 2 weight, 3 height, 4 risk factors, 5 review): today's weight (shows the previous weight and date), height (remembered from the last visit; BMI calculated immediately), alcohol questions (Q9 drinks or not; Q9.2.1 heavy drinking within a short time; Q9.2.2 regular or continued heavy drinking — thresholds shown for the patient's sex), and the symptom and medicine questions (Q10 vomiting or diarrhea, Q11 unable to eat or eating less for 1–2 days, Q12 painkillers / anti-inflammatories / ยาชุด, Q13 herbal medicine / supplements)
- Review screen before saving: patient, eGFR, weight, height, BMI, the Part 1 dose result, the **Part 2 score with its reason**, and all answers
- **MALA assessment (Part 2) result:** additive score — eGFR < 60 = 1, BMI < 23 = 2, heavy alcohol = 2; **score ≥ 2 = at risk**. The number of "yes" answers is not the score (the screen says so). The symptom and medicine answers (Q10–Q13) are shown as answers and are **not scored**
- Recommendations block, including the "กฎวันป่วย (Sick Day Rule)" warning card
- Routing after the result: at risk → the case **enters the case manager queue automatically as soon as the screening is saved** (no button press by SHPH staff), with no SHPH "send to doctor" button and no doctor report; not at risk → record and guidance
- "เสร็จสิ้น · สแกนผู้ป่วยรายถัดไป" to end the flow and start the next patient

**In scope — Physician / team role (mockup only)**
- Login and a dashboard ("ภาพรวมการเฝ้าระวัง MALA") that shows **only dose-fail cases**
- A dose-fail case appears with an urgent alert
- Everything else on the physician screens (dose order form, digital signature, alarm settings, full patient list with search and filters) is a mockup the hospital will replace; these are **not** tested in this plan

**In scope — MALA case manager role (mockup only)**
- Login with role "ผู้ดูแลเคส MALA" opens a screen titled "คิวติดตามกลุ่มเสี่ยง MALA" marked "ผู้ดูแลเคส · ไม่ใช่คิวแพทย์", with two tabs: "แดชบอร์ด/รายงาน" and "คิวติดตาม" (a number badge shows how many cases wait). The queue lists at-risk cases (dose passed, Part 2 score ≥ 2) — contents of the queue tab to verify
- Open a case: patient information, itemized Part 2 score, and the factors found (e.g. NSAID, herbal, ยาชุด)
- Follow-up after receiving the case (care plan, LINE alert, follow-up, closing the case) is done by the hospital and is **not** part of the prototype

**In scope — dashboard metrics for the MALA case manager / administrator (mockup)**
- Dose checks (Part 1): how many were done, how many passed and how many were sent to the doctor for review
- MALA assessments (Part 2): how many were done, how many scored as at risk, and the share at risk (example on screen: 10 assessments, 9 at risk, 90%)
- "ผู้ป่วยที่มีผลบันทึก": share of the listed patients that have a recorded screening result (example on screen: 12 of 30, 40%). This is what the screen shows in place of a "% of patients the system helped" figure, whose exact definition the mentor left open.
- Areas with at-risk cases (most cases first, then by village — counted from Part 2 assessments that scored 2 or more) and community guidance counts: painkillers / anti-inflammatories and herbal / supplements. The screen says the main indicators count only Part 1 and Part 2 and do not include nausea / vomiting. A "ดาวน์โหลด CSV" button exports the data.

**In scope — not yet confirmed on screen; verify before final test-case pass**
- Offline save + auto-sync behaviour for field data capture (not seen in the check)
- AI-assisted camera OCR scanning of the HN (the camera step needs the browser's camera permission; list selection bypasses it)

**Out of scope for this cycle:** real functions of the physician and case-manager roles (mockups; the hospital implements them), including the dose order, MOPH digital signature, audible alarm, patient-list search and filters, role-based access, and the audit log; large-scale load testing, exhaustive penetration testing, and internal testing of third-party services (the hospital's HosXP database, Firebase/hosting infrastructure, LINE's own delivery reliability). The real LINE connection is optional and may not be connected.

---

## 2. Levels — Which Test Levels

The team will perform four levels of testing following the Testing Pyramid, per course guidance (14 Oct lecture).

| Test Level | Testing Purpose | MRSS Example |
|---|---|---|
| Unit Testing | Verify individual functions | Validate the dose check (eGFR ≥ 45 and ≤ 2000 mg/day) and the additive Part 2 score (eGFR < 60 = 1, BMI < 23 = 2, heavy alcohol = 2; at risk when ≥ 2) against known inputs |
| Integration Testing | Verify communication between components | Confirm a screening submitted by SHPH stores the dose-check result and Part 2 score, and routes the case to the correct destination (doctor or case manager queue) |
| End-to-End (E2E) Testing | Verify the complete workflow | SHPH screens a patient → dose check → (fail: case closed and doctor alerted) or (pass: Part 2 → at-risk case appears in the case manager queue) |
| User Acceptance Testing (UAT) | Evaluate system usability with real users | SHPH-role users perform realistic screening tasks |

---

## 3. Test Cases — Test Cases and UAT Scenarios

### 3.1 Functional Test Cases

| ID | Test Case | Test Steps | Test Data | Expected Result | Pass/Fail |
|---|---|---|---|---|---|
| TC01 | Valid login — SHPH | Select role "เจ้าหน้าที่ รพ.สต.", enter valid username/password, submit | `SHPH001` + valid password | Login succeeds; Step 1 "สแกน HN" screen loads | Pending |
| TC02 | Physician/team mockup — login | Select role "แพทย์ / ทีมสหวิชาชีพ", enter valid credentials (e.g. `DOC001`), submit | Valid physician test account | Login succeeds; "ภาพรวมการเฝ้าระวัง MALA" dashboard loads with "รอสั่งการรักษา" tab active | Pending |
| TC03 | MALA case manager mockup — login | Select role "ผู้ดูแลเคส MALA", enter valid credentials, submit | Valid case-manager test account | Login succeeds; the case manager screen "คิวติดตามกลุ่มเสี่ยง MALA" loads with the tabs "แดชบอร์ด/รายงาน" and "คิวติดตาม" | Pending |
| TC04 | Invalid login | Enter incorrect credentials and submit | Invalid password | Access denied, no session created, error shown | Pending |
| TC05 | Select a patient from the list | From Step 1, choose a patient from the list of 30 test patients (or use the search box) | Patient with eGFR 28 (HN 48-998812) and patient with eGFR 68 (HN 51-345678) | The confirmation card shows name, HN, age, previous risk level and latest eGFR; "สแกนใหม่" and "ถูกต้อง · ถัดไป" are available | Pending |
| TC06 | Manual entry — age from date of birth | On Step 1 choose "ไม่พบรายชื่อ? เพิ่มผู้ป่วยด้วยการกรอกมือ" and fill the form | Name, HN / ID card number, date of birth (e.g. 1 Jan 1961, test date 15 Nov 2026), sex, area, eGFR, Metformin dose | The age is calculated from the date of birth (65) and shown; the save button stays disabled until the required fields are filled; an invalid or future date of birth is rejected | Pending |
| TC07 | Search by name or HN | Tap "ค้นหาด้วยชื่อ HN หรือเลขบัตรแทน" and type in the search box | A name or HN from the list | The list narrows to the matching patient, who can be selected | Pending |
| TC08 | Confirm patient | On the confirmation card, tap "ถูกต้อง · ถัดไป" | Patient matched in TC05–TC07 | Proceeds to the dose check with the confirmed patient carried forward | Pending |
| TC09 | Re-scan if patient is wrong | On the confirmation card, tap "สแกนใหม่" | A mismatched patient shown | Returns to Step 1 to retry, without carrying over the wrong patient | Pending |
| TC10 | Auto-pull of eGFR and current dose | Observe the confirmation card and the review screen | Patient with eGFR and Metformin dose on file | Latest eGFR (e.g. "45 mL/min") and current dose auto-populate from the patient's recorded data with no manual entry | Pending |
| TC11 | Dose check — pass | Confirm a patient with eGFR ≥ 45 and Metformin ≤ 2000 mg/day | eGFR 50, 1000 mg/day | Dose check passes; flow continues to the MALA assessment | Pending |
| TC12 | Dose check — fail by dose | Confirm a patient with Metformin > 2000 mg/day | eGFR 60, 2500 mg/day | Dose check fails; "stop the drug" instruction shown | Pending |
| TC13 | Dose check — fail by eGFR | Confirm a patient with eGFR < 45 (including < 30) | eGFR 44 and eGFR 25 | Dose check fails in both cases; "stop the drug" instruction shown; eGFR < 30 is flagged as the most severe | Pending |
| TC14 | Dose-fail closes the case | After a failed dose check, try to continue | Failed case from TC12/TC13 | MALA assessment steps are not offered; the case is closed; no entry appears in the case manager queue | Pending |
| TC15 | Dose-fail alerts the doctor immediately | Log in as physician after TC12/TC13 | Failed case from TC12/TC13 | Case appears on the physician dashboard and the notification bell shows an unread alert | Pending |
| TC16 | Enter today's weight | Enter weight via numeric keypad, tap "ถัดไป" | Weight value, e.g. 59 (HN 51-345678) | Value accepted; the previous weight and its date are shown (e.g. "ครั้งก่อน 70 กก. (12 มิ.ย. 2569)") | Pending |
| TC17 | Enter height & auto-BMI | Enter a height via the numeric keypad | Height remembered from the last visit (168 cm) or a new value | BMI computed and displayed automatically (e.g. weight 59 kg, height 168 cm → "BMI 20.9 kg/m²") | Pending |
| TC18 | Alcohol questions (Q9, Q9.2.1, Q9.2.2) | Choose "ไม่ดื่ม" or "ดื่ม"; when "ดื่ม" is chosen answer the two heavy-drinking questions | Drinks = yes, regular heavy drinking = yes | The two follow-up questions appear with the thresholds for the patient's sex; the answers are carried to the review screen and the "heavy alcohol" 2 points are added to the Part 2 score | Pending |
| TC19 | Vomiting / diarrhea and poor intake (Q10, Q11) | Answer "ใช่" or "ไม่ใช่" to Q10 and Q11 | Q10 = yes | The answers are saved and shown on the review screen; they do not change the Part 2 score | Pending |
| TC20 | Painkillers and herbal use (Q12, Q13) | Answer "ใช่" or "ไม่ใช่" to Q12 (painkillers / anti-inflammatories / ยาชุด) and Q13 (herbal medicine / supplements) | Q12 = yes | The answers are shown on the review screen; they do not change the Part 2 score and are counted in the community guidance on the case manager dashboard | Pending |
| TC21 | Review screen accuracy | Complete steps 1–4 and reach step 5 "ตรวจทานข้อมูล" | HN 51-345678, weight 59 kg, height 168 cm | Patient, eGFR (68), weight, height, BMI (20.9), the Part 1 result "ขนาดยาอยู่ในเกณฑ์", the Part 2 score with its reason, and all answers are shown correctly | Pending |
| TC22 | Part 2 score on review and after saving | Check the Part 2 box on the review screen, then tap "บันทึกและประเมินความเสี่ยง" | eGFR 68 (0), BMI 20.9 (2), regular heavy drinking (2) | Review screen shows "4 คะแนน · เข้าเกณฑ์เสี่ยง" with the rule "eGFR < 60 = 1 · BMI < 23 = 2 · ดื่มหนัก = 2"; the result screen after saving is to verify | Pending |
| TC23 | Part 2 boundary values | Submit profiles that score 1 and 2 | Score 1: eGFR < 60 only. Score 2: BMI < 23 only | Score 1 = not at risk; score 2 = at risk | Pending |
| TC24 | At risk → case manager queue, not doctor | Submit an at-risk screening without pressing any send button, then check both roles | At-risk case from TC22 | Case appears in the case manager queue automatically right after saving; it does **not** appear on the physician dashboard; the SHPH screen has no "send to doctor" button and no doctor report | Pending |
| TC25 | Not at risk — record and guidance | Submit a low-risk screening | Part 2 score 0 or 1 | Result shows not at risk with guidance; the case is saved and not queued for the case manager | Pending |
| TC26 | LINE alert not on the SHPH screen | Review the SHPH result screens | At-risk and dose-fail cases | No LINE alert button is shown to SHPH staff | Pending |
| TC27 | Sick Day Rule warning | Submit a screening with dehydration/diarrhea = present | Symptom present | The result screen shows the "กฎวันป่วย (Sick Day Rule)" warning card | Pending |
| TC28 | Finish & start next patient | Tap "เสร็จสิ้น · สแกนผู้ป่วยรายถัดไป" | Completed screening | Returns to Step 1 ready for the next patient; no data from the previous patient remains | Pending |
| TC29 | Physician sees dose-fail cases only | Log in as physician with one dose-fail and one at-risk case in the system | Cases from TC12 and TC22 | "รอสั่งการรักษา" lists the dose-fail case and not the at-risk case | Pending |
| TC30 | Case manager queue shows at-risk cases | Log in as case manager after TC22 | At-risk case from TC22 | Case appears in the "คิวติดตาม" tab (the tab's count badge increases; status wording to verify) | Pending |
| TC31 | Case manager opens a case | Open a case from the queue | Case from TC30 | Patient info, itemized Part 2 score (eGFR, BMI, alcohol), and found factors are shown; nothing is sent to the doctor | Pending |
| TC32 | Case manager does not see dose-fail cases | Log in as case manager with a dose-fail case in the system | Case from TC12 | The dose-fail case is not in the queue | Pending |
| TC33 | Case manager dashboard (mockup) | Log in as case manager and open "แดชบอร์ด/รายงาน" | Demo data on screen | The cards show dose checks (Part 1), MALA assessments (Part 2), the share at risk and the share of listed patients with a recorded result; the numbers agree with each other (for example, passed + sent to doctor = total dose checks); the area card and community guidance card are shown; "ดาวน์โหลด CSV" is available | Pending |
| TC34 | No volunteer wording | Read the review screen and the dashboard text | Any screening | No screen mentions volunteers (อสม.); the review screen currently shows "บันทึกโดย อสม. ประจำหมู่ 1" and the guidance card on the dashboard mentions อสม. — known issue to fix before UAT | Pending |

**Note:** Pending means the test has not yet been executed. The team will record actual Pass/Fail results during testing. Points to verify against the prototype before marking results: the dose rule for eGFR 30–44 (the dose-fail screen says eGFR below 30 should stop the drug; TC13 assumes any eGFR below 45 fails), the result screen after saving, the case manager queue tab, and the physician dashboard. The patient list still shows the old three-level labels ("ความเสี่ยงต่ำ / เสี่ยงปานกลาง / เสี่ยงสูง"), which are not part of the new Part 1 / Part 2 flow.

### 3.2 UAT Scenarios (SHPH staff only — this round)

| UAT ID | User Role | Scenario | Success Criteria |
|---|---|---|---|
| UAT01 | SHPH staff | "A patient has come in for their Metformin refill. Pull up their record and screen them from start to finish." | Participant completes the screening unaided, passes the dose check, and reaches a MALA result |
| UAT02 | SHPH staff | "The system says this patient's Metformin dose is not safe. Do whatever you think is needed." | Participant reads the "stop the drug" instruction, understands the case is closed and the doctor is alerted, and does not look for further assessment steps |
| UAT03 | SHPH staff | "This patient's dose is fine, but the system says they are at risk. Tell me what happens to the case next." | Participant reaches the at-risk result and correctly states that the case goes to the MALA case manager, not to the doctor |
| UAT04 | SHPH staff | "You made a mistake and scanned the wrong patient. Fix it." | Participant finds "สแกนใหม่" and corrects the patient before continuing |
| UAT05 | SHPH staff | "A patient is low-risk this time. Finish the screening and tell me what the system recommends." | Participant reaches a result screen and correctly reports the guidance shown |
| UAT06 | SHPH staff | "A patient has no QR code. Enter their details by hand using their HN and date of birth." | Participant finds the manual entry option, types the HN and date of birth, and the correct patient and age are shown |
| UAT07 | SHPH staff | "Start a new screening for another patient without restarting the app." | Participant uses "เสร็จสิ้น · สแกนผู้ป่วยรายถัดไป" to move to the next patient smoothly |

Users will receive scenarios rather than step-by-step button instructions, per course guidance — do not tell participants which button to press.

Physician and case-manager scenarios are **not** part of this round's UAT. Their screens are mockups that the hospital takes over; they are only checked by functional test cases TC02, TC03, TC15, TC29–TC33.

---

## 4. Users — Real Users

### 4.1 Target Participants

The team plans to conduct real-user testing sessions with SHPH staff for this round.

| User Group | Planned Users | Planned Sessions |
|---|---|---|
| SHPH nurses / primary-care staff (รพ.สต.) or close proxies | 5 | 5 |
| Classmates/advisors acting as domain-knowledgeable reviewers | 2 | 2 |
| **Total** | **7** | **7** |

Number of sessions: v1.0 planned 10 sessions including 3 physicians; physician and case-manager testing is now done by the hospital, so the total was reduced to 7. The final number follows the instructor's arrangement.

Note for the team: if real SHPH participants cannot be arranged in time, document this honestly in the Test Plan (who you actually tested with and why), per the course's "no fabricated interviews" rule.

### 4.2 Testing Procedure

1. Explain the testing purpose and obtain consent (PDPA — ask before recording audio/video).
2. Assign a participant ID (U01–U07) instead of using real names.
3. Provide the SHPH scenarios from Section 3.2.
4. Have the participant do the task the old/manual way first if applicable, to capture a "before" baseline.
5. Allow the participant to complete tasks independently; observe and think-aloud, do not help or point.
6. Record task success, completion time, errors, and issues.
7. Collect questionnaire responses (if used) and a short closing interview.
8. Store anonymised testing evidence (consent form, raw data sheet, notes).

Each session is expected to take approximately 25–30 minutes.

**Success counting rules:** completed independently counts as success; completed with help or not completed counts as failure. Task Success Rate is calculated per scenario.

---

## 5. Metrics — How You Measure

### 5.1 Required: Task Performance (Before / After)

The team will compare relevant tasks completed through the existing manual screening process (phone call / paper record for eGFR, manual risk judgement) and through MRSS.

| Metric | Before (Existing/Manual Process) | After (MRSS) |
|---|---|---|
| Time to retrieve a patient's eGFR at the point of care | To be measured | To be measured |
| Time to complete one full screening (dose check + MALA assessment) | To be measured | To be measured |
| Time to alert a doctor about a dose-fail case | To be measured | To be measured |
| Task Success Rate | To be measured | To be measured |
| Number of Errors | To be measured | To be measured |

Task Success Rate (%) = (Successful Tasks / Attempted Tasks) × 100

### 5.2 Optional: System Usability Scale (SUS)

The team proposes using SUS to evaluate ease of use for SHPH staff, who interact most with the screening UI.
- 10 standardised questions, 5-point Likert scale, total score 0–100
- Compare the average score with the benchmark of 68

### 5.3 Optional: Technology Acceptance Model (TAM)

MRSS is a workplace system that clinical staff will be required to use as part of their job (not an optional consumer app), so **TAM** is the better-fitting model per course guidance, rather than UTAUT2.

- Perceived Usefulness (PU) — e.g. "Using MRSS helps me screen patients for MALA risk more quickly/accurately."
- Perceived Ease of Use (PEOU) — e.g. "Learning to use MRSS is easy for me."
- Behavioral Intention (BI) — e.g. "I intend to use MRSS in my work."

3–4 items per construct, adapted from Davis (1989), given after the tasks (and after SUS, if both are used). The team will report the mean score and standard deviation per construct, and will not claim causal relationships between constructs.

Final questionnaire selection: if standard instruments are not used, the team must document an alternative evaluation method and its evidence.

---

## 6. Testing Schedule

All dates in v1.0 (Oct 27 – Nov 18) have been superseded. UAT is set for Tuesday, November 17, 2026; the other dates below are the team's working plan.

### 6.1 Revised Activities

| Date | Activity | Owner | Status |
|---|---|---|---|
| Nov 15–16 | Update the prototype to the new flow (dose check first, case manager role) | Product Owner + AI Lead | To Do |
| Nov 15–16 | Verify the test cases against the revised prototype | Product Owner + AI Lead | To Do |
| Nov 16 | Prepare UAT script, observer form, consent form for the SHPH scenarios | Product Owner | To Do |
| Nov 15–16 | Run Unit & Integration tests (dose check, Part 2 score, routing); fix bugs | Product Owner + AI Lead | To Do |
| Nov 16 | Run E2E tests for the critical flows (dose-fail → doctor; at risk → case manager queue) | Product Owner + QA Lead | To Do |
| Nov 16 | Set up staging environment + seed data (patients for dose-pass, dose-fail, at-risk, low-risk) and participants U01–U07 | Product Owner | To Do |
| Nov 17 | ★ UAT with SHPH staff | Moderator + Note-taker | To Do |
| After Nov 17 | Calculate SUS/TAM scores, Before/After comparison | QA Lead | To Do |
| After Nov 17 | Prepare final test summary for report and presentation | All members | To Do |

### 6.2 Exit Criteria (Testing is Complete When All Are Met)

| Criterion | Target |
|---|---|
| Unit / Integration Tests passing | ≥ 90% pass rate |
| E2E core flows passing (SHPH screening → dose-fail doctor alert; SHPH screening → at-risk case in the case manager queue) | 100% in the tested browser |
| UAT sessions completed | All planned SHPH sessions done |
| Task Success Rate | ≥ 70% per task |
| SUS Score (overall, if used) | ≥ 68 (Acceptable) |
| TAM Score (all constructs, if used) | ≥ 3.5 / 5.0 |
| Open Critical bugs | = 0 |

### 6.3 Defect Severity Classification

| Severity | Definition | Example (MRSS) | Fix SLA |
|---|---|---|---|
| Critical | System is unusable, or a patient-safety-relevant calculation or routing is wrong | Dose check passes an unsafe dose; a dose-fail case is not closed or the doctor is not alerted; cannot log in; data loss | Fix before UAT |
| High | Core feature fails | eGFR fails to auto-pull; an at-risk case does not reach the case manager queue; an at-risk case is sent to the doctor | Fix within 1 day |
| Medium | Feature works but causes confusion | Unclear "stop the drug" or at-risk wording; confusing screening-form flow | Fix before Final Presentation |
| Low | Cosmetic / visual issue | Colour inconsistency, typo, minor layout issue | Fix if time permits |
