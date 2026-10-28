# Learnings

These notes come from the owner's Frame/Tizen sessions (October 2026). They are
the reason this fork has an operating rule, not just root tooling.

## 1. The pivot anti-pattern (the main learning)

When Tizen work got hard, the agent repeatedly moved the problem off the TV:

- "Treat the Frame as a dumb panel and put the real UI on Apple TV / a Pi / a NUC
  over HDMI. No hardware risk, nothing to root."
- After OTA wipes, Vision AI Companion, certificate friction, or Home still
  opening Tizen, the workstream drifted toward an Apple TV entertainment shell
  and a portal on HDMI playback.
- Design research framed "Apple TV does the browsing" as a product alternative.

The owner's response was explicit: he already has an Apple TV on HDMI; that is
not the goal. He wants root, a clean Home on the Frame, and full Art control
(images, paper-like mat, backlight). An external box as the OS is out of scope.

**Rule:** do not reopen the pivot. Stay on the Frame. Use HDMI only as a
destination launched from our owned Home. Keep Art on the TV. Escalate A → B → C
instead of inventing a parallel product.

## 2. Root is not a new Home

- UID 0 is control. It does not change what the Home key opens.
- A successful `id` or UID-0 readback proves root, not boot persistence, Home
  ownership, or Art behavior.
- A browser mock or dashboard on the Mac is not an on-TV app. Do not present it
  as done.
- "Never see Tizen again" requires a real app/service that owns Home and is
  verified after a cold boot.

## 3. OTA and persistence

- A runtime-only OTA mask dies on a power cycle.
- Tizen 9 OTA can land and bring Vision AI Companion.
- Persist the OTA block across reboot.
- Block fallback to public DNS resolvers, otherwise the block is only cosmetic.
- Re-check both after a reboot and after 24 hours.

## 4. Tizen 9 packaging

- The certificate must be bound to the TV's real device identity, not the network
  API's `uuid` field.
- `sdb install` may only push on Tizen 9.
- Transfer the WGT, then install with `pkgcmd -i -t wgt -p` as appropriate for
  the firmware.
- A rejected WGT is a packaging/certificate problem, not proof that the TV cannot
  be owned.

## 5. Debloat and Art

- Snapshot the package list before debloat.
- Disabling Home/Art packages can cause a flashing "Not Available" state until a
  full reboot. Do not do it casually.
- Prefer controlling Art over disabling Art packages:
  - manage the image library;
  - bake the paper-like mat into the image;
  - set backlight via AVOC where available.
- Keep the original Art assets.
- Do not replace Art with an Apple TV screensaver or Aerials.

## 6. One agent per boot

- Do not re-inject a root agent while one is alive.
- Exit the old session before changing UEP or starting another route.
- Two agents in one boot create confusing evidence and can destabilize the TV.

## 7. Exact model and firmware

- Compatibility is exact model + firmware, not series.
- A series name is not evidence.
- Record `<TV_MODEL>` and `<TV_FIRMWARE>` before changing anything.
- A community report is "reported," not "proven" on this TV.

## 8. Working with the owner

- Be direct about what is proven, reported, or planned.
- Keep the goal in the answer: root the Frame, own Home, keep Art.
- When the owner says "I still want to never see Tizen again," answer with Option
  C work, not with an Apple TV.
- Do not mistake activity for progress: an installed app is not Home ownership;
  a root shell is not a launcher.

## When stuck

1. Name the layer: SDB/Developer Mode, foothold/preflight, root route,
   packaging/certificate, Home takeover, OTA/persistence, or Art.
2. Fix that layer.
3. If the layer is blocked, move to the next option in the ladder.
4. Never "solve" a layer-3 failure by recommending a new TV or an external box.
