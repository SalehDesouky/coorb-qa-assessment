# QA Technical Assessment – Saleh Desouky

Submission at **Lead** level (QA Manager role), so all five tasks are included.

## Contents

| Task | File(s) |
|------|---------|
| 1 – Test Design & Risk Coverage | [Task1_Test_Design_and_Risk.xlsx](task-1/Task1_Test_Design_and_Risk.xlsx): questions, requirements (acceptance criteria plus implicit requirements), risk register, 12 test cases, requirements and risk traceability, coverage decisions |
| 2 – Exploratory Testing & Bug Reporting | [Task2_Exploratory_Testing_and_Bug_Reports.docx](task-2/Task2_Exploratory_Testing_and_Bug_Reports.docx), screenshots in `task-2/evidence/` |
| 3 – API Testing | [fx-rates.postman_collection.json](task-3/fx-rates.postman_collection.json), [fx-rates.postman_environment.json](task-3/fx-rates.postman_environment.json), [base-currencies.csv](task-3/base-currencies.csv), [Task3_API_Testing.docx](task-3/Task3_API_Testing.docx) (Part A notes and Part B answers), run results in `task-3/evidence/` |
| 4 – Automation Design | [Task4_Automation_Design.docx](task-4/Task4_Automation_Design.docx) |
| 5 – Quality Strategy & Release Judgement | [Task5_Release_Memo.docx](task-5/Task5_Release_Memo.docx) |
| AI usage | [ai-usage.md](ai-usage.md) |

## Time spent

| Task | Time |
|------|------|
| Task 1 | 35 mins |
| Task 2 | 20 mins |
| Task 3 | 30 mins |
| Task 4 | 40 mins |
| Task 5 | 30 mins |
| Packaging (repository, evidence, README) | 30 mins |

## Assumptions

- **Task 1:** every question in sheet *1.1 Questions* has a fallback assumption I tested against. The ones that shape the test cases most: limits are inclusive, the daily limit is a calendar day in the bank's time zone, the fee is included in the balance check but not in the limit, and a failed transfer must never lose money.
- **Task 3:** I used `open.er-api.com`. The backup (`api.frankfurter.app`) returned an HTTP 301 redirect when I tried it. The response-time (2,000 ms) and rate-age (26 h) thresholds are provisional and should come from the app's own requirements.
- **Task 4:** `Bank_Admin_Console_Demo.html` was not attached to the email I received. I have asked for it and designed from the written description in the brief. If the real page differs, only the field catalog changes.
- **Task 5:** the total number of regression tests isn't given, so I haven't inferred one from the 68% pass rate.

## Questions I would ask the BA / PO

The full list, with reasons, is in sheet *1.1 Questions* of the Task 1 workbook. These change the design most:

1. When a transfer fails after the debit, what happens to the money and what does the customer see? AC6 is too vague for a money feature.
2. Are the limits inclusive, and what exactly is the "day" for the daily limit?
3. Is the fee included in the balance check, and is it shown before OTP?
4. Is the purpose an optional free-text note (Task 1, AC1) or a required code (Task 3 API)? The two contradict each other.
5. Payments API: if a request returned 402 and is retried with the same idempotency key, should it return the stored result or re-check the balance?
6. Operations console: is there an isolated tenant or a config API for testing, given the environment is shared and cannot be reset?

## Deliberately left out

- **Task 1:** beneficiary registration, performance and load, look and feel / accessibility / devices. Reasons are in sheet *1.4 Coverage*.
- **Task 2:** no security or load testing on the demo site, as instructed.
- **Task 4:** no implementation beyond short illustrative snippets, as instructed.

## Running the Task 3 collection

In Postman: Runner → select the collection, the environment and `base-currencies.csv` as the data file. From the command line:

```
newman run task-3/fx-rates.postman_collection.json -e task-3/fx-rates.postman_environment.json -d task-3/base-currencies.csv
```
