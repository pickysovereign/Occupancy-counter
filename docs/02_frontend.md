# 02 Frontend

Everything the visitor (staff member) sees, part by part. Words in quotation marks are the exact words shown on screen. `{n}` stands for a number that changes.

The page has eight parts, from top to bottom:

1. Header
2. Counter panel
3. Capacity banner
4. System banner
5. Notice line
6. Room view
7. Sensor simulation panel
8. Hourly log (with staff controls)

There are also two confirmation dialogs, described at the end.

---

## 1. Header

**Shows**
- Heading: "Occupancy Counter"
- Subheading: "Main Dining Room"
- Clock: "Time: {HH:MM}" (the real time, 24-hour)
- Sensor status: "Sensor: Online" (green dot) or "Sensor: Offline" (red dot)
- When demo speed is on, a small tag next to the clock: "Demo speed"

**Visitor can do:** nothing. The header is information only.

---

## 2. Counter panel

**Shows**
- Large number: "{count}"
- Under it: "of 20"
- Status label, one of:
  - "Normal" (green, count 0–15)
  - "Nearly full" (amber, count 16–19)
  - "Full" (red, count 20)
  - "Over capacity" (red, count 21 or more)
- Small line under the status: "Entered this hour: {n} · Left this hour: {n} · Peak this hour: {n}"

The large number and the panel border take the status color.

**Visitor can do:** nothing. The panel is information only.

---

## 3. Capacity banner

A full-width strip directly under the counter panel. **Hidden when the status is Normal.**

**Shows**, depending on the count:
- 16–19, amber: "Warning: {count} of 20 — room is nearly full."
- 20, red: "Full: 20 of 20 — room is at capacity."
- 21 or more, red: "Over capacity: {count} of 20 — {count − 20} over the limit."

**Visitor can do:** nothing. The banner cannot be closed. It disappears by itself when the count drops back to 15 or below, and changes wording as the count crosses 16, 20 and 21.

---

## 4. System banner

A grey strip with a warning icon, under the capacity banner. **Hidden unless something is wrong.** If both problems are happening, both lines show.

**Shows**
- When the sensor is offline: "Sensor offline — count may be inaccurate."
- When saving fails: "Can't save — data will be lost if this page closes."

**Visitor can do:** nothing directly. The banner disappears when the problem is fixed (see 05 Error states).

---

## 5. Notice line

A single line of small text under the banners. Each message shows for **4 seconds** and then fades out. A new message replaces the old one.

**Shows**, one at a time:
- "Exit ignored: room already empty."
- "Exit ignored: everyone inside is already leaving."
- "Saved data couldn't be read — starting from 0."
- "Count reset to 0."
- "Log cleared."
- "{n} missed hour(s) added to the log."

**Visitor can do:** nothing.

---

## 6. Room view

A top-down floor plan of the Main Dining Room, as the ceiling sensor sees it.

**Shows**
- A rectangle with dark walls labelled "Main Dining Room" in the top-left corner.
- A gap in the bottom wall, labelled "Entrance", with a dashed line across it. This dashed line is the counting line.
- A small sensor icon over the entrance, labelled "Sensor".
- One colored dot for each person inside, spread around the room. A dot that is walking moves smoothly to or from the entrance.
- A dot that has been inside for more than 4 hours (4 minutes at demo speed) has an orange ring and the label "Long stay" next to it.
- When the sensor is offline, the whole room view is greyed out.

**Visitor can do:** nothing directly. Dots move only because of the buttons in part 7.

---

## 7. Sensor simulation panel

Heading: "Sensor simulation"

Helper text under the heading: "These controls stand in for the ceiling heat sensor."

**Buttons**

| Button text | What happens |
|---|---|
| "Person enters" | One new dot appears just outside the entrance and walks in. When it crosses the dashed line, the count goes up by 1. It then walks to a free spot in the room. |
| "Group of 3 enters" | Three dots appear side by side outside the entrance and walk in together. Each one adds 1 as it crosses, so the count goes up by 3 within about a second. |
| "Peek in" | One dot walks in across the line (count +1), waits half a second, then walks back out across the line (count −1). |
| "Person leaves" | The dot that has been inside longest (and is not already leaving) walks to the entrance. When it crosses the line, the count goes down by 1 and the dot disappears outside. |

