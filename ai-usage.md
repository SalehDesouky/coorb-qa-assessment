# AI Usage

I used Claude (Anthropic's AI assistant, in Claude Code) throughout this assessment, mainly to draft and format documents quickly. The decisions, the hands-on testing and the final choices were mine.

## Prompts I used

1. **Initial brief:** a detailed role prompt asking it to act as a senior test manager, analyse the attached PDF, and produce a complete response with requirement analysis, risk-based strategy, test design, metrics, release criteria and interview preparation.
2. **Scoping:** told it I am applying at Test Manager (Lead) level, so all five tasks apply, and pointed it to an older ParaBank automation project of mine to review for gaps.
3. **Formatting:** asked for each deliverable as a clean Word or Excel document instead of Markdown.
4. **Task 1:** "Is it better to use the ACs instead of risks for coverage, and show them in the traceability tab? You can still mention risks somewhere else."
5. **Task 2:** shared my ParaBank screenshots and asked whether "no validation on amounts" in Transfer and Bill Pay is one issue or two, then shared the account-opening overdraft I found and asked it to finalise the reports.
6. **Task 3:** asked for help when Postman would not run the imported collection.
7. **Task 4:** "Make it look simpler, follow the latest locator strategies, and make it easy to understand."
8. **Review:** asked it to review all five tasks as CoorB's hiring manager and testing director.
9. **Task 5:** "Consider I am applying to a QA manager position."

## What I kept, changed or rejected

| Task | What the AI did | What I decided |
|------|-----------------|----------------|
| Overall | Proposed following the brief's own format rather than the long governance report my first prompt asked for | **Kept.** The brief values focus over volume. |
| 1 | Drafted the questions, risk register and 12 test cases, with risks as the coverage view | **Changed.** I asked for acceptance criteria to be the primary traceability view. That surfaced six implicit requirements (e.g. no money lost after a failure) that no AC covers. Risks stay as a second matrix. After the review I added a tie-break rule, because five of the six risks scored "High". |
| 2 | Suggested areas to probe and how to capture evidence | **I ran the session myself** and found the negative transfer, the Bill Pay overdraft, the same-account transfer, and the overdraft through Open New Account. The AI helped me group findings by root cause (the account-opening overdraft sits under Bug 2, not as a third bug), check that the balance trail adds up, and write the reports. |
| 3 | Probed the live API before writing assertions, built the collection, and checked it against the live API | **Kept**, after debugging it in Postman. The imported URLs failed; my workaround (removing `https://` from the variable) made the run pass. The AI pointed out it was now running over plain HTTP. I agreed that isn't acceptable for a rates feed and switched to the HTTPS fix. The final review also noted that the negative cases were repeating for every data row, so they now run once. The final run: 85/85 passed. |
| 4 | Drafted a design with component and adapter layers | **Rejected as too complex.** I asked for a simpler design built on current Playwright locators (`getByRole`, `getByLabel`), with one small page object and a field list. After the review I made the locator steps follow the wrapper structure the brief describes, and added maker-checker approval and audit-log checks. |
| 5 | Drafted the go/no-go memo, then did a final pass that numbered the 72-hour priorities and kept it within 1.5 pages | **Kept the position** (not green; go only if the conditions are met), which matches how I would handle a "confirm QA is green" request. Because I am applying for a QA Manager role, I then asked for the management side to be explicit: team capacity, the cost of a delay against a failed launch, looking after the team during the crunch, and named owners for the follow-up changes. |

## What I would do differently next time

My first prompt was too broad. It asked for one large deliverable, and later I had to cut it back, simplify Task 4, and fix a Postman problem the AI's own check had missed. Next time I would use a tighter flow:

| Step | Who | Refined prompt / action |
|------|-----|-------------------------|
| 1. Read the brief myself | Me | List every requirement, the limits (12 cases, 1.5 pages, time boxes) and my own first view of the risks, **before** asking the AI anything, so I have something to check its output against. |
| 2. One prompt per task, with the constraints | AI | *"Here is Task 1 and its limits. First list only the ambiguities that would change the test design, each with a fallback assumption. Then rank risks by impact on money and regulatory controls. At most 12 test cases, each traced to an acceptance criterion or an implicit requirement."* |
| 3. Check behaviour before writing assertions | AI + me | *"Call the API with valid, unsupported, malformed and over-length codes and report what actually happens before writing any assertion. Then prove every assertion can fail."* Then **import and run it in the real tool myself**. A simulated check is not enough, as the Postman import issue showed. |
| 4. State the style up front | AI | *"Design only, no code. Use Playwright's recommended locators, one small page object and a field list. Explain the trade-offs."* This avoids an over-engineered first draft. |
| 5. Do the hands-on work myself | Me | Run the exploratory session myself, then ask the AI only to help group findings by root cause and check the evidence. |
| 6. Review against the brief | AI + me | *"Review all tasks as the hiring panel would, against the brief's eight assessment areas. What would they challenge?"* Then decide myself which points to act on. |

## Why I used it this way

AI was useful for speed: drafting, formatting, and checking API behaviour I could then verify. I did not rely on it for judgement: which coverage view matters, how simple the automation should be, whether a passing run over HTTP is acceptable, and what counts as a real defect were my calls. Everything in this repository is something I can walk through and defend.
