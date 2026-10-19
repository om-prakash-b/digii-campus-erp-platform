# Situation and root causes

## The situation

Digii University is a private, multi-disciplinary university in India: 6 schools, about 4,200 students, semester system. It runs course registration, attendance, fees, timetables and results on Digii. After last December's end-semester exams went badly, the Vice Chancellor asked for the exam process to be fixed before the May cycle.

How it works today: every student registered for a course automatically gets a hall ticket. The exam timetable, seating and question-paper counts are all built from course registration data. Eligibility, fees and backlogs are checked manually, outside the system, usually on exam day or after it.

## What went wrong last semester

| Area | What happened |
|---|---|
| Eligibility | 212 students below 75% attendance wrote the exam. Their results were withheld afterwards and parents went to the media. |
| | Students who had not paid the exam fee still got hall tickets. Accounts chased them for two months. |
| Backlogs | 38 backlog students turned up with no seat. Staff used extra answer booklets and wrote their numbers by hand. |
| | One student's hall ticket showed a subject they had already cleared and did not show their backlog. |
| Printing | Question papers were printed from registration counts. Some papers had 30% wasted copies; others ran short. |
| Fees | Exam fees (₹1,200 per semester plus ₹300 per backlog paper) were paid at the bank or on the portal and matched by hand against an Excel list from the exam cell. |
| | The backlog fee was raised from ₹300 to ₹400 midway. Some students paid the old amount and some the new. It is still unsettled. |
| | About 40 students paid online on the last night. The bank shows success, but the internal list shows unpaid. |
| | 25 students paid and were then declared ineligible. Nobody knows whether refunds are owed. |
| Attendance | Attendance is calculated up to a cut-off date (31 Oct), but classes ran until 30 Nov. |
| | Faculty corrected October attendance in November and were still submitting corrections on 6 Dec, after hall tickets were published. Some students moved from 74% to 76% and others the other way. Which number counts? |
| | The Dean asked on 22 Nov to move the cut-off to 20 Nov; by then hall tickets had been printed. |
| | The state notified condonation (65 to 75%, B.Tech and BBA only, ₹1,000 fee) on 18 Nov, mid-process. It was handled on paper. |
| Students | Students learned about short attendance the day before the exam. A deadline change was announced on WhatsApp and missed. Some students had paid but were shown as ineligible. They had no way to drop a backlog paper or request an improvement attempt. |

Rules changed five times in 11 days:

- 11 Nov: form deadline cut from 15 Nov to 12 Nov
- 13 Nov: backlog fee raised to ₹400
- 14 Nov: deadline extended to 20 Nov with a ₹500 late fee
- 18 Nov: condonation notified
- 22 Nov: request to move the attendance cut-off

What the Controller of Examinations (CoE) needs: to know exactly who is writing which paper, at least three weeks before exams.

## Root causes

These are not separate failures. Almost every symptom traces back to two design gaps.

### Root cause A: no exam registration step

The hall ticket is just a copy of course registration. Nothing in Digii records "this student is writing this paper, and here is whether they are allowed to." As a result:

| # | Cause | Symptoms it produces |
|---|---|---|
| R1 | The hall ticket, seating and paper counts come straight from course registration | Low-attendance and unpaid students got hall tickets; printing was wasted or short |
| R2 | Backlog and improvement papers are not in the system | 38 seatless students; wrong subjects on hall tickets; no way to drop a backlog paper or attempt an improvement |
| R3 | Eligibility is checked last: fees are collected and tickets issued first | Results withheld, refunds unresolved, students surprised the day before the exam |
| R4 | Payment is not linked to the exam form: a Google Form plus a bank challan, matched against Excel | 40 students shown unpaid; unpaid students got tickets; "I paid, why am I not eligible?" |
| R5 | Students have no single place to see their exam status | Shortfalls discovered late; WhatsApp-only notices missed |

### Root cause B: rules and data change mid-process with no control

Deadlines, fees, attendance rules and attendance data all changed while the process was underway. Nothing recorded which version applied to whom, nobody saw who would be affected before confirming a change, and there was no defined effect on money already paid or documents already published.

| # | Cause | Symptoms it produces |
|---|---|---|
| R6 | Rule changes are not versioned and their impact is not shown before confirming | Old vs new fee unsettled; students caught by a shortened deadline; chaos when the cut-off move was requested |
| R7 | The attendance figure is never fixed: one cut-off, but records stay editable and classes continue | Eligibility flipped after publication; dispute over which number counts |
| R8 | No rules for exceptions: condonation, refunds, dues, debarment, dropping a paper, improvement | Handled on paper or case by case; outcomes inconsistent |
| R9 | No lock date and no defined process for changes after publication | Printed and published hall tickets went out of date |

### Summary

There is no exam registration record per student per paper, so eligibility is never decided in time. And when rules or data change, nothing recalculates, records the change, or tells the affected people what it means for them.

Fixing each complaint separately (moving the cut-off, matching payments faster, posting notices better) treats the symptoms. The same failures would return in May.
