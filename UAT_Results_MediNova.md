# UAT Results — MRSS (MALA Risk Screening & Monitoring System)

**Version:** 1.0
**Test Plan:** `Test_Plan_MediNova.md` (v1.3; update to v1.4 and link to this file)
**System under test:** mala-project-2026.web.app
**Evidence:** observer forms, "แบบฟอร์มสังเกตการณ์", participants U01–U09
**Date of sessions:** 13 November 2026 for U01–U08 [CONFIRM: all 8 sessions on this date]. U09: [CONFIRM: date of the session and which version of the prototype was used]
(13 November is before the instructor's own test on 17 November 2026, and before the
prototype was updated to the flow in Test Plan v1.3 on 15–16 November.)

> **AI Disclosure:** AI helped format this document and compute the summary
> figures from the team's observer forms. The raw values come from the forms.
> The team must check every figure and every comment against the forms and the
> recordings before submission.
>
> Items marked **[CONFIRM]** are not known from the forms and must be filled
> or checked by the team. Do not guess them.

---

## 1. Participants

Codes only. No names or nicknames are stored in this repository.

| ID | Group | Role ticked on the form | Test condition (as written on the form) |
|---|---|---|---|
| U01 | Group A: medical personnel (บุคลากรการแพทย์) | รพ.สต. and แพทย์/ทีม | "ผู้ใช้ดูการสาธิตจากผู้สังเกตการณ์ก่อน 1 รอบ แล้วจึงทดลองเอง" (watched the observer demonstrate once, then tried alone) |
| U02 | Group A: medical personnel (บุคลากรการแพทย์) | รพ.สต. and แพทย์/ทีม | Same as U01 |
| U03–U08 | Group B: university students (นักศึกษามหาวิทยาลัย), outside the team | รพ.สต. | "ผู้ใช้ทดลอง และ สอบถามด้วยตนเอง" (tried the system and asked questions on their own, no demonstration) |
| U09 | Group B: university lecturer (อาจารย์มหาวิทยาลัย) | รพ.สต. | Same as U03–U08 |

- Sessions held: **9** (Test Plan v1.3 planned 7: U01–U07).
- U01 and U02 are medical personnel. [CONFIRM: their workplace (hospital or SHPH/รพ.สต.)
  and whether they count as real users of the screening flow.]
- U03–U09 are **PROXY** users (students and one university lecturer acting in the
  รพ.สต. role), so their results show general usability, not acceptance by real
  รพ.สต. staff. [CONFIRM: whether U09 is the course instructor; if the session was
  the instructor's own test on 17 November, it used the updated flow and must not
  be mixed with the 13 November sessions without saying so.]
- **U01 and U02 were observed together** (recorded values are identical in every
  cell). Treat them as one joint session of two people, not two independent samples.
- **U01 and U02 saw a demonstration first**, so their results measure how well
  people do after being shown the system, not unaided use. Group B did not.
- Consent obtained: [CONFIRM: yes/no, and where the consent form is stored].
- Role tested: รพ.สต. screening flow. The forms of U01 and U02 also contain one
  doctor-role task (แพทย์หาเคสและสั่งจ่ายยา).

## 2. Method

- Scenarios were given without button-by-button instructions. Group A watched a
  demonstration first; Group B tried the system and asked questions themselves
  (see Section 1).
- The Test Plan (Section 4.2) says the observer must not help or point. On the
  forms, "มีคนช่วย" was recorded where the observer did help (for example by
  pointing at a button). Each such case is counted as a failure, as the success
  rule requires.
- The observer recorded the outcome, the time in seconds, the number of errors,
  and comments (see the form).
- **Success rule (Test Plan, Section 4.2):** "สำเร็จเอง" (finished alone) = success;
  "มีคนช่วย" (finished with help) or "ไม่สำเร็จ" (not finished) = failure.
