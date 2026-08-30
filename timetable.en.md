# timetable.md — Timetable (5-minute intervals)

Serving Map Tiles from a Raspberry Pi / 2026-08-30 14:00–17:00 / Room 610

Legend: talk = instructor talks / demo = instructor demos / hands-on = participants hands-on

## Part 1: 14:00–14:50 (lecture-focused, online)

| Time | Duration | Content | Format |
|---|---|---|---|
| 14:00 | 5 | Opening. Communications: ask questions by raising hand or directly during breaks; links displayed on screen + added to public page after session; follow-up via GitHub Issues. Burning stations (2 locations): SD cards burned on-site in sequence. When called, stand up and burn your own card. | talk |
| 14:05 | 5 | Translation system guide: Japanese → Gemini English translation displayed on screen, log download available. Instruct PDF download in advance (second half offline). | talk/demo |
| 14:10 | 5 | Instructor introduction (separate slide). Course fee of 7,500 yen goes toward conference operations. | talk |
| 14:15 | 5 | Background: event timing / LOC / pioneers (Hirasawa, UNVT Portable) / Raspberry Pi since university days. | talk |
| 14:20 | 5 | Motivation: mountain huts, conflict zones, polar regions. Comparison table: PWA vs. native cache. Meaning of portable server. | talk |
| 14:25 | 5 | Today's 4 goals and overview. Advance notice: "We'll disconnect the network at the end." | talk |
| 14:30 | 5 | Raspberry Pi basics: 3A+ specs and constraints. How to distinguish between Pi models. | talk |
| 14:35 | 5 | SD card selection and power supply. Physical units available for inspection. | talk |
| 14:40 | 5 | Tile server comparison and why Martin was chosen. What is PMTiles. | talk |
| 14:45 | 5 | Overview of data (planetiler-generated basemap / shelters / hazards). Launch SSH client setup during this segment. | talk/hands-on |
| 14:50 | 10 | Break — Instructor: check-all.sh to verify all units. Handle faulty units. | — |

## Part 2: 15:00–15:50 (hands-on, client phase)

| Time | Duration | Content | Format |
|---|---|---|---|
| 15:00 | 5 | Local verification: drag and drop shelters.pmtiles to pmtiles.io. "A single file becomes a map locally." | demo/hands-on |
| 15:05 | 5 | Reading seat labels (seat number = last octet of IP). SSH connection for all. | demo/hands-on |
| 15:10 | 5 | Connection help time. Those done check hostname / ip a (all show pi-00 just after clone, normal). Those without finished cards move to paired seating. | hands-on |
| 15:15 | 5 | Individualization exercise: sudo /opt/scripts/personalize-sd.sh NN → reboot → reconnect with SSH. Verify hostname is now pi-NN (required to avoid SSID collisions in Part 3). | demo/hands-on |
| 15:20 | 5 | systemctl status martin. Meaning of "running as a service." curl /health → curl /catalog \| jq | demo/hands-on |
| 15:25 | 5 | Inspect /opt/tiles and config.yaml. View browser access in logs with journalctl -u martin -f. | demo/hands-on |
| 15:30 | 5 | Open browser to http://10.x.x.1NN/. Data from Pi is the same as seen locally (verify in DevTools Network). | demo/hands-on |
| 15:35 | 5 | Use click-to-inspect to check shelter attributes. | demo/hands-on |
| 15:40 | 5 | Substitution exercise: mv hazard.pmtiles.bak → restart → catalog → reload to show flood layer. Read overlap with shelters. | demo/hands-on |
| 15:45 | 5 | Catch-up time. For those ahead: small task to observe tile Content-Type and size in DevTools. | hands-on |
| 15:50 | 10 | Break — Instructor: check again. Verify switch-to-ap.sh exists on all units. | — |

## Part 3: 16:00–16:50 (capstone, offline phase)

| Time | Duration | Content | Format |
|---|---|---|---|
| 16:00 | 5 | "No internet from here on." Reference materials as PDF or local Pi clone. Diagram of AP conversion (one-way, SSH drop is normal). | talk |
| 16:05 | 5 | Everyone run sudo /opt/scripts/switch-to-ap.sh. Wait for SSID foss4g-pi-NN to appear. | demo/hands-on |
| 16:10 | 5 | Connect smartphone via QR to personal AP → view map at http://192.168.4.1/ | demo/hands-on |
| 16:15 | 5 | Help time (non-responsive units move to paired seating). Those done try connecting to neighbor's AP. | hands-on |
| 16:20 | 5 | Kill power to Opal. "The map still works" — today's climax. | demo |
| 16:25 | 5 | Power plug exercise: remove → insert → SSID returns → map returns. | demo/hands-on |
| 16:30 | 5 | During recovery wait, explain systemd and auto-connect profile mechanism. Confirm all units recovered. | talk/hands-on |
| 16:35 | 5 | Field deployment design: connection UX / health monitoring / power / SD backup / RF design rationale (5GHz, channel spreading). | talk |
| 16:40 | 5 | Extras: tile generation (on PC) / DEM → Terrain RGB / Maputnik / autohotspot / debris-flow layer substitution. | talk |
| 16:45 | 5 | Q&A (including deferred questions). | talk |

## Closing: 16:50–17:00

| Time | Duration | Content | Format |
|---|---|---|---|
| 16:50 | 3 | Four key takeaways: portable in a box / no device prep needed / updates via one file / autonomous recovery. | talk |
| 16:53 | 2 | Free equipment offer for students (separate slide). Interested parties see instructor after session. | talk |
| 16:55 | 5 | Collect Pi units (verify by seat number), conduct survey, post-session contact via GitHub Issues, repository details. | talk |
| 17:00 | — | End | — |

## Progress notes (morning-of change: SD card burning on-site)

- Pre-burned cards were not ready, so **2 burning stations** are set up
  to continuously burn on-site from before doors open. Participants are called
  in order to burn their own card
  (Imager / SKIP CUSTOMISATION / verify enabled — see setup-notes 13)
- 15–20 minutes per card × 2 in parallel. Seats without finished cards proceed paired;
  move to independent seating once burned
- All cards are clones, so individualization exercise using personalize-sd.sh
  is added at 15:15. IPs are fixed to Pi MAC address lease, so seat number = IP
  convention is maintained even with clones

## Progress notes

- Buffer slots: 15:45 (end of Part 2) and 16:15 (AP help time)
- If running over, compress in this order: 16:35 design discussion (show slides,
  refer to materials) → 16:40 Extras. Keep closing unchanged
- If ahead of schedule: Maputnik from Extras shown as instructor demo on PC
