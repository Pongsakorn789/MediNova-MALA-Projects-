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

## Reflection

### 1. Lessons Learned: AI in software engineering

We gave the hospital's requirements to AI to generate screen images and then code. The staff could try real screens instead of reading a description, so they told us at once what was right or wrong, and we changed the system the same day.

The worse side is that AI does not know how the real system works. Its mockups and documents sometimes used old terms that did not match the live website, so I checked them one by one. When a requirement changed (dose check first, new case manager role), I had to update every place.

The lesson: AI is good for fast first drafts, but requirements must come from real users, and a person must check the result.

### 2. My Position in the Industry

I am a beginner who uses AI as a tool. In this project I handled UX/UI and requirements: collecting them from the hospital, designing in Figma, and passing them to the Product Owner to build with AI. AI can do repeated work, but listening to users, deciding what matters at the workplace, and checking safety still need a person. I want to develop these skills.

### 3. My Plan

I enjoyed finding what the staff really needed more than detailing screens, so I am interested in becoming a Business Analyst (BA).

- **Next year:** I plan to work in a BA role or as a Technical Readiness Coordinator, practise user interviews and user stories, and use MALA in my portfolio.
