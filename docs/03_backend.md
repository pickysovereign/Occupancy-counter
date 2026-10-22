# 03 Backend

What runs out of sight inside the page. There is no server: everything runs in the visitor's browser.

**Nobody logs in.**

## Parts that work behind the screen

| Part | Job |
|---|---|
| **Loader** | Reads saved data when the page opens. |
| **Sensor simulator** | Creates and moves dots, and notices when a dot crosses the entrance line. Stands in for the heat sensor. |
| **Counter** | Keeps the count and the list of people inside. |
| **Level checker** | Decides Normal / Nearly full / Full / Over capacity from the count. |
| **Clock** | Ticks once per second. |
| **Hourly logger** | Adds up this hour's entries, exits and peak, and writes a log row on each hour. |
| **Storage** | Saves to the browser's local storage and reports whether the save worked. |
| **Display** | Updates what's on screen. It only receives messages and never decides anything. |

## What runs, in order

### When the page opens
1. The Loader reads the saved data from browser storage.
2. If the data is missing, it starts with the starting values listed below. If the data can't be read, it also starts fresh and sends a notice to the Display.
3. The Loader works out which full hours passed while the page was closed and adds one "Page closed — no data" row for each.
4. The Counter puts back the count and the people inside. The Sensor simulator draws a dot for each person at a spot in the room.
5. The Level checker works out the level and the Display shows everything.
6. The Clock starts ticking.

### While the page is open (repeats)
7. When the visitor presses a simulation button, the Sensor simulator creates dots or chooses one to walk out.
8. Each time a dot crosses the entrance line, the Sensor simulator tells the Counter.
9. The Counter changes the count, updates the people list, and tells the Level checker, the Hourly logger, the Display and Storage.
10. Each second the Clock ticks:
    - The Display updates the time.
    - The Hourly logger checks whether an hour (or a minute at demo speed) has passed. If one has, it writes a row.
    - The Sensor simulator checks for people inside longer than 4 hours (or 4 minutes) and marks them "Long stay".
11. After every change worth keeping, Storage saves. If the save fails, Storage tells the Display, and it tries again on the next change.

## Messages

| # | Message | Sent by | Received by | Carries |
|---|---|---|---|---|
| M1 | "Enter requested" | Visitor (button) | Sensor simulator | how many: 1 or 3 |
| M2 | "Peek requested" | Visitor (button) | Sensor simulator | nothing |
| M3 | "Leave requested" | Visitor (button) | Sensor simulator | nothing |
| M4 | "Crossed in" | Sensor simulator | Counter | person ID, time |
| M5 | "Crossed out" | Sensor simulator | Counter | person ID |
| M6 | "Exit ignored" | Sensor simulator | Display | the reason: empty, or all leaving |
| M7 | "Count changed" | Counter | Level checker, Hourly logger, Display | new count, in or out |
| M8 | "Level changed" | Level checker | Display | Normal / Nearly full / Full / Over capacity |
| M9 | "Tick" | Clock | Hourly logger, Sensor simulator, Display | current time |
| M10 | "Row written" | Hourly logger | Display | the new row |
| M11 | "Long stay found" | Sensor simulator | Display | person ID |
| M12 | "Save" | Counter, Hourly logger | Storage | the data to keep |
| M13 | "Save failed" / "Save worked" | Storage | Display | nothing |
| M14 | "Reset confirmed" | Visitor (dialog) | Counter, Sensor simulator | nothing |
| M15 | "Clear confirmed" | Visitor (dialog) | Hourly logger | nothing |
| M16 | "Sensor offline" / "Sensor online" | Visitor (switch) | Sensor simulator, Hourly logger, Display | on or off |
| M17 | "Demo speed on" / "off" | Visitor (switch) | Hourly logger, Sensor simulator, Display | on or off |
| M18 | "Data restored" / "Data unreadable" | Loader | Counter, Hourly logger, Display | the saved data, or nothing |

## What it remembers

| Thing | Kind of value | Starts as | Kept after page closes? |
|---|---|---|---|
| Count | whole number, 0 or more | 0 | Yes |
| People inside | list of (person ID, time they entered) | empty list | Yes |
| Entered this hour | whole number | 0 | Yes |
| Left this hour | whole number | 0 | Yes |
| Peak this hour | whole number | 0 | Yes |
| Hour started at | date and time | the time the page first opened | Yes |
| Last log time | date and time, or none | none | Yes |
| Log rows | list of (time, people inside, entered, left, peak, notes) | empty list | Yes |
| Hour had a sensor fault | true / false | false | Yes |
| Level | one of four words | "Normal" | No (worked out from the count) |
| Dots walking | list of (person ID, direction, position) | empty list | No |
| Demo speed | true / false | false | No |
| Sensor offline | true / false | false | No |
| Saving is failing | true / false | false | No |
| Current notice | text | empty | No |

Everything marked "Yes" is saved together under one name in the browser's local storage. It stays on that computer and in that browser only.

## Rules the backend follows

- The count changes **only** when a dot crosses the entrance line. Pressing a button never changes it directly.
- Before a dot crosses inward, it is not counted. If the page closes while a dot is walking in, that person was never counted.
- A dot walking out is still counted until it crosses. If the page closes mid-walk, that person is still inside when the page reopens.
- The count never goes below 0.
- When a new hour starts, Entered and Left go back to 0 and Peak goes back to the current count.
- A log row's "Sensor offline" note is added if the sensor was offline at any point during that hour.
- At demo speed, "an hour" means 60 seconds from the last row (or from the moment demo speed was turned on). Turning demo speed off goes back to real clock hours. Missed-hour rows are only added for real hours.
