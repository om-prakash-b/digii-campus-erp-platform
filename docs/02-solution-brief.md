# Solution brief: Digii Exam Registration & Eligibility

## 1. What it is

A separate, configurable Digii module that decides who is writing which paper for each exam cycle. It gives every student a clear status, handles rule changes mid-cycle with a visible impact preview and audit trail, and hands a final locked list to hall tickets, seating and question-paper printing at least three weeks before exams.

## 2. Capabilities

### C1. Cycle setup

- Create an exam cycle (e.g. "Dec end-semester") and choose the programs and semesters it covers.
- A setup wizard with sensible defaults; required fields are flagged.
- Phases: Setup, Form open, Form closed, Review, Locked and published, Exams, Closed.

### C2. Exam registration (one record per student per paper)

- Regular papers are pulled from course registration.
- Backlog papers are pulled automatically from Results (failed or absent papers).
- Improvement papers come from Results (passed papers) if the university allows improvement.
- Students can opt out of a backlog paper and carry it to a later cycle.
- Registrations are not copied from course registration as-is. Each one carries its own status and reason.

### C3. Eligibility checks

- A configurable list of checks per student: attendance, exam fee, tuition dues, disciplinary hold.
- Each check returns Pass, Fail, Pending or Waived, with the value used, the rule version and the reason.
- Live, provisional attendance from the day the form opens, with a projection of whether the student is on track. It becomes final at the cut-off.
- Condonation: a student who falls short but is within a program's condonation range is offered the condonation route (fee plus approval) instead of a straight fail.

### C4. Student status and exam form

- One status page answering three questions: where do I stand, why, and what do I do next.
- The exam form: confirm papers, opt in or out of backlog and improvement papers, see the fee breakdown, pay.
- A "What changed for you" notice after every change that affects the student. This replaces WhatsApp as the official channel; other channels are simulated in the prototype.
- Hall ticket download once the list is locked.

### C5. Fees and money ledger

- Fee is calculated per student: semester fee plus backlog or improvement papers plus late fee plus condonation fee.
- Payment goes through Digii's existing gateway and is matched to the student automatically; no Excel.
- Payment start time and bank confirmation time are both recorded. The start time decides whether the payment met the deadline.
- Fee fixed at payment: a later fee increase does not change what has already been paid.
- A ledger for each student: amount due, paid, balance owed, refund due, credit. It updates automatically after every change.

### C6. Change management

Every change to a rule, date or piece of data follows the same flow:

Edit, then impact preview, then reason or circular reference, then approve, then new settings version, then recalculate, then notify affected students, then audit log.

- The impact preview shows, before confirming, which students change status, fee or papers, and the money effect.
- Settings are versioned: old versions are kept, and each student's result records which version applied.
- Changes are restricted by phase (see the settings table).

### C7. Lock, publish and outputs

On the lock date the final list is frozen and published.

Outputs:

1. Final list: student by paper by type by hall ticket number
2. Headcount per paper, for printing and seating
3. Exceptions list: blocked students with reasons, condonation granted
4. Money list: balances owed and refunds due

After lock: changes are made only as approved amendments. Each amendment creates a new hall ticket version (the old one is marked superseded) and reports the change in headcount to seating and printing.

### C8. Admin dashboard and audit

- Counts and lists: eligible, at risk, blocked, unpaid, pending condonation, refunds due.
- A per-student view: checks, ledger, history.
- An audit log of every change: who, when, why, before and after, students affected.

## 3. Settings

Settings change by phase. "Before open" means freely editable during Setup. "Live" means changeable during Form open and Review, always through the impact preview. "After lock" means changeable only as an approved amendment.

