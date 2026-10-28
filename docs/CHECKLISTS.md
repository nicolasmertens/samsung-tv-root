# Checklists

Light stubs only. Replace placeholders in your private host configuration, never
in this repository. Do not add LAN addresses, device identities, certificate
material, tokens, or credential paths.

## Pre-flight

- [ ] Exact `<TV_MODEL>` recorded.
- [ ] Exact `<TV_FIRMWARE>` recorded.
- [ ] Tizen Studio and `sdb` installed; `<SDB_PATH>` known.
- [ ] Developer Mode enabled; controller `<HOST_IP>` entered on the TV.
- [ ] TV rebooted after enabling Developer Mode.
- [ ] `sdb devices` lists only the intended TV.
- [ ] Package list snapshotted.
- [ ] Original Art assets backed up.
- [ ] Recovery path understood: full reboot, package restore, factory reset.
- [ ] One root agent per boot rule understood.

## Option A — prototype

- [ ] Browser/featured-page UI loads on the TV.
- [ ] Remote/D-pad navigation is usable at TV distance.
- [ ] Art presentation concept is tested.
- [ ] Notes captured: what works, what fails, what B must fix.
- [ ] Exit A once the interaction model is validated.

## Option B — signed app

- [ ] Certificate profile created and bound to the TV's real device identity.
- [ ] WGT signed for the TV.
- [ ] WGT built at local `<WGT_PATH>` and pushed or transferred to the TV.
- [ ] Tizen 9 path used: `pkgcmd -i -t wgt -p` where applicable.
- [ ] App launches from the TV UI.
- [ ] Autorun Last App configured.
- [ ] Cold-boot test: app starts automatically.
- [ ] Home-key behavior documented (Tizen or bounce-watcher).
- [ ] No private values committed.

## Option C — Home takeover

- [ ] Service design written down before installing anything.
- [ ] Recovery path tested from a known-good state.
- [ ] Service starts after cold boot.
- [ ] Home key opens our Home, not Tizen.
- [ ] Host asleep test: TV still behaves.
- [ ] Art image, mat, and backlight verified after cold boot.
- [ ] OTA block and DNS lock verified after cold boot.
- [ ] Package state and Art assets restorable.
- [ ] Evidence recorded without secrets.

## After every reboot

- [ ] TV boots without a recovery loop.
- [ ] Intended Home appears.
- [ ] Art works.
- [ ] Root/controller state is as expected.
- [ ] OTA block is still active.
- [ ] DNS fallback is still blocked.

## Rollback

- [ ] Know the last known-good package state.
- [ ] Know how to full-reboot the TV.
- [ ] Know how to restore Art assets.
- [ ] Know when a factory reset is the safer move.
- [ ] Do not debloat further while the TV is unstable.
