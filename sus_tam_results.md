# SUS / TAM Results — MRSS (MALA Risk Screening & Monitoring System)

**Version:** 1.0 (DRAFT, to be reviewed by the team before commit)
**Raw data:** [`sus_tam_responses.csv`](sus_tam_responses.csv) (anonymised export of the online form "Form Responses 1")
**Related:** [`UAT_Results_MediNova.md`](UAT_Results_MediNova.md), `Test_Plan_MediNova.md` (Section 5.2 SUS, 5.3 TAM, 6.2 targets)

> **AI Disclosure:** AI helped read the response sheet, compute the scores and
> format this file. The raw answers come from the form. The team must check the
> scores against the CSV before submission. Items marked **[CONFIRM]** must be
> checked by the team. Do not guess them.

## 1. Data and method

- **n = 8 responses** (7 dated 13 Nov 2026 and 1 dated 17 Nov 2026). All 8 answered
  all 10 SUS items and all 9 TAM items (no blanks).
- UAT sessions held: 9 (see the UAT Results). So **1 of 9 participants has no
  response** [CONFIRM: who did not answer, and why].
- The response sheet has **no participant code**, so a response cannot be matched
  to U01–U09. The CSV therefore uses response numbers R01–R08 (in sheet order).
  [CONFIRM: add the U-code to each row if the team can match it from the session
  records.]
- Removed from the export: e-mail, names, job title (typed freely, not proof of
  the workplace), language. The sheet had no timestamp column.
- One date cell held the year 2569 (Buddhist year, a typing error for 2026). It is
  corrected to 2026-11-13 in the CSV (response R07). [CONFIRM from the session record.]
- The sheet has no consent column. [CONFIRM: where PDPA consent is recorded.]
- The response dated 17 Nov (R08, job title "lecturer") was probably given after
  the prototype update; it is shown separately in Section 4. [CONFIRM.]

**SUS scoring (Brooke):** odd items (1, 3, 5, 7, 9) score = answer - 1; even items
(2, 4, 6, 8, 10) score = 5 - answer; sum x 2.5 gives 0–100. Benchmark: 68.

**TAM:** PU (3 items), PEOU (3 items) and BI (3 items) on a 1–5 scale. Each
participant's construct score is the mean of its 3 items; the table gives the mean
and the sample standard deviation across participants. No causal relationship
between constructs is claimed (Test Plan Section 5.3). Target: >= 3.5 / 5.

## 2. Results (all 8 responses)

| Measure | Mean | SD | Min – Max | Target | Met? |
|---|---|---|---|---|---|
| **SUS** (0–100) | **80.9** | 9.8 | 65.0 – 90.0 | >= 68 | Yes |
| **PU** Perceived Usefulness | **4.17** | 0.31 | 4.00 – 4.67 | >= 3.5 | Yes |
| **PEOU** Perceived Ease of Use | **4.38** | 0.45 | 4.00 – 5.00 | >= 3.5 | Yes |
| **BI** Behavioral Intention | **4.21** | 0.75 | 2.67 – 5.00 | >= 3.5 | Yes |

Only the 7 responses dated 13 Nov: SUS 80.4 (SD 10.5), PU 4.19, PEOU 4.43, BI 4.24.
The conclusion is the same.

SUS interpretation: 80.9 is above the benchmark of 68. Two of 8 participants
(R01: 70.0 and R02: 65.0) are near or below it, so the result is not uniform.

## 3. Per-response scores

| Response | Date | SUS | PU | PEOU | BI |
|---|---|---|---|---|---|
| R01 | 2026-11-13 | 70.0 | 4.00 | 4.00 | 4.00 |
| R02 | 2026-11-13 | 65.0 | 4.00 | 4.00 | 4.00 |
| R03 | 2026-11-13 | 90.0 | 4.67 | 5.00 | 4.33 |
| R04 | 2026-11-13 | 90.0 | 4.00 | 4.67 | 5.00 |
| R05 | 2026-11-13 | 90.0 | 4.00 | 4.33 | 2.67 |
| R06 | 2026-11-13 | 82.5 | 4.67 | 5.00 | 5.00 |
| R07 | 2026-11-13 (corrected) | 75.0 | 4.00 | 4.00 | 4.67 |
| R08 | 2026-11-17 | 85.0 | 4.00 | 4.00 | 4.00 |

R05 has a high SUS (90.0) but the lowest BI (2.67); the form has no comment that
explains it. This is the main reason the BI standard deviation is the largest.

## 4. Extra items on the form (1–5, agree)

| Item | Mean (all 8) | Mean (13 Nov, n=7) | R08 (17 Nov) |
|---|---|---|---|
| Understands the risk score and threshold line and what contributed to the score | 4.00 | 4.29 | 2 |
| System advice (Sick Day Rule, no alcohol, warning signs) is easy to understand and can be told to patients | 4.25 | 4.57 | 2 |
| Understands the difference between the "send case to doctor queue" and "send LINE alert" buttons, and is sure the case reached the doctor | 3.62 | 3.86 | 2 |

- R08 rated all three items 2, while rating SUS 85.0. The form has no comment from
  R08, so the reason is unknown. [CONFIRM with the team; ask the participant if possible.]
- The third item refers to the earlier flow (since Test Plan v1.3 the รพ.สต. screen
  has no "send to doctor" button and no LINE button). Its result applies to the
  earlier version, not the final flow.

## 5. Free-text comments (as typed)

- Liked most: "ใช้งานง่าย" (3 responses), "Ui เข้าใจง่าย", "คำแนะนำ" (1 each).
- Most confusing: one response said the scan step may confuse users if nobody
  teaches them first ("ตรงแสกนน่าจะสับสนนิดหน่อยถ้าไม่มีคนมาสอนใช้งาน"). This agrees
  with the UAT finding that help was needed in some tasks.
- No other comments were given.

## 6. Limitations

- n = 8 is small; one participant moves the mean by about 1.2 SUS points on average
  and the SD values are rough.
- Most participants are PROXY users (students and one lecturer), not รพ.สต. staff
  (see the UAT Results, Section 1). Their job title on the form was typed freely.
- SUS and TAM were collected after the UAT. At least 6 of 9 participants had
  difficulty in at least one UAT task, so the high SUS does not mean every task was
  easy. Report this together with the UAT success rates (UAT01 44%, UAT02 67%).
- U01 and U02 were observed together and saw a demonstration first; the response
  sheet cannot show whether they answered separately. [CONFIRM.]
- The response is self-reported. Do not claim it proves clinical benefit.