| Group | Setting | Default | Owner | Can it change once live? |
|---|---|---|---|---|
| Cycle | Programs and semesters in the cycle | All active | CoE | Add only, until the form closes |
| | Exam start date | Required | CoE | Yes, with preview; dates that depend on it are recalculated |
| Dates | Form open date | Required | CoE | No, once open |
| | Form deadline | Exam start minus 28 days | CoE | Extend: yes. Shorten: yes, but students already in progress keep the original date |
| | Late window and late fee | 5 days; fee required if a window is set | CoE + Accounts | Yes; applies only to students who pay after the deadline they were held to |
| | Attendance cut-off | Lock date minus 1 day | CoE (Dean can request) | Yes, until lock, with preview |
| | Final list lock and publish date | Exam start minus 21 days | CoE | Earlier: no. Later: yes, with preview |
| Fees | Semester exam fee | Required | Accounts | Increase: yes, applies to new payments only |
| | Backlog / improvement fee per paper | Required | Accounts | Same as above |
| | Condonation fee | Required if condonation is on | Accounts | Same as above |
| | Fee fixed at | Time of payment | Accounts | No, once live |
| | Payment counted at | Payment start time | Accounts | No, once live |
| | Refund if the student becomes ineligible | Full refund of exam fee (option: credit to next cycle) | Accounts | No, once live |
| Attendance | Threshold | 75% | CoE | Yes, with preview |
| | Basis | Overall (option: per course) | CoE | No, once live |
| | Rounding | None: compared at 1 decimal place | CoE | No, once live |
| | Corrections after cut-off | Allowed until lock with Dean approval; after lock as an amendment | CoE | No, once live |
| Checks | Exam fee paid | Blocks the exam | CoE | No, once live |
| | Tuition dues | Warn only (option: block) | CoE + Accounts | Yes, with preview |
| | Disciplinary hold | Blocks the exam | CoE | No, once live |
| Exceptions | Condonation (per program: percentage range, fee, approval needed) | Off | CoE | Yes; can be added mid-cycle with preview |
| | Improvement attempts (allowed, maximum papers) | Off | CoE | Before form closes only |
| | Opting out of a backlog paper | Allowed | CoE | Before form closes only |

## 4. Change-handling rules

The same rules apply to every event:

1. Students: each student is held to the most favourable deadline they were shown when they started the form. A shortened deadline never penalises someone already in progress.
2. Money already paid: what was paid stands at the rate in force at the time. Increases apply only to new payments. If a student becomes ineligible, a refund or credit is created automatically. New charges (late fee, condonation fee) appear as a balance owed.
3. Already published: published items are never edited silently. Every change after lock is an amendment with a new version, a notice to the student, and the headcount change sent to seating and printing.
4. Attendance: provisional until the cut-off and final at the cut-off. Corrections to earlier attendance need approval, and after lock they become amendments.
5. Admin: sees the impact preview before every change; every change is recorded in the audit log.

The change-handling table in [07-change-handling-edge-cases-scope.md](./07-change-handling-edge-cases-scope.md) applies these rules to the actual timeline of events.

## 5. Journeys

Student:

1. Open the status page.
2. See papers (regular, backlog, improvement), checks and fee.
3. Opt in or out of papers.
4. Pay.
5. Track status, apply for condonation if offered.
6. Read "What changed for you" notices.
7. Download the hall ticket after lock.

Admin (CoE / Accounts / Dean):

1. Create the cycle and complete the setup wizard.
2. Open the form.
3. Monitor the dashboard.
4. Apply changes through the impact preview.
5. Review attendance corrections and condonation.
6. Lock and publish.
7. Hand over the final list and headcounts.
8. Handle amendments; check the audit log.

## 6. Root-cause coverage

| Root cause | Fixed by |
|---|---|
| R1 Hall ticket copied from course registration | C2, C7 |
| R2 Backlog and improvement papers not in the system | C2 |
| R3 Eligibility checked last | C3, C4 |
| R4 Payment not linked to the form; Excel matching | C5 |
| R5 No status page for students | C4 |
| R6 Rule changes not versioned and impact not previewed | C6 and the change-handling rules above |
| R7 Attendance figure never fixed | C3, attendance settings |
| R8 No rules for exceptions | C3, exception settings, C5 |
| R9 No lock date or post-publication process | C7 |
