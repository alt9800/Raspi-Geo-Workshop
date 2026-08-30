# instructor-notes.md — Instructor's Cheat Sheet (Day-of To-Do List)

Chronological action list. All the "instructor's hands-on work" not written in the slides goes here.
Public-facing content only (credentials not listed here → see wifi-credentials.env).

## Day-of Morning Change: SD Flashing On-Site

Pre-burned cards did not arrive in time, so switching to the following approach.

- **Start burning with 2 writers right now** (15–20 minutes per card + verify.
  The number of cards flashed before doors open reduces the day-of load)
- The image only needs to be copied to 2 station PCs
  (Copy via 1 USB stick in sequence. Distribution to 16 people is not needed)
- Keep the 2 stations running throughout the lecture. Call the next person each time a card finishes
  (Call order can follow seat numbers. Display "Next: Seat NN" in the corner of the screen to avoid raising your voice)
- Post procedure card at station: Use custom → Select image →
  **SKIP CUSTOMISATION** → leave verify enabled
- Seats that don't finish in time can share. Move to their own Pi once flashing is done
- All cards are clones → **15:15 personalization exercise (personalize-sd.sh) becomes mandatory**
  workflow. Skipping this will cause all access points' SSIDs and channels to collide in part three
- Confirmation (just once before starting to flash):
  - [ ] Check that the master image contains /opt/scripts/personalize-sd.sh.
        If missing, prepare an alternative procedure using direct nmcli / hostnamectl calls
  - [ ] Verify that the master was sourced in a state passing setup-notes step 12 (AP revert, hazard .bak backup,
        autoconnect defaults)

## Before Doors Open (–13:45)

Materials checklist:

- [ ] Raspberry Pi 3A+ x15 (SD cards inserted, seat-number labels applied)
- [ ] Power adapter + microUSB cable x15
- [ ] Opal (GL-SFT1200) — must be the unit already registered with fixed lease
- [ ] Seat labels (seat number / IP suffix / SSH password / AP connection QR)
- [ ] Demo units to pass around: various SD cards, power adapter, (if available) similar device
- [ ] Instructor PC: confirm HDMI output, confirm translation display (Gemini) is ready
- [ ] Capture card (for displaying demo Raspberry Pi on the front screen, if needed)
- [ ] Confirm survey QR slide is in the deck
- [ ] Verify materials PDFs are latest build (slide.pdf / private-slides.pdf)

Venue setup:

- [ ] Start Opal and confirm access from instructor PC to 10.x.x.1
- [ ] Start one verification Pi early → confirm `check-all.sh` passes
- [ ] Place Pi, power, and labels at each seat (at this point participant Pis remain powered OFF.
      Power on happens at 14:45 by participants themselves)
- [ ] Set up projector layout with slides + translation display side by side

## Part One 14:00–14:50

| Time | Action |
|---|---|
| 14:00 | Course opening. Explain communication flow (raised hand / screen share / post-course via GitHub Issues). Display public page URL at all times |
| 14:05 | Demo translation display. **Have all participants download the materials PDF at this point** (needed for offline in second half. Skipping this breaks part three) |
| 14:10 | **Switch deck to private-slides.pdf** → self-introduction, course fee, icebreaker → return to main slides |
| 14:15 | Origin of the course. Mention UNVT Portable |
| 14:20 | Motivation and comparison table |
| 14:25 | Four goals. Be sure to announce "We will cut the network at the end" |
| 14:30 | Raspberry Pi basics. Can start passing around demo units from here |
| 14:35 | SD cards and power. Remind people to return demo units |
| 14:40 | Server comparison and PMTiles |
| 14:45 | Data introduction → **have participants turn on Pi power** → confirm SSH client runs (`ssh -V`) |

Break 14:50–15:00:

- [ ] Circulate with `check-all.sh`. Identify failed units
- [ ] Address failed units (reseat power → if no luck, switch to backup mode. Pick spare seating partners)
- [ ] Confirm demo units have been returned

## Part Two 15:00–15:50

