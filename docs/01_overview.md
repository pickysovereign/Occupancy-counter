# 01 Overview — Occupancy Counter

## Scenario

An occupancy counter with a capacity limit, counting in and out, with a warning before the limit.

## Overview

An occupancy counter is a system that tracks the real-time number of people in a room or building, and shows a warning when the number of people is close to the capacity limit. It is used by the IT team of any organization to track the real usage of a room, its peak hours and the count of people inside. One real example is a restaurant: the system records the number of people in the building each hour to identify the peak hours.

The system uses a heat sensor to track the number of people and their movements individually. The heat sensor is placed on the ceiling, facing the entrance to the room or building. The system knows the borders of the room from a map of the room with the entrance marked on it.

The system starts with an initial count of 0 visitors and changes each time a person walks in or leaves. If a person peeks in for a second and immediately leaves, the system still counts that person in and out. If two or more people come in at the same time, the system counts each of them in real time.

The system shows the real-time number of people in the room on a screen. Finally, the system logs the number of people in the room every hour.

## Answers to open questions

These answers were collected before writing documents 02–07. Where they make the text above more precise, they take priority.

### What gets built

- The project is one web page. There is no real heat sensor, so the page **simulates** the sensor in two ways that work together:
  - a **room view** where dots (people) walk through the entrance, and
  - **buttons** that send people in or out.
- The room view is a **2D top-down floor plan**, as the ceiling sensor would see it, with the entrance and the sensor marked. It is not real 3D.
- A person is counted the moment their dot **crosses the entrance line**, not the moment a button is pressed.
- The room is called **"Main Dining Room"**. The page heading is **"Occupancy Counter"**.
- The page is for the IT team or restaurant staff. **Nobody logs in.**

### Capacity and warnings

- **Capacity limit: 20 people.**
- **Normal:** 0–15 people. The counter is green.
- **Warning:** 16–19 people (80% of capacity). The counter turns amber and a banner appears.
- **Full:** exactly 20 people. The counter turns red and a banner appears.
- **Over capacity:** 21 or more. A sensor cannot stop a person, so the count keeps going up. The counter stays red and the banner says how many people are over the limit.
- Warnings are shown with **color and banner text only, with no sound**.

### Counting rules

- **Peek:** a "Peek in" button sends a dot across the line and straight back out. It counts +1 and then −1.
- **Several people at once:** a "Group of 3 enters" button sends 3 dots side by side, and each adds 1 as it crosses. Pressing "Person enters" quickly several times also works.
- **Count never goes below 0.** If "Person leaves" is pressed when the room is empty, nothing changes and a short notice explains why.

### Hourly log

- A row is written **on the hour by the real clock** (15:00, 16:00, …).
- A **"Demo speed"** switch makes 1 hour = 1 minute, so the log can be shown working in class.
- Each row records: **time, people inside, entered, left, and peak** (the highest count during that hour).
- An hour with no entries or exits is still logged, with the note **"No activity"**.
- If the page was closed when an hour struck, that hour gets a row marked **"Page closed — no data"** when the page reopens.
- The row with the highest peak is highlighted and labelled **"Peak hour"**.
- The log is shown on the page only. It cannot be downloaded.

### Saving and resetting

- The count, the people inside and the log are **saved in the browser** and are still there after a reload. The count starts at 0 only on the very first visit or after a reset.
- Staff can **reset the count** and **clear the log**. Both ask "Are you sure?" first.
- If saving fails, counting carries on and a banner warns that data will be lost if the page closes.

### Edge cases and faults

- **Someone who never leaves:** a dot that has been inside for more than 4 hours (4 minutes at demo speed) gets a **"Long stay"** marker. The count is not changed automatically. Staff can reset at closing time.
- **Sensor fault:** a "Simulate sensor offline" switch stands in for a disconnected sensor. The count freezes, a banner appears, log rows are marked "Sensor offline", and everything goes back to normal when the switch is turned off.
