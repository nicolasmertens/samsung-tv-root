# Samsung Frame: root, Home, and Art

This fork is about one thing: **make the TV itself ours**. The target is a
Samsung Frame running Tizen. The goal is a rooted TV we control, a Home screen we
control, and Art mode that still belongs to the Frame. An external box is not a
substitute for finishing that work.

> ## Persistence rule (non-negotiable)
>
> Agents working in this repository have repeatedly tried to solve "I hate Tizen
> / I want my own OS" by moving the problem off the TV. That is the failure mode
> this fork exists to prevent.
>
> **The work stays on the Samsung Frame + Tizen root + Options A → B → C.**
> Document blockers honestly. Change tactics inside the ladder. Do not abandon
> the on-TV path.
>
> **Do not recommend** Apple TV, Fire TV/Fire Stick, Chromecast, NVIDIA Shield, a
> Raspberry Pi, a NUC/mini-PC, or a new TV as the solution to Tizen. Do not
> reframe the Frame as "just a monitor" or "just a dumb panel" for another
> computer.
>
> **HDMI inputs are allowed as destinations launched from an owned on-TV Home.**
> If the owner already has an Apple TV on HDMI for one specific app, a link to
> that input from our Home is fine. That is not permission to move the Home,
> launcher, UI, or OS onto the external box.
>
> **Art mode stays on the Frame.** Images, the paper-like mat, and backlight
> control belong to the TV's Art experience. Do not replace Art with an Apple TV
> screensaver, Aerials, or an external media player.
>
> **When stuck, escalate inside the ladder:** A (prototype) → B (signed app +
> Autorun Last App) → C (on-TV root service + real Home takeover). Do not invent
> a parallel product or drift to HDMI because the next step is hard.

The owner already has an Apple TV on HDMI. It is not the goal. "I still want to
never see Tizen again" means taking over the Frame, not buying another box.

## Why this fork exists

The public upstream project provides a working host-side root toolchain for
specific Samsung TVs. That toolchain is useful, but root alone does not give us a
new Home: UID 0 is control, not a launcher. The hard part is installing a real
on-TV app, surviving reboots and OTA updates, and owning the Home and Art
experience.

During the original Frame/Tizen sessions, the workstream repeatedly drifted when
that hard part appeared:

- "Use the Frame as a panel and put the UI on Apple TV / a Pi / a NUC over HDMI."
- "Let Apple TV do the browsing" presented as a product alternative.
- After OTA wipes or certificate friction, the next proposal became an Apple TV
  entertainment shell instead of finishing the on-TV Home takeover.

Those proposals are out of scope for this problem. The learnings and guardrails
are recorded in [docs/LEARNINGS.md](docs/LEARNINGS.md) and the options are
spelled out in [docs/OPTIONS.md](docs/OPTIONS.md).

## What is in this repository

- The upstream host CLI and payload source for obtaining volatile root on tested
  Samsung Tizen TVs.
- A continuous host controller that reacquires root after a TV reboot, plus
  remote-input, display, source, volume, screenshot, and overlay controls where
  implemented upstream.
- Frame/Tizen operating notes for the path from a root shell to an owned Home and
  Art experience.
- Lightweight checklists and decision records. No private credentials, device
  identities, LAN addresses, certificates, or private binaries.

The source tree does not contain Samsung firmware, Samsung binaries, extracted
keys, device tokens, serial numbers, private network configuration, or
precompiled exploit payloads. Release archives contain payloads built from this
source.

## Status: be precise about what is proven

Upstream root is owner-tested on exact QN90B and QN90F model/firmware
combinations. A community Frame 2024 report exists, but it is a report, not a
guarantee for every Frame size, region, or firmware.

**Root is not the same as a new Home.** A successful `id` or UID-0 readback only
proves control. The Home key still opens Tizen until a real app is installed and
Home is taken over. Do not claim "never see Tizen" until that takeover is
verified on the target TV after a cold boot.

Compatibility is **exact model + firmware**, not a whole series. Record both
before changing anything.

## Public foundation and credit

