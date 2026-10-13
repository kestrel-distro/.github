<p align="center">
  <img src="kestrel-mark-512.png" alt="Kestrel" width="160">
</p>

<h1 align="center">Kestrel</h1>

<p align="center">
  A custom Arch Linux distribution with the Hyprland compositor and the Quickshell desktop shell.
</p>

## What Kestrel is

Kestrel is an Arch Linux system built around a single, coherent desktop:

- **One shaped system, installed by copy.** The live ISO boots a full Kestrel
  desktop and installs it by copying its root filesystem to disk, the casper
  and anaconda model, with no second round of package resolution.
- **A desktop written as configuration, not assembled by hand.** Hyprland and
  the Quickshell shell run from versioned defaults under `default/`, so every
  session comes up the same way.
- **Rolling and auditable.** Updates come from `pacman -Syu` against the
  upstream Arch repositories, and every package built outside them is pinned
  and verified before it is installed.
- **Built to measure.** Scheduler, memory reclaim and desktop changes are
  benchmarked before they ship, not assumed.

## Repositories

- `kestrel`: the distribution payload, the packages it installs, the Hyprland
  and Quickshell defaults, and the `kestrel` command line.
- `iso`: the live ISO builder, based on archiso, that produces the bootable
  installer image.
- `kernel`: an experimental optimized kernel, built from the CachyOS-class
  recipe for x86-64-v3.

## Standards

- Changes ship with tests.
- English in commits, code and documentation.
- History is never rewritten.

## Status

Early. The live ISO builds, boots to the Kestrel desktop, and installs to disk
by copying the root filesystem. Rebranding is complete; the experimental
kernel is in progress.
