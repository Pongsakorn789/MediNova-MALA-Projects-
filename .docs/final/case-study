# Case Study: MALA Risk Screening & Monitoring System (MRSS)

**Live prototype:** https://mala-project-2026.web.app
**Presented by:** Team MediNova
**Version:** 1.4

> **AI Disclosure:** AI helped draft this file from the team's project overview, Test Plan v1.3, `UAT_Results_MediNova.md` and `sus_tam_results.md`. Every figure is copied from those files. The team must check the figures against the sources before submission. Items marked **[CONFIRM]** are not known and must not be guessed.

---

## 1. Problem

Hospitals send Metformin to Primary Care Units (PCUs, รพ.สต.) so that stable diabetic patients can collect it close to home and reduce hospital crowding. The staff who dispense the drug at the PCU **cannot see the patient's eGFR** and **do not collect other risk data** (weight, height, alcohol use). Patients therefore receive Metformin with no risk screening.

Metformin-associated lactic acidosis (MALA) is a severe complication of Metformin use. This case study makes no claim about treatment cost per case, because the team has no checked source for it yet.

**Who is affected**
- Diabetic patients on continuous Metformin in a PCU catchment area, especially the elderly and those with reduced kidney function.
- PCU Staff, who dispense the drug and meet patients directly but have no screening tool.

**Goal:** every time Metformin is dispensed through a PCU, risk is screened and recorded systematically, completely, and in a way that can be reviewed afterwards.

## 2. Process

### 2.1 Solution

A mobile-friendly web application for PCU Staff. The system holds each patient's latest eGFR and current Metformin dose, then applies fixed rules in this order (the system uses fixed rules and does not use a trained AI model):

| Part | Rule | Result |
|---|---|---|
| **Part 1: dose check** (source: CPG table) | **Pass:** eGFR >= 45 and Metformin <= 2,000 mg/day, or eGFR 30–44 and Metformin <= 1,000 mg/day. **Fail:** otherwise | Fail: assessment stops and the case goes to the **doctor's queue** immediately. The system only records and forwards; staff follow the doctor's order |
| **Part 2: MALA risk score** (source: research) | eGFR < 60 = 1 point; BMI < 23 = 2 points; heavy alcohol use = 2 points. **Score >= 2 = at risk** | At risk: the case enters the **MALA case manager queue** automatically after saving |

Vomiting or diarrhea, poor oral intake, painkillers / "yaa chud" and herbal remedies are shown with the result but **not scored**. The result screen shows rule-based advice that staff can read to the patient (avoid alcohol, Sick Day Rule, warning symptoms, fluids, avoid NSAIDs, repeat kidney check).

**Screening flow (5 steps):** identify the patient (scan, search the list, Excel import, or manual entry) -> weight -> height (BMI calculated) -> risk factors (Yes / No) -> review and save. Each screening is saved with its result so it can be reviewed; the full audit log is a hospital-side function and is not part of the prototype (Test Plan v1.3, out of scope).

### 2.2 Design decisions

- **Dose check first.** An unsafe dose is not assessed further; the doctor receives the case at once.
- **Automatic routing.** At-risk cases reach the case manager without a button press. The PCU Staff screen has no "send to doctor" button and no LINE button (changed in the 15–16 Nov update, Test Plan v1.3).
- **LINE alert.** Optional, on the case manager side, handled by the hospital, and may not be connected in the prototype.
- **Scope.** The PCU Staff flow is fully implemented. The physician and case manager screens are mockups (display and routing); the hospital implements the real functions.
- **Data protection.** The system handles identifiable patient data, so PDPA and hospital policy apply (restricted access and an audit log are required, but the hospital implements them; they are not in the prototype).

### 2.3 Testing

| Activity | What was done |
|---|---|
| **UAT** (13 Nov 2026) | 9 participants (U01–U09), observer forms. Scenarios were given without button-by-button instructions. Success = finished alone; finished with help = failure. Source: `UAT_Results_MediNova.md`. |
| **SUS / TAM** | Online form after UAT, n = 8 responses (all 10 SUS and 9 TAM items answered). Sources: `sus_tam_results.md`, `sus_tam_responses.csv`. |