- Task Success Rate (%) = successful tasks / attempted tasks x 100, per scenario.
- A "-" in the errors column on the form is counted as 0 in the means. [CONFIRM:
  that "-" means no errors were counted.]
- Times were taken from the **screen recordings** and written on the form (not
  from a stopwatch), in whole seconds, mostly in steps of 5.
  **Measurement tolerance: +/- 8 s.** [CONFIRM: where the recordings are stored;
  they contain patient test data, so store them as the team decided under PDPA,
  not in this repository.]

## 3. Raw results (from the observer forms)

S = สำเร็จเอง (success alone), H = มีคนช่วย (finished with help). Time in seconds.
Errors and comments are copied from the forms; "-" means blank or "-" on the form.

### UAT01 — คัดกรองตั้งแต่ต้นจนจบ (Screening from start to finish)

| ID | Result | Time (s) | Errors | Observer comment |
|---|---|---|---|---|
| U01 | H | 80 | 1 | กรอกน้ำหนักผิด |
| U02 | H | 80 | 1 | กรอกน้ำหนักผิด |
| U03 | H | 120 | 3 | ไม่รู้ว่าต้องกดอะไรในหน้า "ปัจจัยเสี่ยง" |
| U04 | H | 100 | 2 | - |
| U05 | S | 100 | 2 | กดข้ามหน้าวัดความดันไปหนึ่งครั้ง และกรอกน้ำหนักผิดช่อง แล้วแก้เอง |
| U06 | S | 95 | 1 | กดข้ามหน้าวัดความดันหนึ่งครั้ง และกรอกน้ำหนักผิดช่อง แล้วแก้เองได้ทั้งสองจุด [CONFIRM: the comment describes two mistakes but the form says 1 error] |
| U07 | S | 100 | 1 | - |
| U08 | H | 100 | 2 | - |
| U09 | S | 100 | 2 | ช่วยเพิ่มคะแนนเต็มของการเก็บคะแนนความเสี่ยงในหน้าสุดท้ายของการคัดกรอง |

### UAT02 — ส่งต่อเคสเสี่ยงสูง (Refer a high-risk case)

| ID | Result | Time (s) | Errors | Observer comment |
|---|---|---|---|---|
| U01 | S | 5 | - | - |
| U02 | S | 5 | - | - |
| U03 | S | 10 | 1 | เห็นแจ้งเตือนสีแดงและเข้าใจทันที แต่หาปุ่มส่งต่อนานเล็กน้อย "ไม่คิดว่าปุ่มส่งต่ออยู่ข้างล่าง" |
| U04 | H | 10 | 2 | เห็นแจ้งเตือนสีแดงแต่ไม่แน่ใจว่าต้องทำอะไรต่อ ผู้สังเกตการณ์ชี้ปุ่มส่งต่อ "แล้วต้องกดอะไรต่อ" |
| U05 | S | 5 | 2 | - |
| U06 | S | 10 | 1 | - |
| U07 | S | 5 | 1 | - |
| U08 | H | 10 | 3 | อ่านข้อความแจ้งเตือนสีแดงแล้วไม่แน่ใจว่าต้องทำอะไรต่อ กดผิดปุ่ม 2 ครั้ง ผู้สังเกตการณ์ชี้ปุ่มส่งต่อ "ข้อความบอกว่าเสี่ยงสูง แล้วต้องกดอะไรต่อคะ" |
| U09 | H | 10 | 2 | - [reason for the help is not recorded] |

### UAT03 — เคสเสี่ยงต่ำและรายงานคำแนะนำ (Low-risk case and guidance)

| ID | Result | Time (s) | Errors | Observer comment |
|---|---|---|---|---|
| U01 | S | 15 | - | - |
| U02 | S | 15 | - | - |
| U03 | S | 30 | 0 | - |
| U04 | S | 35 | 0 | - |
| U05 | S | 20 | 0 | - |
| U06 | S | 20 | 0 | อ่านคำแนะนำคร่าวๆ "เขียว ใช้ยาต่อได้" |
| U07 | S | 15 | 0 | อ่านคำแนะนำผ่านๆ "ผลปกติ ไม่ต้องทำอะไรเพิ่ม" |
| U08 | S | 35 | 0 | - |
| U09 | S | 35 | 0 | - |

