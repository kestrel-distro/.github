<p align="center">
  <img src="https://raw.githubusercontent.com/kestrel-distro/.github/refs/heads/main/profile/kestrel-mark-512.png" alt="Kestrel" width="160">
</p>

<h1 align="center">Kestrel</h1>

<p align="center">
  A standalone Linux distribution built on Arch, with one shaped desktop and a
  kernel that carries a scheduling bridge for it.
</p>

<p align="center">
  <a href="https://github.com/kestrel-distro/kestrel">Payload</a> ·
  <a href="https://github.com/kestrel-distro/iso">ISO</a> ·
  <a href="https://github.com/kestrel-distro/kernel">Kernel</a> ·
  <a href="https://github.com/kestrel-distro/docs">Documentation</a>
</p>

## What Kestrel is

Kestrel is its own Linux distribution. It starts from Arch, then replaces the
parts that decide how a desktop feels instead of layering configuration on top
of a generic system:

- **Hyprland + Quickshell, built as one set.** Kestrel ships lean Hyprland and
  Quickshell source packages, plus the rebuilt shared-library closure
  (`aquamarine`, `hyprutils`, `hyprlang`, `hyprgraphics`, `hyprcursor`,
  `hyprwire`, and an explicit-sync `qt6-base`) compiled with the same
  `x86-64-v3`/ThinLTO flags.
- **foot is the terminal.** A Kestrel fork adds the per-task hint to `foot`,
  runs one shared `foot --server` in the `kestrel-term` slice, and launches
  every window through `kestrel-footclient` so the active palette follows the
  theme. Terminals run fish; internal scripts stay bash.
- **A kernel built for the desktop.** `linux-kestrel` is the kernel the live
  image boots and installs: Clang ThinLTO, `x86-64-v3`, full preemption, 1000 Hz,
  sched_ext, DAMON reclaim and NTSYNC, from the CachyOS-class recipe.
- **A scheduling bridge, not just a scheduler.** The kernel exposes a
  per-task hint (`sched_setattr`, flag `1ULL << 40`, value `0..1024`).
  `libkestrel-hint` sets and reads it, `kestrel-broker` places processes in
  resource-class cgroups, and `scx_kestrel` consumes the hint through
  `scx_bpf_task_hint()`.
- **Installed by copy.** The live ISO boots a complete Kestrel desktop and the
  installer copies that root filesystem to disk, the casper/anaconda model.
  There is no second definition of the system and no repository contacted during
  the install.

## How the pieces fit

```
Hyprland / Quickshell / foot / apps
        │  libkestrel-hint (sched_setattr hint)
        ▼
kestrel-broker ── cgroups (desktop, term, browser, background)
        │
        ▼
linux-kestrel ── per-task hint in sched_ext
        │
        ▼
scx_kestrel (scx_layered fork, baked layer set)  ← scx_loader / scxctl
```

`sched_ext` uses `scx_bpfland` by default in the live session. `scx_kestrel`
is selectable at runtime and is the subject of the A/B in `kestrel-bench`.

## Repositories

| Repository | What it is |
|---|---|
| [`kestrel`](https://github.com/kestrel-distro/kestrel) | Distribution payload: `kestrel-*` commands, Hyprland/Quickshell/foot/fish defaults, themes, installer scripts, manual and tests. |
| [`iso`](https://github.com/kestrel-distro/iso) | Live ISO builder: archiso profile, installer and configurator, VM boot helper, signing and release tools. |
| [`kernel`](https://github.com/kestrel-distro/kernel) | `linux-kestrel` recipe, the hint patch, CI and the `bench/` harness. |
| [`desktop`](https://github.com/kestrel-distro/desktop) | `kestrel-desktop` metapackage and the matched Hyprland library closure. |
| [`foot`](https://github.com/kestrel-distro/foot) | foot fork with the Kestrel hint in the terminal's process tree. |
| [`hyprland`](https://github.com/kestrel-distro/hyprland) | Lean Hyprland source package. |
| [`quickshell`](https://github.com/kestrel-distro/quickshell) | Lean Quickshell source package, including the QML intercept fixes. |
| [`scx`](https://github.com/kestrel-distro/scx) | `scx_kestrel` (scx_layered fork) and the bake for its layer set. |
| [`scx-loader`](https://github.com/kestrel-distro/scx-loader) | Loader fork that knows `scx_kestrel`; packaged as `scx-tools-kestrel`. |
| [`qtbase`](https://github.com/kestrel-distro/qtbase) | Qt base branch with Wayland explicit sync (`linux-drm-syncobj-v1`). |
| [`packages`](https://github.com/kestrel-distro/packages) | Signed pacman `[kestrel]` overlay published to GitHub Pages. |
| [`docs`](https://github.com/kestrel-distro/docs) | Architecture, packaging, publishing and maintenance documentation. |
| [`.github`](https://github.com/kestrel-distro/.github) | This profile and the organization-wide community files. |

## Build and install

- **Try/install:** build an image with `iso/bin/kestrel-iso-make`, boot it with
  `iso/bin/kestrel-iso-boot`, and use **Install Kestrel** in the Super+D menu.
  Encryption is the default; free-space installs and deferred first-boot
  provisioning are supported.
- **Payload:** `kestrel/install/kestrel-base.packages`,
  `kestrel-aur.packages` and `kestrel-source.packages` describe everything the
  image contains. Source packages are built at ISO time from their repositories.
- **Updates:** rolling Arch. Kestrel's own packages come from `[kestrel]` and
  use distinct names (`linux-kestrel`, `hyprland-kestrel`,
  `quickshell-kestrel`, `scx-kestrel`, ...) that conflict with the Arch ones.

## Measuring

`kernel/bench/run` and `bench/compare` measure CPU/scheduler behaviour in the
VM. `kestrel-bench` (payload, dev-only) runs the bridge checks, the
`scx_bpfland` vs `scx_kestrel` A/B, a foot worker matrix and peak PSS sampling
for the shipped desktop apps. A change that does not move a metric does not
ship.

## Standards

- Changes ship with tests where the project can test them.
- English in commits, code and documentation.
- History is never rewritten.
- Everything runs on the VM, never on a maintainer's daily machine.

## Status

The live ISO builds, boots the Kestrel kernel and installs by copying the
root filesystem. The hint bridge (`linux-kestrel` + `libkestrel-hint` +
`scx_kestrel`) is implemented and exercised by `kestrel-bench`; the desktop,
foot/fish defaults and the themed toolchain are in place. Packaging and
documentation are still catching up with the implementation.

## Contributing and security

See [CONTRIBUTING.md](CONTRIBUTING.md), [SUPPORT.md](SUPPORT.md) and
[SECURITY.md](SECURITY.md). Maintainer documentation lives in
[`kestrel-distro/docs`](https://github.com/kestrel-distro/docs).