This fork builds on [chris-ritsen/samsung-tv-root](https://github.com/chris-ritsen/samsung-tv-root).
The root routes, host controller, payloads, and research notes are upstream work.
This fork adds the Frame/Tizen operating rules, options, and learnings; it does
not claim to have discovered the exploits. The upstream README remains the
authoritative compatibility table and host-tool reference for exact models and
firmware.

Related public projects worth reading before designing Option C:

- [Vetro](https://github.com/andrearicci93/vetro) — an on-TV manager/root-service
  style integration.
- [TizenBrew](https://github.com/reisxd/TizenBrew) — a public Tizen app/module
  ecosystem and a useful reference for packaging and on-TV modules.

Do not redistribute private binaries or credentials from those projects. Link to
their public repositories instead.

## Preconditions

Do not start with root. Start with the things that make root and recovery
possible.

- **You own the TV and accept the risk.** Root, UEP changes, debloat, and OTA
  blocking can void warranties, brick the TV, or leave it in a recovery loop.
- **Exact model and firmware are known and recorded.** Series names are not
  enough. Check the TV's own information screen and write down the exact model
  and firmware/build.
- **Tizen Studio and Samsung `sdb` are installed on the host.** They are not
  redistributed here. Use the version that matches the TV's Tizen generation.
- **Developer Mode is enabled and working.** On the TV: Smart Hub → Apps → focus
  App Settings → enter `12345` → enable Developer Mode → enter the controller
  computer's LAN IPv4 address → reboot. Verify that `sdb` sees only the intended
  TV before continuing.
- **A host controller is available.** A Mac, Linux host, or Windows host can run
  the CLI. The archive route also needs OpenSSL on the host `PATH`. Building
  payloads from source needs the .NET SDK, GNU Make, and the toolchain described
  in [QUICKSTART.md](QUICKSTART.md).
- **The TV and controller can reach each other.** The controller needs a stable
  address, and the TV must be able to call back to the host for the root routes.
- **There is a rollback plan.** Snapshot the package list before debloat, keep
  the original Art assets, and know how to do a full reboot and a factory reset.
- **OTA and DNS behavior is understood.** A runtime-only OTA mask dies on a power
  cycle. Tizen 9 OTA can land and bring Vision AI Companion. Persist the OTA block
  across reboot and block fallback to public DNS resolvers.
- **One root agent per boot.** Do not re-inject an agent while one is alive. Exit
  the old session before changing UEP or starting another route.

## Placeholders used in this repository

Commands and checklists use placeholders so no real values enter git:

| Placeholder | Meaning |
| --- | --- |
| `<TV_IP>` | The TV's current LAN address |
| `<HOST_IP>` | The controller computer's LAN address |
| `<TV_MODEL>` | Exact model string from the TV |
| `<TV_FIRMWARE>` | Exact firmware/build string from the TV |
| `<PROFILE>` | The profile name in the host configuration |
| `<SDB_PATH>` | Full path to Samsung's `sdb` executable |
| `<WGT_PATH>` | Local path to the signed WGT package |
| `<ART_IMAGE>` | Local source image for Art mode |
| `<TIZEN_STUDIO>` | Tizen Studio installation directory |

Never commit real addresses, device identities, certificate material, tokens, or
credential paths. Keep them in the private configuration or password manager.

## The ladder: A → B → C

Each option answers a different question. Do not skip to C just because it sounds
final, and do not retreat to an external box because B has certificate friction.

| Option | What it is | Home taken over? | Survives reboot? | Best use |
| --- | --- | --- | --- | --- |
| **A** | Browser / featured-page hack | No, Home is still Tizen | No; OTA/everyday behavior reverts it | A night of UI prototyping |
| **B** | Signed WGT app + Autorun Last App | Usually no; Home key still Tizen unless a bounce-watcher is used | Yes, boots into the launcher | Day-to-day keeper |
| **C** | On-TV root service + real Home takeover | Yes, if the service holds and owns Home | Yes, after reboot | When never seeing Tizen is non-negotiable |

Journey: **A for a night → B day-to-day → C when never-see-Tizen is
non-negotiable.**

### Option A — browser / featured-page hack

Build the UI fast in the Tizen browser or as a featured page. It is useful for
testing information architecture, copy, layout, and Art presentation.

What it proves: the UI works on the TV panel and with the remote.

What it does not prove: persistence, Home ownership, boot behavior, or Art
control. It reverts with OTA and normal Tizen updates. Do not present it as the
finished product. Exit A once the interaction model is validated.

### Option B — signed app + Autorun Last App

Package the launcher as a WGT signed for the TV, then use Autorun Last App so the
TV boots into it. Tizen Studio is free; a certificate is free with a Samsung
account and the TV's real device identity. This is the pragmatic keeper for many
owners.

What it proves: a real on-TV app can be installed, launched at boot, and used
every day.

What it still does not prove: the Home key belongs to us. Unless a bounce-watcher
takes over Home (with a visible flash), Home still opens Tizen. If the
never-see-Tizen requirement is strict, keep going to C.

Certificate and packaging notes:

- The certificate must be bound to the TV's real device identity, not the network
  API's `uuid` field.
- On Tizen 9, `sdb install` may only push the package. Transfer the WGT, then
  install with `pkgcmd -i -t wgt -p` as appropriate for the firmware.
- If installation is rejected, verify the certificate profile, the TV identity,
  and the Tizen generation before changing the app.

### Option C — on-TV root service + real Home takeover

Run a small service on the TV itself that holds root across reboot, owns the Home
experience, and lets the host sleep. Vetro-style integrations are the reference
shape: a local service on the TV, reached over the loopback SDB path, rather than
a host process that must stay awake.

What it proves: the TV can boot into our Home without a controller computer
holding its hand.

What it costs: the most engineering, the most recovery risk, and the most
maintenance. OTA can still land; root can still be lost; a bad service can leave
the TV in a loop. A partner-level certificate may be needed for boot services.
That is a Samsung partner-program process, not a purchase or a license key.

Do not redistribute private binaries. Use public Vetro/TizenBrew references and
document the local integration. Keep the host controller as the recovery and
development path while C is being built.

## Frame and Art rules

Art is part of the product, not an afterthought.

- Keep Art mode on the Frame. The power-off picture is the Frame's defining
  behavior.
- Control the Art experience by managing the image library, baking the
  paper-like mat into the image, and setting backlight via AVOC where available.
- Prefer controlling Art over disabling Art packages.
- Do not replace Art with an Apple TV screensaver, Aerials, or an external media
  player. That is the pivot anti-pattern wearing a different hat.
- Snapshot the original Art assets and package state before changing anything.
- If debloat causes a flashing "Not Available" state, a full reboot is the first
  recovery step. Disabling Home/Art packages is a known way to cause it.

## Host toolchain

The host CLI is upstream tooling. The examples below use placeholders. See
[QUICKSTART.md](QUICKSTART.md) and [docs/README.md](docs/README.md) for the full
upstream guides and release notes.

### Install and check

```console
git clone https://github.com/nicolasmertens/samsung-tv-root.git
cd samsung-tv-root
python -m venv .venv
.venv/bin/python -m pip install -e .
.venv/bin/python -m samsung_tv_root doctor
```

From a standalone release, run `./samsung-tv-root doctor` instead. If Tizen
Studio is in a nonstandard location, pass `--sdb <SDB_PATH>` before the
subcommand.

### Configure the TV

```console
./samsung-tv-root config init
./samsung-tv-root config check
```

Edit the generated profile with `<TV_IP>`, `<TV_MODEL>`, and `<TV_FIRMWARE>`. Keep
the real values out of git.

### Verify and root

```console
./samsung-tv-root preflight qn90f <TV_IP>
./samsung-tv-root qn90f root <TV_IP> --command 'id; uname -a'
```

Use `qn90b` for the older model. The generic archive route, when the target
supports it:

```console
./samsung-tv-root archive-root probe <TV_IP>
./samsung-tv-root archive-root root <TV_IP> --command 'id'
```

`probe` is not a read-only check: it performs root acquisition, identity
verification, and cleanup. A reachable TV API does not prove SDB is available. If
the timing probe succeeds but the callback times out, the injection ran and the
remaining failure is staging, script execution, or the reverse callback path.

### Keep root available

```console
./samsung-tv-root daemon run
./samsung-tv-root status
./samsung-tv-root root acquire <PROFILE>
./samsung-tv-root execute <PROFILE> 'id; uname -a'
```

The controller reacquires root after TV lifecycle events. It does not install a
TV-side boot service. Option C is the path to that.

### Useful TV controls

```console
./samsung-tv-root source list <PROFILE>
./samsung-tv-root source select <PROFILE> HDMI2
./samsung-tv-root display picture-off <PROFILE>
./samsung-tv-root display wake <PROFILE>
./samsung-tv-root screenshot <PROFILE> frame.png
./samsung-tv-root overlay message <PROFILE> 'Hello' --seconds 5
```

Capabilities differ by model and firmware. Run `capabilities <PROFILE>` and trust
the result for that exact target, not the series name.

## Failure modes

| Symptom | Likely cause | Safe next step |
| --- | --- | --- |
| `sdb devices` is empty or `sdb shell` prints `closed` | Developer Mode, host address, or SDB generation mismatch. A closed shell is expected on some retail firmware. | Re-check Developer Mode and the controller address. Use the preflight timing probe; do not conclude the exploit is impossible from a closed shell. |
| Preflight timing probe fails | The package-name injection did not execute or the host path is blocked. | Fix SDB/network first. Do not jump to an external box. |
| Root works but Home still opens Tizen | Root is control, not a launcher. | This is expected. Continue to B/C; install a real app and take over Home. |
| OTA lands or new apps appear | OTA block was runtime-only or DNS fell back to public resolvers. | Reapply the block, persist it across reboot, lock DNS, re-check. Do not abandon the on-TV path. |
| Root disappears after a reboot | Root is volatile; no TV-side boot service exists yet. | Reacquire with the controller, then plan C. Do not replace the TV with another box. |
| Certificate rejected or app will not install | Certificate is not bound to the TV's real device identity, or Tizen 9 packaging needs `pkgcmd`. | Rebind the certificate; push the WGT, then install with `pkgcmd -i -t wgt -p`. |
| Flashing "Not Available" after debloat | Home/Art packages were disabled. | Full reboot first; restore the package state. Prefer controlling Art over disabling it. |
| Two root sessions fight or the agent dies | More than one agent was injected in the same boot. | Keep one agent per boot. Exit the old session before re-injecting or changing UEP. |
| A Mac browser mock looks like the new Home | A dashboard/report is not an on-TV app. | Install and verify a real WGT on the TV. Do not present the mock as done. |
| An agent proposes Apple TV / HDMI box as the OS | The pivot anti-pattern. | Stop. Re-read the persistence rule. Return to A → B → C. |
| Art changes do not stick after reboot | Art package state or mat/backlight settings were not persisted. | Reapply through the Art path; verify after a cold boot; keep original assets. |

## Checklist stubs

Use these as prompts, not as a substitute for reading the linked documents.

- [ ] Record exact `<TV_MODEL>` and `<TV_FIRMWARE>`.
- [ ] Enable Developer Mode and confirm `sdb` sees only the intended TV.
- [ ] Snapshot package list and original Art assets.
- [ ] Choose A, B, or C and write the success condition for this session.
- [ ] Verify root, Home, and Art after a cold boot, not only after a warm restart.
- [ ] Re-check OTA block and DNS fallback after 24 hours.
- [ ] Confirm no private values entered git.

Detailed stubs: [docs/CHECKLISTS.md](docs/CHECKLISTS.md).

## Documentation

- [Options A/B/C](docs/OPTIONS.md) — mechanisms, trade-offs, and exit criteria.
- [Learnings](docs/LEARNINGS.md) — OTA, packaging, Art, and the pivot anti-pattern.
- [Checklists](docs/CHECKLISTS.md) — light pre-flight and verification stubs.
- [Documentation index](docs/README.md) — upstream end-user guides and research notes.
- [Quick start](QUICKSTART.md) — host toolchain setup.

## License

This fork keeps the upstream [Unlicense](LICENSE). The LICENSE file is unchanged.
Upstream credit and public references are listed above.
