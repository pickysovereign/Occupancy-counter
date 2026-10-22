# 06 Tech spec

## Files

Three files, side by side in one folder:

| File | Holds |
|---|---|
| `index.html` | The structure of the page: header, counter panel, banners, notice line, room view, simulation panel, hourly log, dialogs. |
| `style.css` | All colors, layout and animation. Green, amber and red status colors, the greyed-out offline look, and the "Long stay" ring. |
| `script.js` | All behaviour: the sensor simulator, counter, level checker, clock, hourly logger, storage and screen updates described in 03 Backend. |

No other files. No build step. The page opens by double-clicking `index.html`.

## Framework and libraries

- **Framework: none.** The page is small and runs in one browser tab, so plain HTML, CSS and JavaScript are enough. A framework would add setup without adding anything this page needs.
- **Libraries: none.**
  - The room view is drawn with the browser's built-in canvas, so no 3D or drawing library is needed (the room is 2D top-down).
  - Times are formatted with the browser's built-in date features, so no date library is needed.
  - Saving uses the browser's built-in local storage, so no database library is needed.
- **Fonts:** the system font. Nothing is loaded from the internet, so the page also works offline.

## What is saved and where

- **Where:** the browser's local storage, under one key: `occupancyCounter.v1`.
- **What:** the count, the people inside (ID and entry time), this hour's Entered, Left and Peak, when this hour started, the last log time, whether the sensor was offline this hour, and all log rows.
- **Not saved:** demo speed, the sensor-offline switch, walking dots, notices, and the level (worked out again from the count).
- **When:** after every count change, every log row, every reset and every clear.
- Data stays on that one computer and browser. Nothing is sent anywhere.

## Fixed numbers

| Setting | Value |
|---|---|
| Capacity | 20 |
| Warning starts at | 16 |
| Full at | 20 |
| Over capacity from | 21 |
| Log interval | each real clock hour; 60 seconds at demo speed |
| Long stay after | 4 hours; 4 minutes at demo speed |
| Notice shows for | 4 seconds |
| Peek pause inside | 0.5 seconds |
| Group size | 3 |
| Clock tick | every 1 second |

## Do

- Do keep HTML, CSS and JavaScript in their three separate files.
- Do change the count only when a dot crosses the entrance line.
- Do use the exact on-screen wording from 02 Frontend.
- Do keep the capacity, threshold and timing numbers together at the top of `script.js` so they're easy to find and change.
- Do save after every change, and handle a failed save the way 05 Error states describes.
- Do tell the levels apart with the status word as well as the color, so they can be read without color.
- Do make the buttons usable with the keyboard (Tab and Enter).
- Do make the layout fit a laptop screen without scrolling sideways.

## Do not

- Do not use any framework or library.
- Do not add a login, accounts or passwords. Nobody logs in.
- Do not send data to a server or any outside service.
- Do not let the count go below 0.
- Do not block entries at capacity. Keep counting above 20.
- Do not play sounds.
- Do not remove "Long stay" people automatically.
- Do not invent numbers for hours when the page was closed. Mark them "Page closed — no data".
- Do not add a log download or export.
