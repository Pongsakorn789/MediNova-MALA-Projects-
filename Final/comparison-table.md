# Comparison Table: Before vs After — MRSS (MALA Risk Screening & Monitoring System)

**Version:** 1.0 (DRAFT, to be reviewed by the team before commit)
**Presented by:** Team MediNova
**Related:** `case-study.md`, `UAT_Results_MediNova.md`, `sus_tam_results.md`, `sus_tam_responses.csv`, `Test_Plan_MediNova.md` (Section 5.1)

> **AI Disclosure:** AI helped draft this table from the team's files above. Every figure is copied from those files. The team must check each figure against its source before submission. Items marked **[CONFIRM]** are not known and must not be guessed.

## How to read this table

- **Before** = the existing process at the PCU (รพ.สต.). The team **did not measure a Before baseline** (Test Plan Section 5.1 lists every Before value as "To be measured"). So no Before number is shown, and **no Change is calculated** where the Before is missing. A change is never estimated.
- **After** = the MRSS prototype, as tested in the UAT of 13 Nov 2026 (9 participants, 8 independent sessions; most are proxy users, not real PCU staff). Those sessions most likely ran on the **earlier flow**, not the final flow (see `UAT_Results_MediNova.md`, Section 6).
- Targets come from Test Plan Section 6.2.

## 1. Task performance

| Item | Before | After | Change | Measured with | Evidence |
|---|---|---|---|---|---|
| Time to complete one full screening | Not measured [CONFIRM: any manual timing the team has] | Median **100 s** (mean 97.2 s all 9; 102.1 s Group B) | Not computable (no baseline) | Screen recordings written on observer forms; tolerance ±8 s | `UAT_Results_MediNova.md` §4.1–4.2 |
| Task success: screen a patient start to finish (UAT01) | Not measured | **44.4%** (4/9 as on forms); 50.0% (4/8, pair counted once); 57.1% (4/7, Group B) | Not computable. Below the ≥70% target in every counting | Observer forms; success = finished alone, help = failure | `UAT_Results_MediNova.md` §4, §5, §7 |
| Task success: refer a high-risk case (UAT02) | Not measured | **66.7%** (6/9); 62.5% (5/8); 57.1% (4/7, Group B) | Not computable. Below target in every counting. Concerns the earlier "send to doctor" button, so it is not a result for the final flow | Same | Same |
| Task success: low-risk case and guidance, start next patient, search for a patient | Not measured | **100%** each (9/9; 8/8; 7/7) | Not computable. Meets target | Same | Same |
| Errors per task | Not measured | Mean **1.67** (UAT01), **1.33** (UAT02), 0 to 0.11 (the other tasks) | Not computable | Error counts on observer forms ("-" counted as 0 [CONFIRM]) | `UAT_Results_MediNova.md` §4.1 |
| Time to alert a doctor about a dose-fail case | Not measured | Not measured. By design the case is added to the doctor's queue immediately; the physician screen is a mockup | Not computable | Test cases TC12–TC15 (status: Pending, not yet run) | `Test_Plan_MediNova.md` §3.1 |

## 2. Access to risk information at dispensing

| Item | Before | After | Change | Measured with | Evidence |
|---|---|---|---|---|---|
| Patient's eGFR visible to PCU staff when dispensing | **No**: staff cannot see it | The prototype shows the latest eGFR on the patient card and uses it in the dose check (prototype data are invented test patients) | Not visible → shown (qualitative, **not timed**) | Check of the live prototype on 16 Nov; TC05, TC10 are Pending | `case-study.md` §1; `Test_Plan_MediNova.md` §3.1 |
| Risk screening at each Metformin dispensing | None: no weight, height or alcohol data collected | Dose check (Part 1) then MALA score (Part 2: eGFR <60 = 1, BMI <23 = 2, heavy alcohol = 2; at risk ≥2) | None → rule-based screening on every use (design; **clinical benefit not measured**) | Test Plan v1.3 description, UAT run of the screening flow | `case-study.md` §2.1; `Test_Plan_MediNova.md` §1 |
| Routing of a flagged case | No defined path from the PCU | Dose fail → doctor's queue; at risk → MALA case manager queue, automatic after saving | Qualitative. Final routing not tested with users | Test cases TC24, TC29–TC32 (Pending) | `Test_Plan_MediNova.md` §3.1 |

## 3. Usability and acceptance

| Item | Before | After | Change | Measured with | Evidence |
|---|---|---|---|---|---|
| SUS (0–100) | No baseline (SUS applies to MRSS only) | **80.9** (SD 9.8; 65.0–90.0), n = 8. Target ≥68. Two of 8 scored 70.0 and 65.0 | Not applicable | Brooke SUS, online form after UAT; recomputed from the CSV | `sus_tam_results.md` §2; `sus_tam_responses.csv` |
| TAM: Perceived Usefulness | No baseline | **4.17** / 5 (SD 0.31), n = 8. Target ≥3.5 | Not applicable | 3 items, 1–5 scale | Same |
| TAM: Perceived Ease of Use | No baseline | **4.38** / 5 (SD 0.45), n = 8 | Not applicable | Same | Same |
| TAM: Behavioral Intention | No baseline | **4.21** / 5 (SD 0.75), n = 8; lowest single score 2.67 | Not applicable | Same | Same |

## 4. Use of AI

| Item | Before | After | Change | Measured with | Evidence |
|---|---|---|---|---|---|
| AI in the product (risk results and advice) | No AI | **No trained AI model.** Fixed rules (CPG dose table, additive score) and rule-based advice. Reading the HN from a photo with AI is not part of the prototype | No change in AI use. The risk output is deterministic and can be checked against the rules | Test Plan statement; rule definitions; unit tests TC23, TC36 (Pending) | `case-study.md` §2.1; `Test_Plan_MediNova.md` AI Disclosure and §1 |
| AI used by the team to prepare project documents | Not applicable | AI helped draft the Test Plan revisions, UAT Results, SUS/TAM results, case study and this table. Each file carries an AI disclosure. The team checks all content and figures against the sources | Not applicable | Disclosure notes in each file; team review [CONFIRM: who reviewed which file] | Header of each file |

## 5. Limits (state these in the presentation)

- No Before baseline was measured, so the table shows After values and qualitative differences only. Measure one if time allows: for example time to find a patient's eGFR by the current manual route [CONFIRM].
- 7 of the 8 independent UAT sessions (U03–U09: 6 students and 1 lecturer) are PROXY users; no real PCU staff are in the sample. Results show general usability, not acceptance by PCU staff, and do **not** prove clinical benefit.
- U01–U02 were observed together and saw a demonstration first, so the 9 participants are 8 independent sessions.
- The UAT used the earlier prototype flow. Retest the final flow before claiming these figures for it.
- Times of 5–10 s are inside the ±8 s error margin and are not reported as results.
- n = 8–9 is small; one person moves a task success rate by 11–14 points.
- Test cases marked "Pending" in the Test Plan have not been run yet; do not present them as passed.
