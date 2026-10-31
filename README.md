# mikrotik-gpl

**A public mirror of the open-source code MikroTik provides for RouterOS — and a standing, automated request for the current version.**

MikroTik does not publish the GPL source code for RouterOS (the Linux kernel and other open-source components) on its website or anywhere else publicly downloadable. The only way to get it is to email MikroTik and ask. This repo automates the asking, and mirrors whatever comes back, so anyone can download it here instead.

## The short version

- **We ask, automatically.** A scheduled GitHub Action watches the current stable RouterOS release. For each new version, it emails MikroTik (`support@mikrotik.com`) requesting the GPL sources for that version. Each request is recorded as a [GitHub Release](https://github.com/tikoci/mikrotik-gpl/releases) named after the RouterOS version.
- **MikroTik does reply.** A human at MikroTik support answers, with a download link to a ZIP file.
- **But the reply is always the same file.** Every response so far links to the same ZIP: a disclosure dated **2022-01-04**, containing the Linux 5.6.3 kernel. That is the copy mirrored in this repo, under [`2025-03-19/`](2025-03-19/) (named for the date it was received here).
- **Nothing newer has ever been provided.** MikroTik support first said an updated disclosure was "being worked on" on 2025-03-20, and has repeated that since — most recently in October 2026 — with no ETA. So: the copy here is old, and there is no news. If that changes, the new disclosure will be checked in here.

| | |
|---|---|
| Requests | Automated, one per stable RouterOS release — see [Releases](https://github.com/tikoci/mikrotik-gpl/releases) |
| Responses | Yes — a MikroTik support reply with a ZIP link, every time so far |
| Newest source provided | 2022-01-04 disclosure (Linux kernel 5.6.3) |
| Current RouterOS sources | Requested for every release; never provided |

## What's in the disclosure

The mirrored disclosure is one folder, [`2025-03-19/`](2025-03-19/):

- `linux-5.6.3/` — the full Linux 5.6.3 kernel tree as patched by MikroTik, plus `linux-5.6.3.patch` (~25 MB) if you only want the diff.
- `configs/` — **the kernel configuration for every RouterOS architecture** (see below).
- Per-driver and per-package patch sets alongside their trees: network drivers (`atl1c`, `mvneta`, `mvpp2`, `vmxnet3`, `gianfar`, …), netfilter extras (`ipt_TARPIT`, `ipt_psd`, `imq`, `xt_tls`, …), and patches for `qemu-2.0.2`, `e2fsprogs-1.46.2`, `libpcap-0.8.3`, `tcpdump-3.8.3`, and `libelf-0.8.13`.

### Start with the kernel configs

If you only look at one thing, look at [`2025-03-19/configs/`](2025-03-19/configs/). It holds the actual kernel `.config` MikroTik builds with for each architecture — the fastest way to answer "is this driver, filesystem, or netfilter module even in RouterOS?" without reading kernel source:

| Config | Architectures |
|---|---|
| [x86_64.config](2025-03-19/configs/x86_64.config) | x86 hardware and CHR (the virtual/cloud router) |
| [arm.config](2025-03-19/configs/arm.config) · [arm64.config](2025-03-19/configs/arm64.config) | 32-bit and 64-bit ARM boards |
| [mips.config](2025-03-19/configs/mips.config) · [mmips.config](2025-03-19/configs/mmips.config) · [smips.config](2025-03-19/configs/smips.config) | MIPS boards (three variants) |
| [e500.config](2025-03-19/configs/e500.config) · [e500-smp.config](2025-03-19/configs/e500-smp.config) · [powerpc.config](2025-03-19/configs/powerpc.config) · [440.config](2025-03-19/configs/440.config) | PowerPC boards |
| [tile.config](2025-03-19/configs/tile.config) | Tile-Gx boards (CCR series) |

## What this is not

- **Not all of RouterOS.** Large parts of RouterOS are proprietary and not subject to disclosure. The kernel alone cannot rebuild RouterOS or add custom packages — most of the userland is closed.
- **Not current.** This is a 2022 snapshot. Today's RouterOS is years ahead; its sources have been requested for every release and not provided.
- **Not affiliated with MikroTik.** TIKOCI is an independent open-source community. We will keep asking, per release, and keep mirroring whatever is provided. The GPL and the other licenses involved do not require a reason to ask.

For older disclosures, see unrelated projects such as [robimarko/routeros-GPL](https://github.com/robimarko/routeros-GPL) (RouterOS 6.x era).

## Related TIKOCI projects

This repo covers the *open* parts of RouterOS. Two sister projects cover the rest of the picture:

- **[tikoci/m2ir](https://github.com/tikoci/m2ir)** — the *closed* parts, documented. A CC0 YAML reference for MikroTik's binary protocols and data formats — `nv::message` (M2), the WinBox protocol and CDB database, MNDP, the Dude object store, RoMON — built from binary analysis and experiments against real RouterOS, with sources and confidence recorded for each finding. The kernel sources here show what RouterOS is built *on*; m2ir documents how it *talks*.
- **[tikoci/quickchr](https://github.com/tikoci/quickchr)** — the *running* thing. A CLI and library that downloads, boots, and manages disposable MikroTik CHR (Cloud Hosted Router) VMs under QEMU, so RouterOS behavior can be tested against the real thing instead of guessed. Its `start --frozen` option holds a fresh VM at reset with a gdb stub attached, so a tool can inspect or patch guest RAM before RouterOS starts, then `continue` lets it boot — the technique [tikoci/enbeep](https://github.com/tikoci/enbeep) uses to make a virtual router beep. The `x86_64.config` above describes the kernel those VMs run.

More TIKOCI projects at [tikoci.dev](https://tikoci.dev).

## If you receive a newer disclosure

Email MikroTik yourself (`support@mikrotik.com`) — the more requests, the better. And if you receive anything newer than the 2022-01-04 disclosure, please open an issue here so it can be mirrored for everyone.

*Maintainers: the request automation, the support-ticket history, and the verification procedure live in [AGENTS.md](AGENTS.md), not here.*
