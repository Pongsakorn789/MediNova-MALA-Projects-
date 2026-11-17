# Role Statement — Pongsakorn Chaikittiporn

**Student ID:** 6631503114  
**Role:** Product Owner

## What I owned

As Product Owner, I defined and refined the MALA workflow and also personally implemented the code across every page and feature in the web application. My work covered the patient screening flow, dose assessment, risk scoring, result and review screens, patient search and Excel import, doctor views, case-manager views, reporting, login and role navigation, and browser-side data storage. The source files for the full frontend are in `mala-web/src/`, and the API code is in `mala-web/backend/`.

I set the screening sequence so the system checks the patient's Metformin dose against eGFR first; when the dose is within the project's criteria, staff continue to the MALA risk score and triggering-factor questions. I implemented the intended routing: dose-review cases go to the clinician, while cases meeting the MALA risk-score threshold go to the case manager. During development I also added patient history details for the case manager, per-row Excel area import with a file-level fallback, a larger patient search list, and score display as a score out of five with the criteria shown. The repository history records implementation commits under the account name Pongsakorn. I have not claimed user interviews because the reviewed materials do not establish that I conducted them.

## Evidence

- [Product Backlog](../../01-requirements/backlog.md) — priorities and acceptance criteria used to define scope.
- [User Journeys](../../02-design/user-journey.md) — intended screening and case-routing workflows.
- [Screening flow](../../../mala-web/src/modules/vhv/VhvScreen.tsx) — dose-first sequencing and risk-case routing.
- [Risk scoring](../../../mala-web/src/utils/riskEngine.ts) — Part 2 criteria and threshold.
- [Case manager screen](../../../mala-web/src/modules/care-manager/CareManagerScreen.tsx) — case queue and patient history details.
- [Complete frontend source](../../../mala-web/src/) — pages and features I implemented across the application.
- [Backend source](../../../mala-web/backend/) — API implementation.
- [GitHub history](https://github.com/Pongsakorn789/MALA_WEB/commits/main/) — commits under Pongsakorn; commit list is implementation evidence, not proof of interviews or team decisions.
- [Case Study](../../../Final/case-study.md) — project process and reported test outcomes.

## AI disclosure

I used OpenAI Codex as a coding and writing assistant to help implement interface and workflow changes and draft project documentation. I reviewed suggested changes against the requested screening flow and the repository code, and checked the frontend build during development. The MALA scoring and advice in the application are implemented as explicit rules; Codex did not train a clinical prediction model. Clinical criteria still require review by the project's clinical supervisor before use in patient care.
