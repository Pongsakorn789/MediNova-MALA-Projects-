# Role Statement: Thanakit Chupviroj (6631503020), AI Lead

**Team:** MediNova · **Project:** MALA Risk Screening & Monitoring System (MRSS) · **Live URL:** https://mala-project-2026.web.app

## What I owned

As AI Lead, I owned the screening logic behind MRSS. The system uses fixed rules and no trained AI model, and my job was to define those rules clearly and keep them traceable to the code.

**Part 1, dose check.** The rule follows the CPG table. At eGFR below 30 the status is contraindicated, and the screen tells staff to stop Metformin and inform the doctor. At eGFR 30–44 a dose of 1,000 mg/day or less passes. At eGFR 45 or higher a dose of 2,000 mg/day or less passes. Any higher dose goes to the doctor to review. The system records and forwards the case only; it never orders a dose change in place of the doctor.

**Part 2, MALA score.** The score adds eGFR below 60 (1 point), BMI below 23 (2 points) and heavy alcohol use (2 points). A score of 2 or more means the patient is at risk, and the case goes to the case manager queue.

**Advice.** The backend in `backend/main.py` checks each condition and returns the matching advice text: eGFR below 45, heavy drinking, vomiting or diarrhea, fasting, painkillers and herbal products. The endpoint is still named `/api/evaluate-mala-ai`, but it returns `guidanceType: "rule-based"`.

**Limits I documented.** The rules need review by a clinical owner before real patient use. The output is screening support and not a diagnosis. The API receives `age` and `metforminDose`, but the advice rules do not use them. Two points remain open: the code scores 2 points for either kind of heavy drinking, which the mentor must confirm, and the code still returns `Yellow` and `Green` levels that the final flow does not show.

**Testing.** The Test Plan v1.3 (Section 6.1) assigned the prototype update and the unit and integration tests of the dose check, the score and the routing to the Product Owner and me.

## Evidence

- Logic description: [`ai-logic.md`](ai-logic.md)
- Code: `src/utils/metforminGuideline.ts`, `src/utils/riskEngine.ts`, `backend/main.py` 
- Test cases and role table: `Test_Plan_MediNova.md` (v1.3)
- Working prototype: https://mala-project-2026.web.app

## AI Disclosure

AI (Claude) helped draft this statement from `ai-logic.md` and the Test Plan. I must check every claim against the repository and commit history before submission.