**Switches**

| Switch label | Off (start) | On |
|---|---|---|
| "Demo speed (1 hour = 1 minute)" | The log is written on each real hour. | The log is written every minute. "Long stay" happens after 4 minutes. The "Demo speed" tag shows in the header. |
| "Simulate sensor offline" | Normal counting. | All four buttons are disabled (greyed). Dots that were walking are removed and nothing is counted for them. Header shows "Sensor: Offline". System banner shows the offline message. |

---

## 8. Hourly log

Heading: "Hourly log"

**Table columns:** "Time" · "People inside" · "Entered" · "Left" · "Peak" · "Note"

- Newest row at the top.
- "Note" can be empty or contain one or more of: "No activity", "Sensor offline", "Page closed — no data". For a "Page closed — no data" row, the number columns show "—".
- The row with the highest Peak (the earliest one if there's a tie, and never a "no data" row) has a light highlight and a "Peak hour" tag in the Note column. If every peak is 0, nothing is highlighted.

**Empty state** (no rows yet): "No entries yet. The first entry will be written at {next HH:00}." At demo speed, it says "…written at {HH:MM:SS}." instead, using the time one minute from when demo speed was turned on.

**Staff controls** (two buttons under the table)
- "Reset count": opens the reset dialog.
- "Clear log": opens the clear dialog. Disabled while the log is empty.

---

## Confirmation dialogs

**Reset dialog**
- Text: "Reset the count to 0? Everyone shown in the room will be removed."
- Buttons: "Reset" · "Cancel"
- "Reset": the count becomes 0, all dots disappear, the banners update, and the notice says "Count reset to 0." This hour's Entered, Left and Peak are not changed.
- "Cancel": the dialog closes and nothing changes.

**Clear dialog**
- Text: "Clear the whole hourly log? This cannot be undone."
- Buttons: "Clear" · "Cancel"
- "Clear": every row is removed, the empty state shows, "Clear log" becomes disabled, and the notice says "Log cleared."
- "Cancel": the dialog closes and nothing changes.

---

## The screen before anyone does anything (first ever visit)

- Header: "Occupancy Counter", "Main Dining Room", "Time: {now}", "Sensor: Online".
- Counter: "0", "of 20", green "Normal".
- "Entered this hour: 0 · Left this hour: 0 · Peak this hour: 0"
- No capacity banner, no system banner, no notice.
- Room view: an empty room, entrance and sensor marked.
- All four buttons enabled. Both switches off.
- Hourly log: the empty-state sentence. "Clear log" disabled.

On later visits, the screen opens with the saved count, dots for the saved people, and the saved log rows.

## What changes after each action

| Action | What changes on screen |
|---|---|
| Person enters | A dot walks in. On crossing: the number +1, Entered +1, Peak updates if the count is higher, and the color, status label and banner update if a threshold is crossed. |
| Group of 3 enters | Three dots walk in. Three separate +1 updates within about a second. |
| Peek in | A dot walks in and out. The number goes up by 1 and back down. Entered +1, Left +1. |
| Person leaves (someone inside) | A dot walks out. On crossing: the number −1, Left +1, and the color, label and banner update if a threshold is crossed. |
| Person leaves (room empty) | Nothing moves. Notice: "Exit ignored: room already empty." |
| Demo speed on/off | The "Demo speed" tag appears or disappears. The next log time changes. |
| Sensor offline on | Buttons disabled, room greyed, walking dots removed, "Sensor: Offline", system banner shown. |
| Sensor offline off | Buttons enabled, room in color, "Sensor: Online", offline banner line hidden. |
| An hour passes (or a minute at demo speed) | A new row appears at the top of the log. This hour's Entered, Left and Peak go back to 0 (Peak goes back to the current count). The peak-hour highlight may move. |
| Reset → Reset | See the reset dialog above. |
| Clear log → Clear | See the clear dialog above. |
