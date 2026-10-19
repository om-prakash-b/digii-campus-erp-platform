# Change handling, edge cases, V1 scope and go-live

## 1. Change handling: one row per timeline event

| Event | Students see | Admin sees | Money already paid | Already published |
|---|---|---|---|---|
| 31 Oct, attendance cut-off | Final attendance figure; if short, the reason and any route | Dashboard: eligible / at risk / blocked | None | Nothing yet |
| 1 Nov, form opens | Papers (incl. backlogs), checks, fee, deadline | Live dashboard | None | Nothing yet |
| 11 Nov, deadline 15 to 12 Nov | New date, unless they had started the form: those keep 15 Nov (S12) | Preview: who loses days, who is protected | Unaffected | Nothing |
| 13 Nov, backlog fee ₹300 to ₹400 | Unpaid backlog students see the new amount | Preview: change in total due; settings v+1 | Stands at the rate paid (S07 keeps ₹300). No top-up, no refund | Nothing |
| 14 Nov, extension to 20 Nov plus ₹500 late fee | Late window and fee, only if paying after their own deadline | Preview: who can now pay, and with what fee | Unaffected | Nothing |
| 18 Nov, condonation (65 to 75%, B.Tech and BBA, ₹1,000) | In range: "apply," with a 3-day catch-up window, no late fee (S02, S04, S09). Others: why not (S05, B.Sc) | Preview plus approvals queue | ₹1,000 charged only on applying | Nothing |
| 20 Nov, 11:58 PM, last-night payments | Counted on time: payment start time is used (S08) | Start and bank-confirmation time, auto-matched | Recorded against the student; no Excel | Nothing |
| 22 Nov, cut-off 31 Oct to 20 Nov | S02 becomes eligible (77%). S03 drops to 72%: apply for condonation in 3 days or get a refund | Preview of every student before confirming | Refunds raised automatically (S02's ₹1,000 condonation; S03's ₹1,200 if he doesn't apply) | Nothing (lock is 5 Dec) |
| 30 Nov, last day of classes | No change: attendance after the cut-off doesn't count | None | None | None |
| 5 Dec, hall tickets published | Hall ticket, or the reason they are not on the list | Final list, headcount per paper, money list | Refunds and dues go to Accounts | Final list v1 locked |
| 6 Dec, late correction (S09 to 75.1%) | Now eligible; 3-day window to pay; hall ticket v2 | Preview shows the list change; Dean approval required | New payment only | Amendment v2; old ticket superseded; headcount change sent to seating and printing |
| 12 Dec, exams begin | Hall ticket | Changes frozen | None | None |

## 2. Additional edge cases

| Edge case | How the product handles it |
|---|---|
| A rule change makes a student eligible after their window closed | Catch-up window (default 3 days), no late fee |
| Student drops a backlog paper after paying for it | That paper's fee is refunded automatically |
| Condonation rejected after the fee was paid | Fees refunded; regular papers blocked; backlog papers still allowed |
| A correction lowers attendance after lock | Removed by amendment, refund raised, student told why |
| Bank confirmation arrives after lock | Counted from payment start; student added by amendment |
| CoE locks while items are open (unpaid, condonation pending) | Lock screen lists them; they can join later only by amendment |
| Disciplinary hold released before exams | Student restored; refund cancelled; amendment if after lock |

## 3. Left out of V1, and why

- Per-course attendance: overall is the default; per-course would need course-level rules.
- Per-student overrides: would bring back case-by-case decisions; every outcome comes from a rule.
- Bank-counter payments: online only, to end manual matching. V1.1: Accounts records counter payments.
- Failed-payment retry, timetable clashes, which improvement grade counts: owned by the gateway, timetable and Results modules.
- Changing the exam start date once live: would recalculate every date; not in V1.

## 4. Dependencies on other teams

- Results: backlogs and grades per student (read only).
- Attendance: percentage for any date range, edit timestamps, Dean-approval flag on late corrections.
- Fees / payment gateway: exam fee heads, payment start time, automatic refunds.
- Hall ticket, timetable and seating: read the final list and headcounts instead of course registration.
- Notifications: in-app now; SMS and email later.

## 5. Go-live before May

| When | What |
|---|---|
| January | Build integrations with Results, Attendance and Fees |
| February | Dry run: replay last December's data and timeline; fix gaps |
| March | CoE and Accounts configure the May cycle; staff training |
| April | Form opens; list locked 21+ days before exams |
| May | Exams; review metrics with the CoE; then offer to other universities |

## 6. Corrections to the solution brief

A few decisions changed after the first draft of the solution brief, while working through the edge cases:

- Exam start date cannot change once live in V1 (the solution brief originally said "with preview").
- New setting: catch-up window after a change. Default 3 days, no late fee; owner CoE; cannot change once live.
- Metrics: kept to four core ones: final list on time, ineligible students in the hall, students without a seat, papers printed correctly.