| Time | Action |
|---|---|
| 15:00 | Demo drag-and-drop shelters.pmtiles to pmtiles.io → have all do it. File available at /shelters.pmtiles on public page, each downloads their own |
| 15:05 | Explain seat label layout → everyone SSHes together. **Demo end-to-end on your own screen first** |
| 15:10 | Help time. For known_hosts issues direct to docs/troubleshooting-network.md. Let everyone know that hostname pi-00 is normal. Assign unburned seats to share. |
| 15:15 | Personalization exercise: `sudo /opt/scripts/personalize-sd.sh NN` → reboot → re-SSH → confirm hostname. **Confirm everyone completes before moving on** (prerequisite for part three) |
| 15:20 | `systemctl status martin` and `curl /health` → combine with `curl /catalog \| jq` |
| 15:25 | /opt/tiles and config.yaml → have them run `journalctl -u martin -f` continuously |
| 15:30 | Browser to `http://10.x.x.1NN/`. Show access appearing in journalctl. Also show DevTools Network |
| 15:35 | Click-to-inspect. Draw attention to attributes (name, disaster category) |
| 15:40 | Swap exercise. mv → restart → catalog → reload. **Have them read the overlap** between flood zones and shelters |
| 15:45 | Catch-up time. For fast finishers, offer small task: observe Content-Type / file sizes |

Break 15:50–16:00:

- [ ] Re-circulate (`check-all.sh`)
- [ ] **Confirm /opt/scripts/switch-to-ap.sh exists on all units** (can include `ls` check in circulation script)
- [ ] Confirm Opal power-down sequence (where and what you'll unplug at 16:20)

## Part Three 16:00–16:50

| Time | Action |
|---|---|
| 16:00 | Announce "We will not use the network from here on." Diagram AP transformation. **Emphasize one-way traffic and that SSH will disconnect normally** — say this twice |
| 16:05 | Everyone runs `sudo /opt/scripts/switch-to-ap.sh`. Wait until SSID foss4g-pi-NN appears (1–2 minutes) |
| 16:10 | Each participant scans seat QR on phone to join their own AP → `http://192.168.4.1/` |
| 16:15 | Help time. Units that don't start up share with others. Finished people can join adjacent APs |
| 16:20 | **Pull Opal power plug**. Pause. "Yet the map still works" — the highlight moment of today. Build the drama slowly |
| 16:25 | Power-unplug exercise. Pull → wait 10 seconds → plug → SSID returns → map returns |
| 16:30 | While waiting for recovery, explain systemd / autoconnect mechanism verbally. **Confirm everyone's SSID has returned before continuing** |
| 16:35 | Design theory. If running over, show slides and compress ("see the materials for details") (compression priority 1) |
| 16:40 | Extra topics introduction (compression priority 2). If racing ahead, demo Maputnik on instructor PC |
| 16:45 | Q&A. Revisit questions deferred during hands-on |

## Closing 16:50–17:00

| Time | Action |
|---|---|
| 16:50 | Wrap-up: 4 points |
| 16:53 | **Switch deck to private-slides.pdf** → transfer info (students: free / general public: costs below, Revolut / Kotora transfer / PayPay / cash, available later). Return to main slides and segue "We collect these..." verbally |
| 16:55 | Collect Pis (verify seat numbers. Keep units for participants who want to take them home). Survey QR. Brief on post-course contact via GitHub Issues, re-state repository link |

## Compression Decision Tree

- Buffers are at 15:45 and 16:15
- Compression priority: 16:35 design theory → 16:40 Extra. Do not cut closing
- If running ahead: add Maputnik demo

## Troubleshooting Branches

- **Opal itself fails**: Power down Opal → all units fall back to venue SSID.
  The seat number=IP rule breaks, so rescue via scan-seats.sh and mDNS (pi-NN.local)
  (docs/troubleshooting-network.md)
- **Individual Pi dies**: Move to shared seating. SD cards are 15 total, so no replacement recovery available
  <!-- TODO: Final judgment on backup mode (handoff open decision 2) -->
- **Many PCs cannot SSH**: Clean known_hosts (separate steps for Mac/Windows) →
  if still failing, have participants follow along via projector feed of instructor screen
