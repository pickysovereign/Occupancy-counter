# 05 Error states

Everything that can go wrong, and the edge cases. "Notice line" messages fade out after 4 seconds. Banner messages stay until the problem is fixed.

## Errors

### E1. Exit when the room is empty
- **Message:** "Exit ignored: room already empty."
- **Where:** notice line.
- **Meanwhile:** no dot moves. The count stays at 0. Left this hour does not change.
- **Back to normal:** the notice fades after 4 seconds. Nothing else needs fixing.

### E2. Exit when everyone inside is already walking out
- **Message:** "Exit ignored: everyone inside is already leaving."
- **Where:** notice line.
- **Meanwhile:** no extra dot is sent. The dots already leaving carry on and are counted out as they cross.
- **Back to normal:** the notice fades after 4 seconds.

### E3. Sensor offline
- **Message:** "Sensor offline — count may be inaccurate." The header also changes to "Sensor: Offline".
- **Where:** system banner and header.
- **Meanwhile:**
  - The count is frozen at its last value.
  - All four simulation buttons are disabled.
  - Dots that were walking are removed and not counted. A dot that was walking out goes back to being a person inside.
  - The room view is greyed out.
  - The clock and hourly log keep running. Any row for an hour in which the sensor was offline gets the note "Sensor offline".
  - Reset count and Clear log still work.
- **Back to normal:** turn off "Simulate sensor offline". The banner line disappears, the header shows "Sensor: Online", the buttons work again, and counting carries on from the frozen count.

### E4. Saving fails (storage full or blocked)
- **Message:** "Can't save — data will be lost if this page closes."
- **Where:** system banner.
- **Meanwhile:** counting, alerts and logging all carry on normally in memory. Storage tries to save again after every change.
- **Back to normal:** the first save that works hides the banner line. Nothing that happened in between is lost while the page stays open.

### E5. Saved data can't be read when the page opens
- **Message:** "Saved data couldn't be read — starting from 0."
- **Where:** notice line.
- **Meanwhile:** the page starts as if it were the very first visit: count 0, no people, empty log. The unreadable data is replaced on the next save.
- **Back to normal:** right away. The page is fully usable.

### E6. Over capacity
This isn't a software fault, but it is the main alarm the system exists for.
- **Message:** "Over capacity: {count} of 20 — {count − 20} over the limit."
- **Where:** capacity banner (red), and the counter status label "Over capacity".
- **Meanwhile:** people can still enter and are still counted. The banner number updates with each person.
- **Back to normal:** when the count drops to 20, the banner changes to "Full"; at 19, to "Nearly full"; at 15, it disappears.

## Edge cases

### X1. Two or more people at once
- "Group of 3 enters", or several fast presses of "Person enters", sends several dots at the same time.
- Each dot is tracked on its own and adds exactly 1 at the moment it crosses. Three dots mean three +1 updates, all within about a second.
- If the count crosses a threshold partway through the group (for example 15 → 18), the banner shows the right wording after each step: nothing at 15, "Warning: 16 of 20" at 16, and so on.
- **No message.** This is normal behaviour.

### X2. Someone who never leaves
- A person inside for more than 4 hours (4 minutes at demo speed) gets an orange ring and the label "Long stay" in the room view.
- **No banner, and the count isn't changed automatically**, because the system can't know whether the person is really still there.
- **Back to normal:** the marker goes away when that person leaves. At closing time, staff use "Reset count", which removes all dots including long stays.

### X3. Nothing happens for an hour
- The hourly row is still written on the hour, with Entered 0, Left 0, Peak equal to the count, and the note "No activity".
- **No message.** The row itself is the record.

### X4. Peek in and straight out
- Counted +1 then −1. Entered and Left both go up by 1. The count ends where it started.
- If the room was at 15, the peek briefly shows "Warning: 16 of 20" and then the banner disappears again.

### X5. Page closed across one or more hours
- When the page reopens, each missed full hour gets a row: number columns "—", note "Page closed — no data".
- **Message:** "{n} missed hour(s) added to the log." in the notice line.
- The current hour's tallies carry on from what was saved.

### X6. Page closed while dots were walking
- A dot that hadn't yet crossed inward was never counted, so it is simply gone.
- A dot that hadn't yet crossed outward is still counted, and reappears inside the room.

### X7. Reset while dots are walking
- All dots disappear, including walking ones, and the count becomes 0. Nothing more is counted for the removed dots.

### X8. "Clear log" on an empty log
- Can't happen: the button is disabled while the log is empty.

### X9. Tie for peak hour
- If two rows share the highest peak, the earlier one is highlighted. If every peak is 0, no row is highlighted.
