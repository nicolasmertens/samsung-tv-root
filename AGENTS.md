# AGENTS.md — Samsung Frame / Tizen fork

This repository is about owning the Samsung Frame itself: Tizen root, our own
Home, and Art mode on the TV. Read this file before proposing a plan or editing
docs.

## 1. Non-negotiable: stay on the TV

**The persistence rule.** Agents have repeatedly tried to solve "I hate Tizen /
I want my own OS" by moving the problem off the TV. That is the anti-pattern this
fork exists to prevent.

- **Stay on Samsung Frame + Tizen root + Options A → B → C.** Document blockers
  honestly. Change tactics inside the ladder. Do not abandon the on-TV path.
- **Never recommend Apple TV, Fire TV/Fire Stick, Chromecast, NVIDIA Shield, a
  Raspberry Pi, a NUC/mini-PC, or a new TV as the solution to Tizen.**
- **Never reframe the Frame as "just a monitor," "just a panel," or a dumb
  display for another computer.**
- **HDMI is allowed only as a destination launched from an owned on-TV Home.**
  For example, a link to the owner's existing Apple TV for one app is fine. It is
  not permission to move the Home, launcher, UI, or OS onto that box.
- **Art stays on the Frame.** Keep the image library, paper-like mat, and
  backlight control on the TV. Do not replace Art with an Apple TV screensaver,
  Aerials, or an external media player.
- **When stuck, escalate inside the ladder:** A (prototype) → B (signed app +
  Autorun Last App) → C (on-TV root service + real Home takeover). Do not invent
  a parallel product or drift to HDMI because the next step is hard.

If you catch yourself writing "the simplest solution is to use an Apple TV" or
"treat the TV as a monitor," stop. That is the failure mode. Return to the
ladder.

## 2. What "done" means

Root is not a new Home.

- UID 0 is control only. The Home key still opens Tizen until a real app is
  installed and Home is taken over.
- A browser mock or dashboard running on the Mac is not an on-TV app. Do not
  present it as the Home.
- A successful `id` readback proves root. It does not prove boot persistence,
  Home ownership, or Art behavior.
- "Never see Tizen again" is only true after an on-TV launcher/Home is verified
  on the target TV after a cold boot.

Label every claim as one of:

- **proven** on this exact model/firmware;
- **reported** by another owner (not reproduced here);
- **planned** or **candidate** (not tested).

## 3. Compatibility and safety

- **Exact model + firmware, not series.** Record `<TV_MODEL>` and
  `<TV_FIRMWARE>` before changing anything. A series name is not evidence.
- **One injected root agent per boot.** Do not re-inject while an agent is alive.
  Exit the old session before changing UEP or starting another route.
- **OTA is hostile.** A runtime-only OTA mask dies on a power cycle. Tizen 9 OTA
  can land and bring Vision AI Companion. Persist the OTA block across reboot and
  block fallback to public DNS resolvers.
- **Snapshot before debloat.** Capture the package list and original Art assets.
- **Do not disable Home/Art packages casually.** It can cause a flashing "Not
  Available" state until a full reboot. Prefer controlling Art over removing it.
- **Tizen 9 packaging:** `sdb install` may only push. Transfer the WGT, then
  install with `pkgcmd -i -t wgt -p` as appropriate for the firmware.
- **Certificate binding:** the certificate must be bound to the TV's real device
  identity, not the network API's `uuid` field.
- **Keep a recovery path.** Know how to full-reboot, restore package state, and
  factory-reset before making irreversible changes.

## 4. Secrets and repository hygiene

Never commit or paste into docs:

- LAN addresses or hostnames;
- device identities, serial numbers, or DUIDs;
- certificates, private keys, passwords, tokens, or credential paths;
- private photo/Art paths;
- account emails or third-party service tokens;
- private binaries from Vetro, TizenBrew, Samsung, or another owner.

Use placeholders such as `<TV_IP>`, `<HOST_IP>`, `<TV_MODEL>`, `<TV_FIRMWARE>`,
`<PROFILE>`, `<SDB_PATH>`, `<WGT_PATH>`, `<ART_IMAGE>`, and `<TIZEN_STUDIO>`.
Keep real values in the private host configuration or a password manager.

## 5. Working rules for this repo

- Read [README.md](README.md), [docs/OPTIONS.md](docs/OPTIONS.md),
  [docs/LEARNINGS.md](docs/LEARNINGS.md), and
  [docs/CHECKLISTS.md](docs/CHECKLISTS.md) before proposing changes.
- Use the A/B/C vocabulary consistently. Do not invent a parallel project name
  for the same work.
- Keep the upstream root code and attribution intact. The canonical upstream is
  [chris-ritsen/samsung-tv-root](https://github.com/chris-ritsen/samsung-tv-root).
  This fork is [nicolasmertens/samsung-tv-root](https://github.com/nicolasmertens/samsung-tv-root).
  Do not open a pull request upstream from this fork.
- The `LICENSE` file is the upstream Unlicense and must remain intact.
- Do not claim a new exploit route, a new compatible model, or a finished Home
  takeover without an exact-model/firmware test and evidence.
- Do not copy private binaries, private config, or another owner's credential
  paths into this repository. Link to public projects instead.

## 6. Failure handling

When a step fails, name the layer:

1. **SDB / Developer Mode** — no connection, closed shell, wrong host address.
2. **Foothold / preflight** — timing probe fails, callback never arrives.
3. **Root route** — identity mismatch, payload/runtime mismatch, UEP issue.
4. **Packaging / certificate** — WGT rejected, wrong device identity, Tizen 9
   `pkgcmd` path.
5. **Home takeover** — app installs but Home still opens Tizen; Autorun/bounce
   behavior not verified.
6. **OTA / persistence** — changes revert after reboot or update.
7. **Art** — image, mat, or backlight does not persist.

Fix the named layer. Do not "solve" a layer-3 failure by recommending a new TV or
an external box. Escalate to the next option only when the current option's
success condition is met or explicitly blocked.

## 7. Communication

- Be direct about what is proven, what is reported, and what is still a plan.
- Keep the owner's goal in the answer: root the Frame, own Home, keep Art.
- If the owner says "I still want to never see Tizen again," that is a request
  for Option C work, not for an Apple TV.
- If you need to mention an HDMI input, say explicitly that it is a destination
  from our Home, not the new OS.
