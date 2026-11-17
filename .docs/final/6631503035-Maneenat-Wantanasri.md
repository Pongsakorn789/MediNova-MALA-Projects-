# Role Statement: Maneenat Wantanasri (6631503035), UX/UI Designer

**Team:** MediNova · **Project:** MALA Risk Screening & Monitoring System (MRSS) · **Live URL:** https://mala-project-2026.web.app

## What I owned

As UX/UI Designer, I owned how PCU Staff (รพ.สต.) move through the screening on a phone. The flow has five steps (patient, weight, height, risk factors, review and save), one question group per screen, large buttons for one-handed use, and a numeric keypad for weight and height. I designed the screens that show the Part 1 dose result, the Part 2 score with its reason, the advice card with the Sick Day Rule, and the queue banner after saving.

The UAT showed where the design needs work: weight entered in the wrong field, an unclear "ปัจจัยเสี่ยง" page, and users unsure what to do after the red high-risk alert. These cases come from the earlier flow, so the final flow still needs a retest. I did not change the code myself; these findings are recorded in the UAT Results (Section 6) for the team to fix.

## Evidence

- [Feature List](https://github.com/Pongsakorn789/MediNova-MALA-Projects-/blob/main/.docs/02-design/feature-list.md): features of the screening flow and the case manager screens (3 commits).
- [User Journeys](https://github.com/Pongsakorn789/MediNova-MALA-Projects-/blob/main/.docs/02-design/user-journey.md): screening and case-routing workflows for PCU staff (3 commits).
- [Rules](https://github.com/Pongsakorn789/MediNova-MALA-Projects-/blob/main/.docs/rule.md): legal notes and test-patient guidelines (3 commits).
- [Test Plan](https://github.com/Pongsakorn789/MediNova-MALA-Projects-/blob/main/Test_Plan_MediNova.md): I created version 1.0 on 28 October (commit `ffb9fa5`) and updated it to v1.3 (6 commits).
- [README](https://github.com/Pongsakorn789/MediNova-MALA-Projects-/blob/main/README.md): project overview, updated with the Test Plan v1.3 and the sources (3 commits).
- [Working prototype](https://mala-project-2026.web.app): the final system after Feedback round 2.
- [Figma design](https://www.figma.com/design/zoZGhvUfplE9LOq8hVHxZo/MALA?node-id=0-1&p=f&t=W5tSUzLDkRsCJrem-0): the first version of the screens, made before Feedback round 1.

## AI Disclosure

We used Stitch to design the screens, Gemini to write prompts, Claude in VS Code to write the code, and Claude to help with the wording of this statement, the slides and the documents. I checked every AI output against the live system and the repository files before using it.
