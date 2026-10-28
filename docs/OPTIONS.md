# Options: A → B → C

This document is the decision ladder for the Frame/Tizen work. It is subordinate
to the persistence rule in [README.md](../README.md): the target is the TV
itself, not an external box.

## How to choose

Pick the lowest option that can answer the question in front of you. Move up when
its success condition is met or when a blocker is named explicitly. Do not skip
to C for status, and do not jump to HDMI because B is annoying.

| Criterion | A: browser/featured page | B: signed app + Autorun | C: on-TV service + Home |
| --- | --- | --- | --- |
| Time to first result | Hours | Days | Days to weeks |
| On-TV UI | Yes | Yes | Yes |
| Home taken over | No | Usually no | Yes, if the service holds |
| Survives reboot | No | Yes, boots the app | Yes, after cold boot |
| Survives OTA | No | Fragile | Fragile; needs OTA/DNS work |
| Art control | No | Partial | Full if implemented |
| Host must stay awake | No | No | No |
| Main risk | Wasted UI work | Certificate/install friction | Bricking, boot loops, maintenance |

## Option A — browser / featured-page hack

**Use for:** a one-night prototype of the Home's layout, copy, remote
navigation, and Art presentation.

**Mechanism:** open a web UI in the Tizen browser or promote it as a featured
page. No root and no signed package are required.

**What it proves:**

- the layout works at TV distance;
- the remote/D-pad interaction is usable;
- the Art presentation concept is worth pursuing.

**What it does not prove:**

- persistence across reboot or OTA;
- Home ownership;
- boot behavior;
- Art library, mat, or backlight control.

**Exit condition:** the interaction model is validated and written down. Move to
B. Do not keep polishing a browser mock as if it were the product.

## Option B — signed app + Autorun Last App

**Use for:** the day-to-day keeper. A real on-TV app that boots with the TV.

**Mechanism:** package the launcher as a WGT signed for the TV, install it, and
use Autorun Last App to start it at boot. Tizen Studio is free. A certificate is
free with a Samsung account and the TV's real device identity.

**What it proves:**

- the app is a real TV app, not a Mac dashboard;
- it can be installed, launched, and updated;
- it can be the first thing the owner sees after boot.

**What it does not prove:**

- the Home key belongs to us. Home still opens Tizen unless a bounce-watcher
  takes over (with a visible flash);
- full Art control;
- resilience to OTA or package changes.

**Certificate and packaging notes:**

- Bind the certificate to the TV's real device identity, not the network API's
  `uuid` field.
- On Tizen 9, `sdb install` may only push. Transfer the WGT, then install with
  `pkgcmd -i -t wgt -p` as appropriate for the firmware.
- If the WGT is rejected, diagnose the certificate/profile, device identity, and
  Tizen generation before changing the app.

**Exit condition:** the app boots reliably on the target TV after a cold boot,
and the owner accepts the Home-key limitation. If the requirement is "never see
Tizen again," continue to C.

## Option C — on-TV root service + real Home takeover

**Use for:** the strict requirement. The TV boots into our Home without a host
computer holding its hand.

**Mechanism:** a small service on the TV holds root across reboot, launches and
owns the Home experience, and exposes a local control path over the loopback SDB
interface. Vetro-style integrations are the public reference shape.

**What it proves:**

- the TV can be self-sufficient after boot;
- the Home experience is genuinely ours;
- the host can sleep and the TV still behaves.

**What it costs:**

- the most engineering and the most recovery risk;
- the most maintenance after Tizen changes;
- possible partner-level certificate requirements for boot services. That is a
  Samsung partner-program process, not a purchase or a license key;
- a bad service can cause a boot loop, a black screen, or a lost Art state.

**Non-negotiables for C:**

- Keep a tested recovery path (full reboot, package restore, factory reset).
- Do not disable Home/Art packages casually.
- Verify Home, Art, and OTA behavior after a cold boot, not only after a warm
  restart.
- Do not redistribute private binaries. Use public Vetro/TizenBrew references
  and document the local integration.

**Done condition:** on the exact target TV, after a cold boot, with the host
asleep: the Frame starts into our Home, the Home key does not fall back to
Tizen, Art still works, and the OTA/DNS block is still in place.

## The only acceptable HDMI use

HDMI is a destination, not the new OS.

- Allowed: a tile/link in **our on-TV Home** that switches to an existing HDMI
  input, such as the owner's Apple TV for one app.
- Not allowed: moving the Home, launcher, UI, or OS to that box.
- Not allowed: treating the Frame as a monitor for another computer.
- Not allowed: using an Apple TV screensaver or Aerials as the Art replacement.

If an HDMI link is mentioned, say explicitly that it is launched from our Home
and that the Home remains on the Frame.

## Public references

- [Vetro](https://github.com/andrearicci93/vetro)
- [TizenBrew](https://github.com/reisxd/TizenBrew)
- [Upstream root toolchain](https://github.com/chris-ritsen/samsung-tv-root)

Use these as public reading and integration references. Do not copy private
binaries, credentials, or another owner's configuration into this repository.
