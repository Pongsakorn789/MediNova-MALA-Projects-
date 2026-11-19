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

AI (Claude) helped draft this statement and the Reflection from `ai-logic.md`, the Test Plan and our project files. I must check every claim against the repository and commit history before submission.

## Reflection

**1. Lessons learned.** Building MRSS with AI changed every phase of software engineering, and not always in the same direction.

*Requirements.* AI helped turn the mentor's rules into user stories with acceptance criteria, so each rule had a clear "done" condition. But AI cannot settle a rule that the mentor has not settled: the definition of heavy alcohol use is still open, and the code scores 2 points for either kind of heavy drinking.

*Design and coding.* The five-step mobile flow and the rule files (`metforminGuideline.ts`, `riskEngine.ts`, `backend/main.py`) were quick to produce. The UAT then showed what a quick design misses: weight typed in the wrong field, an unclear risk-factor page, and users who did not know what to do after the red alert. In the code, the endpoint is still named `/api/evaluate-mala-ai` and it receives `age` and `metforminDose` that the advice rules do not use.

*Testing.* *Better:* testing became more traceable. We wrote 36 test cases and 7 UAT scenarios, and each dose-check and score rule could be tied to a test, so I could say exactly what was checked.

*Quality and security.* *Worse:* consistency and quality control. Our documents drifted apart. The early overview said eGFR below 45 fails the dose check, but the Test Plan and the code follow the CPG table, where eGFR 30–44 passes up to 1,000 mg/day. A cost note about "AI tokens" and the word "ai" in the endpoint suggested a model, yet the system is rule-based. Some slide figures did not match the UAT results. AI writes confident text, so every number and rule needs a source and a human check. For a health tool, privacy also needs design from the start: our prototype still has no consent step, and we used invented patients only.

*Teamwork.* Each member owned a role, and AI helped each of us write. The lesson was to mark what is not verified. We used [CONFIRM] notes so that no one submitted a guess as a fact.

**2. Where I stand.** I am a student who wants to work as a Business Analyst, mostly on UAT and test cases. In the AI era I can build and document a working prototype much faster than before, but AI cannot decide whether a rule is clinically correct or whether users can really finish a task. Our UAT showed this: two scenarios missed the 70% target even though the SUS score was high. That is the work I want to do: write the test cases, run the UAT, and report the results honestly. My gap is domain and security knowledge.

**3. Plan.** *Next year:* I will do a Business Analyst internship to learn the work in a real team. After I graduate, I plan to work in the private sector for about 2–3 years to build experience, and during that time I will prepare for the civil service entrance exam. *In 5 years:* I want to be a computer technical officer (นักวิชาการคอมพิวเตอร์) at the Ministry of Public Health. I chose this path because I like both healthcare and computers, and this job lets me combine them to build systems that help patients and health staff.
