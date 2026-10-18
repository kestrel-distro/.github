<p align="center">
  <img src="https://raw.githubusercontent.com/kestrel-distro/.github/refs/heads/main/profile/kestrel-mark-512.png" alt="Kestrel" width="160">
</p>

<h1 align="center">Kestrel</h1>

<p align="center">
  A standalone Linux distribution that repacks Hyprland and Quickshell around a
  custom, efficiency-tuned kernel.
</p>

## What Kestrel is

Kestrel is its own Linux distribution, built around one shaped desktop and a
kernel tuned for it:

- **A custom kernel, not an off-the-shelf one.** `linux-kestrel` is built with
  Clang and ThinLTO for x86-64-v3, full preemption, a 1000 Hz tick, sched_ext
  (scx_bpfland), DAMON reclaim and NTSYNC, from the CachyOS-class recipe. The
  kernel repository builds and checks it in CI.
- **Hyprland and Quickshell built lean.** Kestrel ships its own Hyprland and
  Quickshell source packages, compiled with the fat trimmed, instead of the
  stock builds.
- **A desktop written as configuration.** Hyprland and the Quickshell shell run
  from versioned defaults under `default/`, so every session comes up the same
  way.
- **Installed by copy.** The live ISO boots a full Kestrel desktop and installs
  it by copying its root filesystem to disk, the casper and anaconda model, with
  no second round of package resolution.

## Repositories

- `kestrel`: the distribution payload, the packages it builds and installs, the
  Hyprland and Quickshell defaults, and the `kestrel` command line.
- `iso`: the live ISO builder that produces the bootable installer image.
- `kernel`: `linux-kestrel`, the custom efficiency-tuned kernel.
- `hyprland`: the lean Hyprland source package Kestrel ships.
- `quickshell`: the lean Quickshell source package Kestrel ships.
- `packages`: the binary package repository, a pacman `[kestrel]` overlay served
  over GitHub Pages.

## Efficiency and speed over everything

- **Heavy packages are built, not taken as-is.** The kernel, Hyprland and
  Quickshell are compiled from source with the unused paths trimmed, rather than
  installed prebuilt.
- **Latency first.** Full preemption, a 1000 Hz tick, sched_ext for the
  compositor and shell, and process priorities that keep the interactive stack
  ahead of the background.
- **Weight is treated as a cost.** The shell drops fonts it never renders; the
  kernel drops per-module BTF and other unused weight.
- **Nothing ships on a hunch.** CPU, footprint and desktop metrics are measured,
  and a change that does not move them does not ship.

## Status

Early. The live ISO builds, boots to the Kestrel desktop, and installs to disk
by copying the root filesystem. The custom kernel and the lean Hyprland and
Quickshell builds are in progress.
