---
name: Classroom finder constraints
description: Durable product rules for trustworthy room availability and admin access.
---

Room availability must be calculated from the separate active room master minus occupied rooms in verified timetable entries; expose uncertainty when university-wide timetable coverage is incomplete.

**Why:** A room absent from one timetable is not evidence that it is available across campus, and the initial dataset does not include the full university room inventory.

**How to apply:** Keep new departments, semesters, sections, and submissions in the normalized timetable tables; only verified records may affect finder results.

Admin review data must remain behind managed authentication while student finder, repository, and contribution entry stay public.

**Why:** Timetable approval changes the trust boundary for all downstream availability results.

**How to apply:** Protect admin API endpoints server-side and keep unauthenticated users on an explicit sign-in prompt rather than silently exposing review data.
