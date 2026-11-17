# Role Statement: Thanakit Chupviroj (6631503020), AI Lead

**Team:** MediNova · **Project:** MALA Risk Screening & Monitoring System (MRSS) · **Live URL:** https://mala-project-2026.web.app

## What I owned

As AI Lead, I owned the screening logic behind MRSS. This covers the Part 1 dose check by eGFR (CPG table), the Part 2 MALA score (eGFR < 60 = 1, BMI < 23 = 2, heavy alcohol = 2; at risk when the score is 2 or more), and the rule-based advice that the backend returns for the result screen.

I wrote down that this logic is fixed rules and not a trained model, so the team's AI disclosures stay accurate. The same document states its limits: the rules need clinical review before real patient use, the output is not a diagnosis, and the backend does not use `age` or `metforminDose` to choose advice.

With the Product Owner, I shared the work of updating the prototype to the final flow and checking the unit and integration tests for the dose check, the score and the routing (Test Plan v1.3, Section 6.1). [CONFIRM: which of these tasks I completed myself.]

## Evidence

- Logic description: [`ai-logic.md`](ai-logic.md) [CONFIRM: path in repo]
- Code: `src/utils/metforminGuideline.ts`, `src/utils/riskEngine.ts`, `backend/main.py` [CONFIRM: add commit links]
- Test cases and role table: `Test_Plan_MediNova.md` (v1.3)
- Working prototype: https://mala-project-2026.web.app

## AI Disclosure

AI (Claude) helped draft this statement from `ai-logic.md` and the Test Plan. I must check every claim against the repository and commit history before submission. Items marked **[CONFIRM]** are not verified.