**Participants:** U01–U02 are medical personnel who were **observed together** (identical values on both forms) and were shown a demonstration first. So the 9 participants make **8 independent sessions**. U03–U09 (6 students and 1 lecturer) are **proxy users** acting in the PCU Staff role and were not shown a demonstration (Group B). No real PCU Staff are in the sample. [CONFIRM: workplace of U01–U02.]

**Important:** UAT sessions U01–U08 were held before the 15–16 Nov prototype update, so they most likely ran on the **earlier flow** (with a "send to doctor" button). [CONFIRM: prototype version for each session, including U09.]

## 3. Outcome

### 3.1 Usability and acceptance (SUS / TAM, n = 8)

| Measure | Result | Target | Met? |
|---|---|---|---|
| SUS (0–100) | **80.9** (SD 9.8; range 65.0–90.0) | >= 68 | Yes |
| TAM: Perceived Usefulness | **4.17** / 5 | >= 3.5 | Yes |
| TAM: Perceived Ease of Use | **4.38** / 5 | >= 3.5 | Yes |
| TAM: Behavioral Intention | **4.21** / 5 (min 2.67) | >= 3.5 | Yes |

Two of 8 respondents scored SUS at 70.0 and 65.0, so the result is not uniform. The scores were recomputed from `sus_tam_responses.csv` and match. 8 of 9 UAT participants responded. [CONFIRM: both files are pushed to the repo.]

### 3.2 Task success (UAT)

Counted three ways, because U01–U02 are one pair and Group A saw a demonstration:

| Scenario | All 9 participants (as on forms) | Pair counted once (8 sessions) | Group B only (7, no demo) |
|---|---|---|---|
| Screening start to finish | 4/9 (**44.4%**) | 4/8 (**50.0%**) | 4/7 (**57.1%**) |
| Refer a high-risk case | 6/9 (**66.7%**) | 5/8 (**62.5%**) | 4/7 (**57.1%**) |
| Low-risk case and guidance | 9/9 (100%) | 8/8 (100%) | 7/7 (100%) |
| Start the next patient | 9/9 (100%) | 8/8 (100%) | 7/7 (100%) |
| Search for a patient | 9/9 (100%) | 8/8 (100%) | 7/7 (100%) |

Median full screening time was about 100 seconds (from screen recordings, tolerance +/- 8 s). Times of 5–10 seconds are within the error margin and are not reported as results.

### 3.3 Honest reading of the results

- **Two scenarios missed the >= 70% target** in every way of counting: full screening and referring a high-risk case.
- **A high SUS does not mean every task was easy.** 6 of 9 participants needed help in at least one task.
- **Main usability issues found:** weight entered in the wrong field or with a wrong value (U01, U02, U05, U06); users did not know what to do after the red high-risk alert (U03, U04, U08; U09 also needed help, reason not recorded); one unclear page ("ปัจจัยเสี่ยง", U03). One respondent said the scan step may confuse people if nobody teaches them first.
- **The final flow was not tested in these sessions.** The referral problem concerns the earlier "send to doctor" button, which the final flow removed. Retest is needed. These figures are not results for the final flow.
- Results show general usability with proxy users. They do **not** show acceptance by real PCU Staff and do **not** prove clinical benefit.

### 3.4 Expected benefits (design intent, not measured)

- Screen for MALA risk at every dispensing, before harm occurs.
- Unsafe doses reach the doctor early.
- Complete screening records saved in one place that the PCU and the hospital can both review (audit and access control to be implemented by the hospital).
- Less manual work through automatic routing.

### 3.5 Next steps

1. Retest the final flow with real PCU Staff to confirm the referral and weight-entry fixes.
2. Verify the remaining prototype items marked "to verify" in Test Plan v1.3 (Excel import, offline mode, dose-fail screen for eGFR below 30).
3. Clean up leftover wording from earlier versions (known display issues listed in Test Plan v1.3, Section 3.1).
4. Pilot at one or more PCUs, then extend to the wider hospital network. [CONFIRM: pilot site and scope with the mentor.]
5. The hospital implements the real physician and case manager functions and decides whether to connect the optional LINE alert.

## 4. Live URL

**https://mala-project-2026.web.app**

## 5. Evidence

| File | Content |
|---|---|
| `Test_Plan_MediNova.md` (v1.3) | Method, scenarios, exit criteria |
| `UAT_Results_MediNova.md` | Task success, times, issues |
| `sus_tam_results.md` | SUS and TAM scores |
| `sus_tam_responses.csv` | Anonymised raw responses |
