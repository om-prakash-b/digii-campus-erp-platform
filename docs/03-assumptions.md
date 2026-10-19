# Assumptions

Where something was not fully specified, I made an assumption, wrote it down, and moved on. Each one below names the fact or gap it rests on.

## A. Digii's platform

Digii's internals aren't public, so these assumptions describe what I took the existing platform to already provide.

| # | Assumption | Based on | Affects |
|---|---|---|---|
| A1 | The Results module holds each student's outcome per paper (pass, fail or absent, plus grade) for past semesters, so backlog and improvement papers can be found automatically | Digii already runs results; backlog courses aren't re-registered automatically | S02, S06, S07, S10 |
| A2 | The Attendance module can return a percentage for any date range and records when each entry was edited | Attendance is described as editable later and reportable over any date range; edit history isn't mentioned | S09, cut-off moves |
| A3 | The payment gateway records both the payment start time and the bank confirmation time against the student ID | A student paid at 11:58 PM with confirmation arriving after midnight | S08 |
| A4 | The Fees module can raise charges and record refunds and credits | Digii already handles fee heads and dues tracking | All payments |
| A5 | Hall tickets, seating and printing can take the final list from the new module instead of course registration | Needed for the final output to work | Final output |
| A6 | Digii has no disciplinary module, so the CoE enters a debarment as a hold | Sample data, S11 | S11 |

## B. Rules and policy not otherwise specified

| # | Assumption | Based on | Affects |
|---|---|---|---|
| B1 | Attendance is calculated overall, not per course | Taken as the default unless there's a reason to argue otherwise | All students |
| B2 | No rounding: 74.6% is below 75% | S09 is the test case for this | S09 |
| B3 | Attendance is the percentage from semester start up to the cut-off date in force | The sample data | All students |
| B4 | Condonation range is 65% or above and below 75%, for the listed programs only, with a fee and CoE approval | The condonation notification: 65 to 75%, B.Tech and BBA only | S02, S03, S04, S05 |
| B5 | The exam fee (₹1,200) is a flat fee per semester, whatever the number of papers. Backlog and improvement fees are per paper. | The fee structure given | Fee calculation |
| B6 | The attendance rule applies only to current-semester (regular) papers. Backlog and improvement papers are not blocked by this semester's attendance. | Not stated; backlog attendance was earned in an earlier semester | S02 (CS204) |
| B7 | A student blocked from all regular papers does not owe the semester exam fee (it is refunded if already paid). Backlog fees still apply to the backlog papers they write. | Not stated | S03, S05 |
| B8 | Improvement attempts are allowed at the backlog/improvement paper rate | A fee is listed for "backlog or improvement paper," implying both are permitted | S10 |
| B9 | A student can opt out of a backlog paper with no penalty; it carries over to a later cycle | A student asked for exactly this | S07 |
| B10 | Tuition dues give a warning but do not block the exam | Not stated; exam fee and tuition are separate fee heads | S06 |
| B11 | A disciplinary hold blocks all papers for that cycle, and the exam fee is refunded | S11 was debarred for one semester | S11 |
| B12 | A paid student who later becomes ineligible gets a full refund of the fees for the papers they lose | Refunds for this exact situation were left unresolved | S03, S11 |
| B13 | The late fee (₹500) is charged once per student, not per paper | Described as a flat ₹500 late fee | S08, S12 |

## C. Reading the timeline

| # | Assumption | Based on | Affects |
|---|---|---|---|
| C1 | Students who started the form before the 11 Nov deadline cut keep the original 15 Nov deadline | S12 started the form on 11 Nov, before the announcement | S12 |
| C2 | The 14 Nov late fee applies only to students paying after their own deadline (15 Nov if protected by C1, otherwise 12 Nov) | Reading of the timeline | S08, S12 |
| C3 | The 13 Nov fee increase applies only to payments made from 13 Nov; earlier payments stand | Reading of the timeline | S07 at ₹300 |
| C4 | The Dean's 22 Nov request to move the cut-off is approved and applied | The timeline records the request but not an outcome | S02, S03, S09 |
| C5 | "Hall tickets had been printed," in the Dean's account, refers to a provisional print. In this design, nothing is published before the lock. | That account conflicts with the published date of 5 Dec | Change-handling table |
| C6 | S09's correction on 6 Dec moves his figure at the cut-off in force to 75.1% | The sample data gives only the corrected 31 Oct figure | S09 |
| C7 | S08 stands for the "about 40" last-night payments | Numbers given for both | S08 |
| C8 | The prototype replays last semester's dates (hall tickets 5 Dec, exams 12 Dec) so the walkthrough works. The defaults recommended for May (lock 21 days before exams) are separate. | Needed to replay the scenario exactly | Prototype |

## D. Simulated in the prototype

These are simulated in place of real integrations:

| # | Simulated | Real version |
|---|---|---|
| D1 | Payments: a "Pay" button with a chosen timestamp | Payment gateway |
| D2 | Notifications: in-app "What changed for you" notices only | SMS, email, app push |
| D3 | Login and roles: one admin view with an owner label on each setting, plus a student switcher | Role-based access |
| D4 | Attendance, results and dues: loaded from sample data | Live module data |
| D5 | Date control: "today" can be set by the user; deadlines end at 11:59 PM | System clock |
| D6 | Output: final list plus headcount per paper. Room allocation and the timetable are not modelled. | Seating and timetable modules |
