# Test Plan — MRSS (MALA Risk Screening & Monitoring System)
Sub-district Health-Promoting Hospital (SHPH / รพ.สต.) Risk Screening Web Application

**Version:** 1.0
**Date Prepared:** October 28, 2026
**Submission Deadline:** before Wednesday, October 28, 2026
**UAT Date:** Wednesday, November 4, 2026
**System under test (prototype):** mala-project-2026.web.app

**AI Disclosure:** AI was used to help draft the structure, tables, and initial test-case wording of this document, based on the team's backlog, proposal, and workflow documentation, and verified against live-prototype screenshots supplied by the team. All content has been reviewed by the team.

| Role | Name | Student ID |
|---|---|---|
| Product Owner | Pongsakorn Chaikittiporn | 6631503114 |
| AI Lead | Maneenat Wantanasri | 6631503035 |
| QA / Test Lead | Thanakit Chupviroj | 6631503020 |
| UX/UI Designer | Phorn Potjanasuj | 6631503029 |

*Team of 4 — no separate Tech Lead; development ownership is shared across the team, led by the Product Owner.*

> ✅ **Verified against the live prototype on Oct 27–28, 2026** (screenshots supplied by the team), covering both confirmed roles: the SHPH (รพ.สต.) 5-step screening wizard + Risk Score result + escalation actions, and the Physician/team ("แพทย์ / ทีมสหวิชาชีพ") Dashboard, patient list, and Settings screen. The system has exactly **two** login roles.

---

## 1. Scope

Verify that each of the system's two user roles (SHPH staff and Physician/team) can complete their part of the MALA screening workflow correctly, that the Risk Engine computes and classifies risk as specified, and that high-risk cases are escalated and closed out correctly. Compare task outcomes before and after using the proposed evaluation design in Section 5.

> **Confirmed final:** the system has only **two** login roles — "เจ้าหน้าที่ รพ.สต." (SHPH staff) and "แพทย์ / ทีมสหวิชาชีพ" (Physician/team).

**In scope — confirmed, SHPH screening flow**
- Login screen with role selector (**"เจ้าหน้าที่ รพ.สต."** / **"แพทย์ / ทีมสหวิชาชีพ"**) plus username (e.g. `SHPH001`) and password fields
- 5-step screening wizard: (1) สแกน HN — camera scan, search fallback, or "โหมดจำลอง" mock mode pulling a random patient from the hospital DB; (2) น้ำหนักวันนี้ — numeric keypad, shows delta vs. previous weight; (3) ส่วนสูง — numeric keypad, auto-computes BMI; (4) ปัจจัยเสี่ยง — 4-level alcohol selector + dehydration/diarrhea toggle; (5) ตรวจทานข้อมูล — full review before submit
- Risk Score result screen: numeric "คะแนนความเสี่ยง MALA" with a threshold bar (cutoff **60** in the sample seen), a risk-tier label, and contributing factors listed
- AI-generated recommendations block, including a "กฎวันป่วย (Sick Day Rule)" warning card, alcohol-avoidance note, and hospital warning signs
- Two independent escalation buttons on a high-risk result: "ส่งเคสเข้าคิวแพทย์" (confirms as "ส่งเข้าคิวแพทย์แล้ว · รอแพทย์สั่งยา") and a separate "ส่งแจ้งเตือนเข้ากลุ่ม LINE" — **manual, not automatic**
- "เสร็จสิ้น · สแกนผู้ป่วยรายถัดไป" to end the flow and start the next patient

**In scope — confirmed, Physician / team role**
- Dashboard ("ภาพรวมการเฝ้าระวัง MALA") with 3 stat cards: pending dose decisions ("รอพิจารณาปรับยา", Red Tier count), cases adjusted ("ปรับยาแล้ว"), and adjustment rate (e.g. "83% · 10 จาก 12 เคสที่ส่งต่อ")
- Two tabs: "รอสั่งการรักษา" and "สั่งยาแล้ว", each listing patients with name, HN, eGFR, current dose, and status tag
- A "สั่งจ่ายยา" action per pending patient
- Full patient list ("คนไข้ในความดูแล") with search (name / ID card) and status filter tabs: ทั้งหมด / รอแพทย์พิจารณา / ปรับยาแล้ว
- Notification bell with unread count
- Settings page with two toggles: **"การแจ้งเตือนด้วยเสียง (Audible Emergency Alarm)"** (Red Tier only, to prevent alarm fatigue) and **"การยืนยันตัวตนก่อนบันทึกคำสั่งยา (MOPH Digital Signature)"** — the system's implementation of the ETA §9 e-signature requirement