### UAT04 — เริ่มคัดกรองรายถัดไป (Start the next patient)

| ID | Result | Time (s) | Errors | Observer comment |
|---|---|---|---|---|
| U01 | S | 5 | - | - |
| U02 | S | 5 | - | - |
| U03 | S | 5 | 0 | กดเริ่มใหม่ได้เลย ข้อมูลคนเก่าไม่ค้าง "อันนี้ง่าย" |
| U04 | S | 10 | 0 | หาปุ่มเริ่มใหม่เจอเร็ว |
| U05 | S | 5 | 0 | - |
| U06 | S | 5 | 0 | - |
| U07 | S | 5 | 0 | - |
| U08 | S | 10 | 0 | - |
| U09 | S | 10 | 0 | - |

### ค้นหาผู้ป่วยในรายชื่อ (Search for a patient in the list)

On the forms this is **UAT05** for U03–U09 and **UAT06** for U01–U02.

| ID | Result | Time (s) | Errors | Observer comment |
|---|---|---|---|---|
| U01 | S | 5 | - | - |
| U02 | S | 5 | - | - |
| U03 | S | 5 | 1 | - |
| U04 | S | 10 | 0 | ใช้ช่องค้นหาได้ทันที "ง่ายดี" |
| U05 | S | 5 | 0 | ใช้ช่องค้นหาได้ทันที "ง่ายดี" |
| U06 | S | 10 | 0 | พิมพ์ชื่อในช่องค้นหา เจอรายชื่อเลย "หาง่ายดี" |
| U07 | S | 5 | 0 | ค้นหาเจอตั้งแต่ครั้งแรก "ใช้ง่าย" |
| U08 | S | 10 | 0 | อ่านชื่อในรายการเทียบกับที่ค้นหา ก่อนเลือก "ง่ายดี" |
| U09 | S | 10 | 0 | - |

### UAT05 (U01–U02 only) — แพทย์หาเคสและสั่งจ่ายยา (Doctor finds a case and orders a drug)

| ID | Result | Time (s) | Errors | Observer comment |
|---|---|---|---|---|
| U01 | H | 30 | 2 | ผู้ใช้งานไม่รู้ว่าต้องกดตรงไหน |
| U02 | H | 30 | 2 | ผู้ใช้งานไม่รู้ว่าต้องกดตรงไหน |

> **U01 and U02:** the two forms contain identical values in every cell because
> the two people took the test together and were observed as one pair. The values
> are therefore not independent. Section 4.1 counts them as two participants, as
> on the forms; Section 4.2 and Section 7 show the results without them or with
> the pair counted once.

## 4. Summary

### 4.1 All participants (U01–U09, as on the forms)

| Scenario | Success alone | Task Success Rate | Mean time (s) | Median time (s) | Mean errors |
|---|---|---|---|---|---|
| UAT01 Screening from start to finish | 4 / 9 | **44.4%** | 97.2 | 100 | 1.67 |
| UAT02 Refer a high-risk case | 6 / 9 | **66.7%** | 7.8 | 10 | 1.33 |
| UAT03 Low-risk case and guidance | 9 / 9 | 100% | 24.4 | 20 | 0 |
| UAT04 Start the next patient | 9 / 9 | 100% | 6.7 | 5 | 0 |
| Search for a patient (n=9) | 9 / 9 | 100% | 7.2 | 5 | 0.11 |
| Doctor-role task, UAT05 on the U01–U02 form (n=2) | 0 / 2 | 0% | 30 | 30 | 2 |

### 4.2 Group B only (U03–U09, PROXY, no demonstration first)

This is the cleaner measure of unaided use, because Group A saw a demonstration.

