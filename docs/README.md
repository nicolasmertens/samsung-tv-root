# Documentation

Start with [README.md](../README.md) and its persistence rule. The target is the
Samsung Frame itself; an external HDMI box is not a replacement OS.

## Project docs

- [Options A/B/C](OPTIONS.md): what each option proves, what it does not, and
  when to move up the ladder.
- [Learnings](LEARNINGS.md): OTA, packaging, Art, root-vs-Home, and the pivot
  anti-pattern.
- [Checklists](CHECKLISTS.md): light pre-flight and verification stubs. No
  private values.

## Host toolchain

These are inherited from upstream. They describe the host CLI, not the Home
takeover plan.

- [Quick start](../QUICKSTART.md): install the host tool, create a TV profile,
  run preflight, and open a root shell.
- [Host controller](user/CONTROLLER.md): configuration, lifecycle, service, and
  control commands.
- [Remote input](user/REMOTE_POLICY.md): opt-in observation, suppression, and
  remapping.

`archive-root` requires v0.0.9 or newer, or a source checkout. For older
standalone downloads, follow their bundled documentation or the upstream
[v0.0.8 README](https://github.com/chris-ritsen/samsung-tv-root/blob/v0.0.8/README.md).

## Research notes

These files preserve implementation, exploit reproduction, and reverse-
engineering details. They are not setup guides. Commands and observations may be
historical, incomplete, or specific to a source checkout and tested firmware.

- [Exploit chain](research-notes/EXPLOIT_CHAIN.md)
- [QN90B reproduction](research-notes/QN90B_REPRODUCTION.md)
- [QN90F reproduction](research-notes/QN90F_REPRODUCTION.md)
- [SWU AES extraction](research-notes/AES_EXTRACTION.md)
