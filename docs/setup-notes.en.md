# Setup postmortem: pitfalls and fixes

This document summarizes the points where work stopped and the solutions applied when constructing and replicating the master SD card. It is published to save time for anyone reproducing the same configuration (Raspberry Pi OS Lite 64bit + Martin + nginx + NetworkManager).

## 1. Raspberry Pi Imager setting carryover

Symptom: The password field displays "Saved (hidden) — leave blank to keep".

Imager retains customization from the previous session (password and SSH key path). If you proceed with the field left blank, the "previous value" gets burned to the card, resulting in a card that cannot be accessed with the intended password.

Mitigation: Visually verify the customization screen before each write, and re-enter the password explicitly each time. Rather than relying on Imager for SSH public key configuration, it is more reliable to connect with password authentication on the first boot and then add the key using `ssh-copy-id`.

## 2. .local (mDNS) not found — network mismatch

Symptom: `ping pi-00.local` does not resolve, or returns a previous IP.

mDNS is resolved via multicast within the same network segment. If the Mac and Pi are on different Wi-Fi networks (for example, Mac on a portable router and Pi on a venue AP), resolution will not work. This happened to us during the move from the accommodation to the venue.

Mitigation: First verify that the Mac and Pi are on the same SSID. When it is unclear which network the Pi is on, scan the segment using LanScan or similar and locate the Raspberry Pi by MAC OUI (`b8:27:eb` / `dc:a6:32` / `e4:5f:01` / `d8:3a:dd` / `2c:cf:67`). Tools that show "Raspberry Pi Foundation" in the Vendor field make this immediately obvious.

## 3. SSH Connection refused — initial boot is slow

Symptom: ping works but `ssh` returns Connection refused.

On the first boot, the sequence of filesystem expansion, reboot, user creation, and sshd startup takes 3–5 minutes from power-on. Ping responses begin returning before sshd is ready.

Mitigation: Wait 2–3 minutes and retry. Only if connection is still refused after 5 minutes or more, insert the card into the Mac and place an empty file named `ssh` in the boot partition (`touch /Volumes/bootfs/ssh`).

## 4. Host key warning / known_hosts collision

Symptom: `REMOTE HOST IDENTIFICATION HAS CHANGED` warning, or failure to connect with a different IP than before.

This always occurs when a same-named host's IP changes (due to network relocation) or when connecting to a different card or reflashed card with the same name.

Mitigation: Remove the corresponding line using `ssh-keygen -R pi-00.local` (or the relevant IP) and reconnect. When verifying replicated cards in sequence, combining `ssh-keygen -R pi-00.local; ssh workshop@pi-00.local` into one line and reusing it is faster.

## 5. /tmp tmpfs overflow (512MB RAM machine)

Symptom: `No space left on device` when extracting an archive in `/tmp`. Broken binaries end up on disk, resulting in cryptic runtime errors like `error while loading shared libraries`.

When `/tmp` is on RAM (in this case, 208MB) due to configurations such as logging as tmpfs, extraction of several hundred MB easily causes overflow. Moreover, tar writes partway before failing, so subsequent `mv` operations move broken files.

Mitigation: Perform downloads and extraction in the home directory. After an overflow, clean both the extraction and placement destinations before retrying. Pre-check with `df -h /tmp`.

## 6. Martin gnu build dependency shortage — use musl version

Symptom: `martin: error while loading shared libraries: libuv.so.1`.

The `aarch64-unknown-linux-gnu` release build depends on shared libraries. Plain Raspberry Pi OS Lite does not include libuv.

Mitigation: Use the `aarch64-unknown-linux-musl` build (statically linked). It runs reliably with zero dependencies and improves robustness of distributed images.

## 7. nginx sites-enabled symbolic link

Symptom: Configuration is written and `nginx -t` passes, but the expected location returns 404.

`ln -sf 新設定 /etc/nginx/sites-enabled/default` can produce unintended results when `default` is an existing symbolic link. In this case, the link remained pointing to the original `sites-available/default`.

Mitigation: Always visually verify the link target with `ls -la /etc/nginx/sites-enabled/`. The reliable procedure is to "delete everything and then create":

```
sudo rm -f /etc/nginx/sites-enabled/*
sudo ln -s /etc/nginx/sites-available/workshop /etc/nginx/sites-enabled/workshop
sudo nginx -t && sudo systemctl restart nginx
```

## 8. External dependencies that fail offline (CDN and glyphs)

Symptom: A viewer that works perfectly online becomes blank or loses place-name labels when switched to AP mode (offline).

When loading MapLibre GL JS / pmtiles.js from a CDN (such as unpkg), the entire application stops working the moment it goes offline. Even if libraries are placed locally, if the style's `glyphs` points to an external URL (font glyph delivery), labels silently disappear. The latter is easy to miss.

Mitigation:

- Place JS/CSS on the server itself and load with relative paths
- Place the complete glyph set (PBF) on the server and make `glyphs` a relative path
- Verify using DevTools Network tab to check "are all request domains only the server itself". This is the most reliable offline resilience check.

## 9. Martin's --font targets TTF (not PBF glyphs)

Symptom: Passing a directory of converted PBF glyphs to `--font` yields `No font files found`.

Martin's `--font` is a feature that accepts TTF/OTF and converts them to PBF at runtime. An already-converted glyph set is not recognized as font files.

Mitigation: The shortest path is to serve the complete PBF glyph set as static files via a web server (nginx). If you want Martin to serve it, pass TTF files.

## 10. Long heredoc paste errors

Symptom: Pasting a multi-line script via SSH using `tee <<'EOF'` results in a corrupted file with missing or garbled lines.

Depending on terminal and network conditions, long paste operations can become corrupted. Broken scripts appear plausible at first glance, so corruption is discovered only upon execution.

Mitigation: Transfer long scripts via scp. If pasting is necessary, visually verify the complete text with `cat` immediately after pasting before execution. When time is tight, as in this case, switching to a one-command-at-a-time approach is also effective.

## 11. Three dd fundamentals (macOS)

- `of=~/master.img` fails because `~` is not expanded under sudo. Use the full path `/Users/name/master.img`
- Always verify the target disk with `diskutil list`. Only use one marked `external, physical` with matching size. Pay special attention in environments with many disk images like the iOS Simulator
- Using `rdiskN` (raw device) is significantly faster than `diskN`

## 12. Pre-replication 'reset to initial state' verification

Image extraction must be done after resetting to the initial state. For this configuration:

- Extracting while still in AP mode results in all replicated cards starting in AP mode
- Forgetting to restore hazard from `.bak` after substitution exercises results in distributing cards with the answer revealed
- Including autoconnect on/off and hostname initial values, verify end-to-end once before extraction what state the card will be in when powered on

## 13. Use SKIP CUSTOMISATION when replicating with Imager

When writing replicated images from a master, always select "SKIP CUSTOMISATION" in Imager's customization screen. All configuration is already burned into the image, and applying Imager's settings here (which may be carryover values from before) will overwrite and corrupt them. Keep verify enabled.
