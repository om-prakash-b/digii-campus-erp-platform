# Digii Exam Registration & Eligibility

A product case study and working prototype for Digii, a campus ERP platform: redesigning how it handles exam registration and eligibility.

A university's exam process broke down under rule changes made mid-cycle: deadlines shifted, fees changed, attendance cut-offs moved, and nobody could say who was actually eligible to write which paper until exam day. This project diagnoses why, then designs and builds the fix as a working prototype with an admin side and a student side, starting from an empty setup, using twelve sample students, with a simulated clock so you can walk the timeline day by day.

**Live demo:** https://om-prakash-b.github.io/digii-campus-erp-platform/

## The case study

- [Situation and root causes](./docs/01-situation-and-root-causes.md)
- [Solution brief](./docs/02-solution-brief.md)
- [Assumptions](./docs/03-assumptions.md)
- [User flows](./docs/04-user-flows.md)
- [Changes to existing Digii modules](./docs/05-existing-module-changes.md)
- [Metrics](./docs/06-metrics.md)
- [Change handling, edge cases, scope and go-live](./docs/07-change-handling-edge-cases-scope.md)

## Run it

- **Locally:** open `index.html` in Chrome or Edge. There's no install or build step.
- **Hosted:** this folder is deployed as-is via GitHub Pages; it also works on any static host such as Netlify or Vercel.
- State is saved in the browser's local storage. Use **Restart** to begin again from an empty setup.

## Suggested walkthrough

1. **Admin, Setup:** create the cycle. Optionally use "Fill with sample values," then review all 7 steps and create.
2. **Jump to 5 Nov.** Switch to the **Student** view and pick S01, S02, S06 and S07. Each sees their status and reasons, and can pay. S07 can untick BB108.
3. **Admin, Changes:** each timeline event has a **Prepare** button. Click it, then **Preview impact** to see before and after for all 12 students, then confirm with a reason.
4. After each change, open S02, S03, S04, S05, S08, S09 and S12. Each one's **"What changed for you"** panel explains the effect on them.
5. **Admin, Audit log:** every change records what the admin saw before confirming, the reason and circular reference, and the new settings version.
6. **Jump to 6 Dec** and apply S09's correction. Since the list is already locked, the change becomes a numbered **amendment**.
7. **Admin, Final list** shows four outputs:
   - the hall-ticket list (student by paper)
   - the headcount per paper, compared with the old course-registration count
   - students not on the list, with reasons
   - money owed and refunds due

   Each one can be downloaded as CSV.

## How it is built

| File | Role | In production this would be |
|---|---|---|
| `js/data.js` | Sample data for the twelve students, course catalogue, timeline of events | Read APIs of the Course registration, Results, Attendance and Fees modules |
| `js/engine.js` | **Pure rules engine**: computes every student's eligibility, fees, refunds, windows and paper status from the data plus the settings in force at a given time. It also previews and applies changes and handles lock and amendments. | The Exam Registration service (domain layer) |
| `js/store.js` | Commands: simulated clock, sample-data student actions, undo, persistence | API and app layer, plus the job scheduler |
| `js/ui.js` | Admin and student views | Web front end |
| `tests/scenario.test.js` | Replays the whole walkthrough and checks 22 outcomes | CI test suite |

No outcome is hard-coded. Each one comes from `evaluateStudent(state, student, now)`. Run the tests with `node tests/scenario.test.js`.

### Suggested API (how the engine would be exposed)

| Endpoint | Engine function |
|---|---|
| `POST /exam-cycles` | `createCycle` (validates settings, creates settings v1) |
| `GET /exam-cycles/{id}/students/{sid}/status` | `evaluateStudent` |
| `PUT /exam-cycles/{id}/students/{sid}/registration` | `submitForm` (paper choices) |
| `POST /exam-cycles/{id}/students/{sid}/payments` | `pay` (idempotency key = gateway order id; stores start and confirmation times) |
| `POST /exam-cycles/{id}/students/{sid}/condonation` | `applyCondonation` |
| `POST /exam-cycles/{id}/changes:preview` | `previewChange` (read-only; returns before/after per student) |
| `POST /exam-cycles/{id}/changes` | `commitChange` (requires a reason; creates a settings version, notices and audit entry) |
| `POST /exam-cycles/{id}/condonation/{sid}:decide` | `decideCondonation` |
| `POST /exam-cycles/{id}/final-list:lock` | `lock` |
| `GET /exam-cycles/{id}/final-list` | `outputs` (hall tickets, headcount per paper, exceptions, money) |
| `GET /exam-cycles/{id}/audit` | audit log |

Design principles behind this API:

- Changes are always **previewed, then committed**.
- Settings are **versioned and append-only**. Nothing is overwritten.
- Money already paid is fixed at the rate that applied when it was paid.
- After lock, the published list changes only through **numbered amendments**.
- Every write produces an **audit event**.

## What is simulated

- **Payments:** a Pay button. You choose when the bank confirms: instantly, after 5 minutes or after 2 hours.
- **Notifications:** in-app "What changed for you" notices only.
- **Login and roles:** there is one admin view, with each setting's and change's owner shown. Dean approval of a correction is a checkbox.
- **Data from other modules:** attendance, results and dues come from the sample data. Attendance between the three given dates is interpolated. Corrections shift that period and every later one.
- **Sample-data student actions** play automatically on their dates: S03 pays on 3 Nov, S12 starts the form on 11 Nov, S07 pays on 12 Nov, and S08 pays at 11:58 PM on 20 Nov. You can switch this off on the Dashboard.
- **Clock:** "today" is simulated and time only moves forward. Undo steps back through actions.
