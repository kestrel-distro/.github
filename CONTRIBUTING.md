# Contributing to Kestrel

Kestrel is a distribution assembled from a small number of repositories, each
with one job. Read the repository's README before changing it; the org profile
has the full map.

## Ground rules

- **Test in the VM.** Anything that installs, boots, changes the scheduler or
  changes the desktop runs in the QEMU VM (`iso/bin/kestrel-iso-boot`), never on
  a daily machine. The installer writes disks.
- **English** in commits, code, comments and documentation.
- **History is never rewritten.** Add commits; do not force-push shared
  branches.
- **Tests where they are possible.** Shell changes have `test/shell.d` (run in
  the guest with `test/vm`). Kernel changes go through the patch apply check and
  the `bench/` harness. Packaging changes go through the package workflow.
- **Do not add a dependency for something the standard library or the platform
  already does.**

## Where to change what

| Change | Repository |
|---|---|
| A command, default, theme, Hyprland/Quickshell/foot/fish config | `kestrel` |
| Installer, live environment, ISO build, VM boot | `iso` |
| Kernel recipe, hint ABI, scheduler bench | `kernel` |
| `scx_kestrel` layer set or code | `scx` |
| Loader/scheduler list, `scx-tools-kestrel` | `scx-loader` |
| Package list, signing and publishing | `packages` |
| Architecture, publishing, maintenance docs | `docs` |

## Building the pieces

```bash
# Payload checks (in the guest, not on the host)
kestrel test/vm cli shell acceptance

# ISO (network required; output in iso/release/)
iso/bin/kestrel-iso-make --local-source --no-boot-offer

# Boot the result in QEMU
iso/bin/kestrel-iso-boot iso/release/<image>.iso

# Kernel package
cd kernel/packaging/local && makepkg -s -f --noconfirm --nocheck

# Scheduler A/B and desktop memory
kestrel-bench            # inside the VM
```

The ISO builder mounts the sibling `kestrel` checkout with `--local-source`, and
the sibling `kernel` checkout for the benchmark harness. Keep the repositories
side by side.

## Commits and reviews

- Write a subject that says what changed and why, in the imperative.
- Keep unrelated changes in separate commits when practical.
- A pull request should describe how it was tested and which VM/ISO was used.
- Packaging changes that alter `[kestrel]` need a successful
  `validate-packages` run before publish.

## Licensing

Contributions are accepted under the license of the repository they touch
(MIT for the distribution, kernel and ISO; the upstream licenses for forks).