| Scenario | Success alone | Task Success Rate | Mean time (s) | Mean errors |
|---|---|---|---|---|
| UAT01 Screening from start to finish | 4 / 7 (U05, U06, U07, U09) | **57.1%** | 102.1 | 1.86 |
| UAT02 Refer a high-risk case | 4 / 7 (U03, U05, U06, U07) | **57.1%** | 8.6 | 1.71 |
| UAT03 Low-risk case and guidance | 7 / 7 | 100% | 27.1 | 0 |
| UAT04 Start the next patient | 7 / 7 | 100% | 7.1 | 0 |
| Search for a patient | 7 / 7 | 100% | 7.9 | 0.14 |

### 4.3 Group A only (U01–U02, saw a demonstration first, one joint session)

UAT01: 0/2 (help needed, wrong weight typed). UAT02, UAT03, UAT04 and search:
2/2 each. Doctor-role task: 0/2 (did not know which button to press).

## 5. Against the exit criteria (Test Plan Section 6.2)

| Criterion | Target | Result | Met? |
|---|---|---|---|
| Task Success Rate, per task | >= 70% | UAT01: 44.4% (all) / 57.1% (Group B). UAT02: 66.7% (all) / 57.1% (Group B). The others: 100% | **No (UAT01 and UAT02)** |
| UAT sessions completed | all planned | 9 held (7 planned) | Yes |
| SUS | >= 68 | 80.9 (SD 9.8; range 65.0–90.0), n = 8. Source: `sus_tam_results.md`, `sus_tam_responses.csv`. Two of 8 scored 70.0 and 65.0 | Yes |
| TAM | >= 3.5 / 5 | PU 4.17, PEOU 4.38, BI 4.21 (lowest single BI 2.67), n = 8. Same sources | Yes |
| Open Critical bugs | 0 | [CONFIRM from the issue log] | [CONFIRM] |

Questionnaires: SUS and TAM were answered in an online form filled in by each
participant after the test. The anonymised export is `sus_tam_responses.csv`
(responses R01–R08); the scores are in `sus_tam_results.md`. [CONFIRM: file
path of both files in the repo.]

Questionnaire coverage (checked against `sus_tam_responses.csv`):
- **8 responses** for 9 UAT participants, so 1 participant has no response
  [CONFIRM: who did not answer, and why]. 7 responses are dated 13 November and
  1 (R08) is dated 17 November [CONFIRM: whether R08 is U09, and which prototype
  version that person used].
- All 8 answered all 10 SUS items and all 9 TAM items (no blanks). Report SUS and
  TAM with n = 8, not n = 9. (An earlier version of this section said 7 responses
  and TAM n = 5; that was written before the export was checked and is replaced.)
- The export has no participant code, so responses cannot be matched to U01–U09.
  The CSV uses R01–R08 in sheet order. [CONFIRM: add the U-code to each row if
  the team can match it from the session records.]
- E-mail, names, job title (typed freely, not proof of the workplace) and
  language were removed from the export. The "job title" labels are not used;
  keep the labels from Section 1.
- One test-date cell held a wrong year (2569, a typing error) and is corrected to
  2026-11-13 for R07. [CONFIRM from the session record.]
- The export has no consent column. [CONFIRM: where PDPA consent is recorded.]
- The extra item about the buttons "ส่งเคสเข้าคิวแพทย์" and "ส่งแจ้งเตือนเข้ากลุ่ม
  LINE" (mean 3.62; R08 answered 2) matches the earlier flow, which supports the
  note in Section 6 that these sessions used the earlier version.
- SUS and TAM are self-reported and collected after the UAT. A high SUS does not
  mean every task was easy: 6 of 9 participants needed help in at least one task.

## 6. Issues found (for the issue log)

