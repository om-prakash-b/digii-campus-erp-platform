# Changes to existing Digii modules

How each existing module works with the new Exam Registration module.

| Module | Change | Today | With the Exam Registration module |
|---|---|---|---|
| Course registration | No change | Maps students to this semester's courses. Backlog courses are not re-registered. | Works exactly as before. The Exam Registration module reads it only to list each student's regular papers; backlogs are taken from Results instead. |
| Results | No change (read only) | Stores each student's paper results and grades. | The Exam Registration module reads past results to find backlog papers (failed or absent) and papers eligible for improvement (passed). Which grade counts after an improvement attempt stays a Results rule and is out of scope for V1. |
| Attendance | Edits after the cut-off need approval | Faculty mark attendance per session and can edit it at any time. The percentage can be reported over any date range. | Marking and reporting are unchanged, and the Exam Registration module reads the percentage at the cut-off date. Edits to sessions before the cut-off, made after the cut-off, need Dean approval, and edits made after the list is locked become amendments. |
| Fees | Exam fee heads, payment time and refunds added | Fee heads, online gateway, receipts and dues tracking. The exam fee is collected outside Digii and matched against an Excel sheet. | The Exam Registration module creates exam fee charges (semester fee, per paper, late fee, condonation) and links each payment to the student automatically. It records both the payment start time and the confirmation time, fixes the fee at the time of payment, and raises refunds or credits automatically when a change requires one. |
| Hall tickets | Now come from the final list | Generated directly from course registration, so every registered student gets one. | Generated from the Exam Registration module's final locked list, showing only papers the student is cleared to write, including backlog and improvement papers. A change after lock creates a new hall-ticket version, and the old one is marked superseded. |
| Exam timetable and seating | Now use the final list's counts | Built from course registration counts, which include students who won't write and leave out backlog students. | Built from the final list's headcount per paper, including backlog and improvement papers. Amendments send the change in headcount, so seating and question-paper printing stay accurate. |

In short: course registration and results are only read; attendance and fees get small additions; hall tickets and timetable/seating now take their input from the final list instead of course registration.