**In scope — not yet confirmed on screen; verify before final test-case pass**
- The actual "สั่งจ่ายยา" dose-order form itself (entry point confirmed, fields not yet seen)
- Role-based access restrictions and the audit log (not visible from the UI; needs backend verification)
- Offline save + auto-sync behaviour for field data capture
- AI-assisted camera OCR scanning of the HN (the live build's mock mode bypasses this)

**Out of scope for this cycle:** large-scale load testing, exhaustive penetration testing, and internal testing of third-party services (the hospital's HosXP database, Firebase/hosting infrastructure, LINE's own delivery reliability). Basic access-control checks and graceful handling of an unreachable external service remain in scope.

---

## 2. Levels — Which Test Levels

The team will perform four levels of testing following the Testing Pyramid, per course guidance (14 Oct lecture).

| Test Level | Testing Purpose | MRSS Example |
|---|---|---|
| Unit Testing | Verify individual functions | Validate the Risk Engine's additive scoring function (eGFR<60=+1, BMI<23=+2, heavy alcohol=+2) against known inputs |
| Integration Testing | Verify communication between components | Confirm a screening submitted by SHPH correctly stores the computed Risk Score and triggers the escalation check |
| End-to-End (E2E) Testing | Verify the complete workflow | SHPH screens a patient end-to-end → Risk Engine classifies → high-risk case reaches the Physician dashboard → dose order recorded |
| User Acceptance Testing (UAT) | Evaluate system usability with real users | SHPH- and Physician-role users perform realistic screening and escalation tasks |

---

## 3. Test Cases — Test Cases and UAT Scenarios

### 3.1 Functional Test Cases

| ID | Test Case | Test Steps | Test Data | Expected Result | Pass/Fail |
|---|---|---|---|---|---|
| TC01 | Valid login — SHPH | Select role "เจ้าหน้าที่ รพ.สต.", enter valid username/password, submit | `SHPH001` + valid password | Login succeeds; Step 1/5 "สแกน HN" screen loads | Pending |
| TC01a | Valid login — Physician/team | Select role "แพทย์ / ทีมสหวิชาชีพ", enter valid credentials (e.g. `DOC001`), submit | Valid physician test account | Login succeeds; "ภาพรวมการเฝ้าระวัง MALA" dashboard loads with 3 stat cards and "รอสั่งการรักษา" tab active | Pending |
| TC02 | Invalid login | Enter incorrect credentials and submit | Invalid password | Access denied, no session created, error shown | Pending |
| TC03 | Scan HN — mock mode | From Step 1/5, tap "แตะเพื่อสแกน" | Mock/demo mode (random patient) | A patient record loads (e.g. "นายสมชาย รักชาติ", HN 50-112345, age 65) with risk level and eGFR shown | Pending |
| TC04 | Scan HN — search fallback | Tap "ค้นหาด้วยชื่อหรือเลขบัตรประชาชนแทน" and search | Valid patient name/ID | Correct matching patient record is found and selected | Pending |
| TC05 | Confirm scanned HN | On the HN-confirmation card, tap "ถูกต้อง · ถัดไป" | Patient matched in TC03/TC04 | Proceeds to Step 2/5 (น้ำหนัก) with the confirmed patient carried forward | Pending |
| TC06 | Re-scan if HN is wrong | On the HN-confirmation card, tap "สแกนใหม่" | A mismatched patient shown | Returns to the scan screen to retry, without carrying over the wrong patient | Pending |
| TC07 | Auto-pull of eGFR | Observe the HN-confirmation card and the Step 5 review screen | Patient with a known eGFR on file | Latest eGFR (e.g. "45 mL/min") auto-populates with no manual entry anywhere in the flow | Pending |
| TC08 | Enter today's weight | On Step 2/5, enter weight via numeric keypad, tap "ถัดไป · ส่วนสูง" | Weight value, e.g. 59 | Value saved; delta vs. previous weight shown (e.g. "ครั้งก่อน 67 กก. · -8 กก.") | Pending |
| TC09 | Enter height & auto-BMI | On Step 3/5, enter a height via the numeric keypad | Height value, e.g. 170 | BMI computed and displayed automatically (e.g. "BMI 20.42 kg/m²") | Pending |
| TC10 | Select alcohol-consumption level | On Step 4/5 "ปัจจัยเสี่ยง", select one of the 4 options | ไม่ดื่มเลย / ดื่มเป็นครั้งคราว / ดื่มประจำ / ดื่มหนัก | Selected level is highlighted and carried into the risk calculation | Pending |
| TC11 | Toggle dehydration/diarrhea symptom | On Step 4/5, toggle "มีอาการขาดน้ำ" vs. "ไม่มีอาการ" | Symptom = present | Toggle state saved and reflected on the Step 5 review screen | Pending |
| TC12 | Review screen accuracy | Complete Steps 1–4, reach Step 5 "ตรวจทานข้อมูล" | Full sample screening (TC03–TC11) | Patient info, eGFR, today's weight/height/BMI, and all risk-factor answers shown correctly | Pending |
| TC13 | Submit screening & compute Risk Score | On Step 5, tap "บันทึกและประเมินความเสี่ยง" | Completed screening data | Risk Score result screen appears with a score (e.g. "67"), threshold bar (e.g. "เกณฑ์ 60"), risk-tier label | Pending |
| TC14 | Risk Score — contributing factors shown | Review the result screen's summary line | High-risk sample from TC13 | Summary lists the factors that drove the score (eGFR, age, alcohol, etc.) | Pending |
| TC15 | Risk Engine Part 1 — CPG dose rule `[TO CONFIRM UI]` | Submit a screening with eGFR ≥45 | eGFR 50, standard dose | System treats the current dose as within the CPG limit (≤2000 mg/day) | Pending |
| TC16 | Risk Engine Part 1 — contraindicated range | Submit a screening with eGFR <30 | eGFR <30 | System flags Metformin as contraindicated / most severe tier | Pending |
| TC17 | Risk Engine — additive scoring sanity check | Compare a low-risk vs. a high-risk profile | Two contrasting sample profiles | The high-risk case scores meaningfully higher and crosses into the red tier | Pending |
| TC18 | Sick Day Rule trigger | Submit a screening with dehydration/diarrhea = present | Symptom present | The result screen shows the "กฎวันป่วย (Sick Day Rule)" warning card | Pending |
| TC19 | Green-tier result — no urgent escalation `[TO CONFIRM]` | Submit a low-risk screening | Low-risk sample | Score below the threshold line; escalation cards not pushed as urgent (layout not yet screenshotted) | Pending |
| TC20 | Red-tier — send to physician queue | Tap "ส่งเคสเข้าคิวแพทย์" under "ส่งปรึกษาแพทย์ รพ.ศูนย์" | High-risk sample from TC13 | Button changes to "ส่งเข้าคิวแพทย์แล้ว · รอแพทย์สั่งยา" | Pending |
| TC21 | Red-tier — send LINE group alert | Tap "ส่งแจ้งเตือนเข้ากลุ่ม LINE" | High-risk sample from TC13 | Preview card (patient, site, eGFR, dose) is sent — a manual, separate action from TC20 | Pending |
| TC22 | Finish & start next patient | Tap "เสร็จสิ้น · สแกนผู้ป่วยรายถัดไป" | Completed screening | Returns to Step 1/5 "สแกน HN" ready for the next patient | Pending |
| TC23 | Physician sees the case on the dashboard | Log in as physician, check "รอสั่งการรักษา" for the case sent in TC20 | Case escalated in TC20 | Case appears in the pending list with correct data `[TO CONFIRM latency]` | Pending |
| TC24 | Dashboard stat counts update | Note "รอพิจารณาปรับยา" count before/after TC20/TC23 | Before/after counts | The pending-count stat increments by 1 | Pending |
| TC25 | Place a dose order | From the pending list, tap "สั่งจ่ายยา" | Pending case | Order form/flow opens `[TO CONFIRM fields]` | Pending |
| TC26 | MOPH Digital Signature on order | Submit a dose order with the signature toggle enabled | Dose order + license credential | Order cannot finalize without signature; once signed, moves to "สั่งยาแล้ว" | Pending |
| TC27 | Audible Emergency Alarm scope | Toggle alarm on; trigger a Red Tier case vs. a non-critical one | One Red Tier, one lower-risk case | Sound fires only for the Red Tier / critical-eGFR case | Pending |
| TC28 | Patient list search | Search by patient name and by ID card number | Known patient's name / ID | Matching patient(s) filtered correctly in both modes | Pending |
| TC29 | Patient list status filter | Switch between ทั้งหมด / รอแพทย์พิจารณา / ปรับยาแล้ว | 12-patient sample list | Each tab shows only patients matching that status | Pending |
| TC30 | Role-based access — SHPH restriction `[TO CONFIRM]` | Log in as SHPH, attempt to reach the physician's screens | SHPH test account | Action blocked or option not available | Pending |
| TC31 | Unauthorized access | Attempt to open another facility's record directly | Unauthorized record reference | Access denied | Pending |
| TC32 | Audit log `[TO CONFIRM]` | Perform a login, submit, queue/LINE send, and signed order | Standard test actions | Each action logged with user ID, action, timestamp | Pending |

**Note:** Pending means the test has not yet been executed. The team will record actual Pass/Fail results during testing, and must clear each remaining `[TO CONFIRM]` tag once verified against the live prototype.

### 3.2 UAT Scenarios

| UAT ID | User Role | Scenario | Success Criteria |
|---|---|---|---|
| UAT01 | SHPH staff | "A patient has come in for their Metformin refill. Pull up their record and screen them from start to finish." | Participant completes all 5 steps unaided and reaches a Risk Score result |
| UAT02 | SHPH staff | "The system just told you this patient is high risk. Do whatever you think is needed next." | Participant finds and uses both escalation buttons without being told they are separate |
| UAT03 | SHPH staff | "You made a mistake and scanned the wrong patient. Fix it." | Participant finds "สแกนใหม่" and corrects the patient before continuing |
| UAT04 | SHPH staff | "A patient is low-risk this time. Finish the screening and tell me what the system recommends." | Participant reaches a result screen and correctly reports the guidance shown |
| UAT05 | Physician / team | "A high-risk case just came in. Find it on your dashboard and place a dose order for it." | Participant finds the case, completes the order (incl. signature if enabled), sees it move to "สั่งยาแล้ว" |
| UAT06 | Physician / team | "Find [a specific patient] in your full patient list without using the dashboard shortcuts." | Participant uses search or status filters to locate the patient correctly |
| UAT07 | SHPH staff | "Start a new screening for another patient without restarting the app." | Participant uses "เสร็จสิ้น · สแกนผู้ป่วยรายถัดไป" to move to the next patient smoothly |

Users will receive scenarios rather than step-by-step button instructions, per course guidance — do not tell participants which button to press.

---

## 4. Users — Real Users

### 4.1 Target Participants

The team plans to conduct at least 10 real-user testing sessions.

| User Group | Planned Users | Planned Sessions |
|---|---|---|
| SHPH nurses / primary-care staff (รพ.สต.) or close proxies | 5 | 5 |
| Physicians / clinical instructors (or a qualified clinical proxy) | 3 | 3 |
| Classmates/advisors acting as domain-knowledgeable reviewers | 2 | 2 |
| **Total** | **10** | **10** |

`[TO CONFIRM]` Note for the team: if real SHPH/physician participants cannot be arranged in time for Nov 4, document this honestly in the Test Plan (who you actually tested with and why), per the course's "no fabricated interviews" rule.

### 4.2 Testing Procedure

1. Explain the testing purpose and obtain consent (PDPA — ask before recording audio/video).
2. Assign a participant ID (U01–U10) instead of using real names.
3. Provide tasks appropriate to the participant's role (SHPH or Physician scenario from Section 3.2).
4. Have the participant do the task the old/manual way first if applicable, to capture a "before" baseline.
5. Allow the participant to complete tasks independently; observe and think-aloud, do not help or point.
6. Record task success, completion time, errors, and issues.
7. Collect questionnaire responses (if used) and a short closing interview.
8. Store anonymised testing evidence (consent form, raw data sheet, notes).

Each session is expected to take approximately 25–30 minutes.

---

## 5. Metrics — How You Measure

### 5.1 Required: Task Performance (Before / After)

The team will compare relevant tasks completed through the existing manual screening process (phone call / paper record for eGFR, manual risk judgement) and through MRSS.

| Metric | Before (Existing/Manual Process) | After (MRSS) |
|---|---|---|
| Time to retrieve a patient's eGFR at the point of care | To be measured | To be measured |
| Time to complete one full 5-step screening (target: ≤2 min) | To be measured | To be measured |
| Time to escalate a high-risk case to a physician | To be measured | To be measured |
| Task Success Rate | To be measured | To be measured |
| Number of Errors | To be measured | To be measured |

Task Success Rate (%) = (Successful Tasks / Attempted Tasks) × 100

### 5.2 Optional: System Usability Scale (SUS)

The team proposes using SUS to evaluate ease of use for SHPH and physician users (who interact most with the full web UI).
- 10 standardised questions, 5-point Likert scale, total score 0–100
- Compare the average score with the benchmark of 68

### 5.3 Optional: Technology Acceptance Model (TAM)

MRSS is a workplace system that clinical staff will be required to use as part of their job (not an optional consumer app), so **TAM** is the better-fitting model per course guidance, rather than UTAUT2.

- Perceived Usefulness (PU) — e.g. "Using MRSS helps me screen patients for MALA risk more quickly/accurately."
- Perceived Ease of Use (PEOU) — e.g. "Learning to use MRSS is easy for me."
- Behavioral Intention (BI) — e.g. "I intend to use MRSS in my work."

3–4 items per construct, adapted from Davis (1989), given after the tasks (and after SUS, if both are used). The team will report the mean score and standard deviation per construct, and will not claim causal relationships between constructs.

`[TO CONFIRM]`: Final questionnaire selection. If standard instruments are not used, the team must document an alternative evaluation method and its evidence.

---

## 6. Testing Schedule

### 6.1 Master Timeline

| Date | Milestone |
|---|---|
| Oct 27 | Draft Test Plan |
| Oct 28 | ★ Submit Test Plan (Deadline) |
| Oct 29–31 | Unit & Integration Tests |
| Nov 1–3 | E2E tests; prepare staging/seed data; confirm UAT participants |
| Nov 4 | ★ UAT Round 1 & Round 2 (Before/After) |
| Nov 5–10 | Analyze SUS/TAM, Before/After results; fix issues found |
| Nov 11 / 18 | ★ Final Presentation (teams 1–10 on the 11th, teams 11–20 on the 18th) |

### 6.2 Activity Table

| Date | Activity | Owner | Status |
|---|---|---|---|
| Oct 27 | Draft complete Test Plan document | QA Lead (Thanakit) | Done |
| Oct 27–28 | Verify test cases against the live prototype | Product Owner (Pongsakorn) + AI Lead (Maneenat) | Done |
| Oct 27–28 | Write/refine all Functional Test Cases by module | QA Lead + Product Owner | Done |
| Oct 27–28 | Prepare UAT script, observer form, consent form | Product Owner | To Do |
| Oct 28 | ★ Submit Test Plan (Deadline) | Product Owner | To Do |
| Oct 29–31 | Run Unit & Integration tests on the Risk Engine; fix bugs | Product Owner + AI Lead | To Do |
| Nov 1–3 | Run E2E tests for all critical flows | Product Owner + QA Lead | To Do |
| Nov 1–3 | Set up staging environment + seed data (U01–U10) | Product Owner | To Do |
| Nov 1–3 | Contact and confirm UAT participants (U01–U05) | Product Owner | To Do |
| Nov 4 (AM) | ★ UAT Round 1 — U01 through U05 | Moderator + Note-taker | To Do |
| Nov 4 (PM) | Collect Round 1 feedback; prioritize critical fixes | QA Lead | To Do |
| Nov 4 (PM) | ★ UAT Round 2 — U06 through U10 (after critical fixes) | Moderator + Note-taker | To Do |
| Nov 5–9 | Calculate SUS/TAM scores, Before/After comparison | QA Lead | To Do |
| Nov 9–10 | Prepare final test summary for report and presentation | All members | To Do |
| Nov 11 / 18 | ★ Final Presentation | All members | To Do |

### 6.3 Exit Criteria (Testing is Complete When All Are Met)

| Criterion | Target |
|---|---|
| Unit / Integration Tests passing | ≥ 90% pass rate |
| E2E core flows passing (SHPH screening → Physician dose order) | 100% in the tested browser |
| UAT sessions completed | ≥ 10 sessions (Round 1 + Round 2 both done) |
| Task Success Rate | ≥ 70% per task |
| SUS Score (overall, if used) | ≥ 68 (Acceptable) |
| TAM Score (all constructs, if used) | ≥ 3.5 / 5.0 |
| Open Critical bugs | = 0 |

### 6.4 Defect Severity Classification

| Severity | Definition | Example (MRSS) | Fix SLA |
|---|---|---|---|
| Critical | System is unusable, or a patient-safety-relevant calculation is wrong | Risk Engine computes the wrong tier; cannot log in; data loss | Fix before UAT Round 2 |
| High | Core feature fails | eGFR fails to auto-pull; high-risk alert does not reach the physician dashboard | Fix within 1 day |
| Medium | Feature works but causes confusion | Unclear risk-tier labelling; confusing screening-form flow | Fix before Final Presentation |
| Low | Cosmetic / visual issue | Colour inconsistency, typo, minor layout issue | Fix if time permits |

> **Note:** A failure of the "MOPH Digital Signature" step to actually block an unsigned dose order (TC26) should be classified as **Critical**, not just a functional bug — it is the system's implementation of the `rule.md` / Electronic Transactions Act §9 requirement that a dose-adjustment confirmation be provable as a valid e-signature.
