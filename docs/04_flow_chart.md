# 04 Flow chart

Rectangles are actions. Diamonds are questions, each numbered Q1–Q14 with a Yes and a No path. Every path ends back at **Waiting**, which is the way back to the start of the loop.

```mermaid
flowchart TD
    S([Start: page opens]) --> A1[Read saved data]
    A1 --> Q1{Q1. Is the saved data readable?}
    Q1 -- Yes --> A2[Restore count, people and log]
    Q1 -- No --> A3[Start at 0, show 'Saved data couldn't be read']
    A2 --> Q2{Q2. Did any full hours pass while the page was closed?}
    A3 --> Q2
    Q2 -- Yes --> A4[Add one 'Page closed — no data' row per missed hour]
    Q2 -- No --> W
    A4 --> W

    W([Waiting: show count, room and log]) --> Q3{Q3. Is the sensor online?}
    Q3 -- No --> A5[Disable buttons, show 'Sensor offline' banner]
    A5 --> Q9
    Q3 -- Yes --> Q4{Q4. Was Enter, Group of 3 or Peek pressed?}

    Q4 -- Yes --> A6[Send dots toward the entrance]
    A6 --> W
    Q4 -- No --> Q5{Q5. Was Person leaves pressed?}

    Q5 -- Yes --> Q6{Q6. Is anyone inside who isn't already leaving?}
    Q6 -- Yes --> A7[Send the longest-inside dot to the entrance]
    A7 --> W
    Q6 -- No --> A8[Show 'Exit ignored' notice, count unchanged]
    A8 --> W
    Q5 -- No --> Q7{Q7. Did a dot cross the line inward?}

    Q7 -- Yes --> A9[Count +1, Entered +1, update Peak]
    A9 --> Q11
    Q7 -- No --> Q8{Q8. Did a dot cross the line outward?}
    Q8 -- Yes --> A10[Count −1, Left +1]
    A10 --> Q11
    Q8 -- No --> Q9

    Q9{Q9. Has an hour passed? 1 min at demo speed} -- Yes --> Q10{Q10. Were there 0 entries and 0 exits this hour?}
    Q10 -- Yes --> A11[Write row with note 'No activity']
    Q10 -- No --> A12[Write row with numbers]
    A11 --> A13[Reset this hour's tallies, save]
    A12 --> A13
    A13 --> W
    Q9 -- No --> Q14{Q14. Has anyone been inside more than 4 hours?}
    Q14 -- Yes --> A14[Mark their dot 'Long stay']
    A14 --> W
    Q14 -- No --> W

    Q11{Q11. Is the count 21 or more?} -- Yes --> A15[Red 'Over capacity' banner]
    Q11 -- No --> Q12{Q12. Is the count exactly 20?}
    Q12 -- Yes --> A16[Red 'Full' banner]
    Q12 -- No --> Q13{Q13. Is the count 16 or more?}
    Q13 -- Yes --> A17[Amber 'Nearly full' banner]
    Q13 -- No --> A18[Green 'Normal', hide banner]
    A15 --> A19[Save to browser storage]
    A16 --> A19
    A17 --> A19
    A18 --> A19
    A19 --> W
```

## The questions in words

| # | Question | Yes | No |
|---|---|---|---|
| Q1 | Is the saved data readable? | Restore it | Start at 0, show notice |
| Q2 | Did any full hours pass while the page was closed? | Add a "no data" row for each | Go to Waiting |
| Q3 | Is the sensor online? | Check the buttons (Q4) | Disable buttons, show banner, still check the clock (Q9) |
| Q4 | Was Enter, Group of 3 or Peek pressed? | Send dots in | Check Q5 |
| Q5 | Was Person leaves pressed? | Check Q6 | Check Q7 |
| Q6 | Is anyone inside who isn't already leaving? | Send a dot out | Show "Exit ignored" |
| Q7 | Did a dot cross the line inward? | Count +1, check level | Check Q8 |
| Q8 | Did a dot cross the line outward? | Count −1, check level | Check Q9 |
| Q9 | Has an hour passed (1 minute at demo speed)? | Check Q10 | Check Q14 |
| Q10 | Were there 0 entries and 0 exits this hour? | Row with "No activity" | Row with numbers |
| Q11 | Is the count 21 or more? | Over capacity | Check Q12 |
| Q12 | Is the count exactly 20? | Full | Check Q13 |
| Q13 | Is the count 16 or more? | Nearly full | Normal |
| Q14 | Has anyone been inside more than 4 hours (4 minutes at demo speed)? | Mark "Long stay" | Go to Waiting |

Peek in follows the same path twice: once through Q7 (in) and once through Q8 (out).