| # | Issue | Seen in | Suggested severity | Fix commit | Retest |
|---|---|---|---|---|---|
| 1 | Weight entered wrongly (wrong value or wrong field) during screening | U01, U02, U05, U06 | Medium | [CONFIRM] | [CONFIRM] |
| 2 | Unsure which button to press on the risk-factor page ("ปัจจัยเสี่ยง") | U03 | Medium | [CONFIRM] | [CONFIRM] |
| 3 | After the red high-risk alert, users did not know what to do or where the send button was | U03, U04, U08; U09 also needed help but the reason was not recorded | High | [CONFIRM] | [CONFIRM] |
| 4 | Blood-pressure page skipped once | U05, U06 | Low | [CONFIRM] | [CONFIRM] |
| 5 | Doctor-role task: did not know which button to press | U01, U02 | Medium (physician screens are mockups) | n/a | n/a |
| 6 | Advice read only briefly ("คร่าวๆ" / "ผ่านๆ"); the user may not separate a screening result from a diagnosis | U06, U07 | Low (observation only; not confirmed as a problem) | [CONFIRM] | [CONFIRM] |
| 7 | Suggestion: show the full (maximum) score next to the risk score on the last screening page ("ช่วยเพิ่มคะแนนเต็มของการเก็บคะแนนความเสี่ยงในหน้าสุดท้ายของการคัดกรอง") | U09 | Low (suggestion) | [CONFIRM] | [CONFIRM] |

Severity is the team's proposal; use the definitions in the Test Plan, Section 6.3.

Note on Issue 3: since Test Plan v1.3, an at-risk case goes to the case manager
queue automatically and the รพ.สต. screen has no "send to doctor" button.
The sessions U01–U08 were held on 13 November, before the 15–16 November
prototype update, so they were most likely run on the earlier flow (with a "send
to doctor" button). [CONFIRM: which version of the prototype was used, for U09 too.]
The final flow was not part of these sessions; say so, and do not present these
figures as results for the final flow.

## 7. Limitations (state these in the presentation)

- Only U01 and U02 are medical personnel, and they were observed together, so
  they are one joint session. The other 7 are PROXY (6 students and 1 lecturer).
- U01 and U02 saw a demonstration before they tried the system; Group B did
  not. The two groups should not be pooled without saying so. Group B alone
  (Section 4.2) gives UAT01 = 57.1% and UAT02 = 57.1%, both below the 70% target.
- Counting U01 and U02 as one pair (8 sessions) changes UAT01 to 4 of 8 (50%) and
  UAT02 to 5 of 8 (62.5%); the other scenarios stay at 100%. UAT02 is below 70%
  in every way of counting.
- Sample size is small (n=9), so percentages move by 11 points per person
  (14 points per person in Group B, n=7).
- The scenario names on the observer forms differ from the UAT01–UAT07
  definitions in Test Plan v1.3, Section 3.2 (the forms use an earlier set), and
  the UAT numbers differ between the U01–U02 form and the U03–U09 forms. Map or
  rename them before submission.
- The observer sometimes pointed at a button ("มีคนช่วย"), which the Test Plan
  procedure says should not happen. These cases are counted as failures.
- Times come from screen recordings, rounded to whole seconds (mostly steps of
  5 s), with a tolerance of +/- 8 s. Short tasks (5–10 s) are therefore within
  the error margin; use their times as an indication only. The screening time
  (about 100 s) is the only time that is meaningful at this tolerance.
- The physician and case-manager screens are mockups; only the doctor-role
  task for U01 and U02 was observed, not the case-manager role.
- Observer comments and quotes must match what the user said or did in the
  recordings. [CONFIRM: team has checked every comment against the recordings.]

## 8. Reconcile with the slides and the comparison table

Earlier figures used in the slides (success 100%, 7 of 7; median screening
1:14; mean 1:32 for n=5; 0 errors) do not match Section 4. The team must pick
one set, or explain how they were measured (for example, video review of five
sessions versus this observer form), and give the source file for every number
shown. A number without a source file earns a low or zero score.
