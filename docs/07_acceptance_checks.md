# 07 Acceptance checks

Each sentence is either true or false when you try it on the finished page.

1. On the very first visit, the counter shows "0" and "of 20" with the green status "Normal", and no banner is visible.
2. After 16 people have entered and none have left, the counter is amber and the banner reads "Warning: 16 of 20 — room is nearly full."
3. When the 21st person crosses the entrance line, the counter shows 21 and the red banner reads "Over capacity: 21 of 20 — 1 over the limit."
4. With the count at 5, pressing "Group of 3 enters" once makes the count 8 within 2 seconds, going up one person at a time.
5. With the count at 0, pressing "Peek in" leaves the count at 0 and adds 1 to both "Entered this hour" and "Left this hour".
6. *(something going wrong)* With the count at 0, pressing "Person leaves" keeps the count at 0 and shows "Exit ignored: room already empty." for 4 seconds.
7. *(edge case)* With demo speed on and no button pressed for 1 minute, a new log row appears with Entered 0, Left 0 and the note "No activity".
8. After the page is reloaded with 7 people inside, the counter still shows "7" and the room view shows 7 dots.
