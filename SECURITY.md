# Security policy

## Supported versions

Kestrel is rolling. Only the current `main` of each repository and the latest
published ISO/package set are supported. There are no backport branches.

## Reporting a vulnerability

Do **not** open a public issue for a vulnerability. Use GitHub's private
vulnerability reporting:

<https://github.com/kestrel-distro/kestrel/security/advisories/new>

Include the affected repository and version, the impact, and a minimal
reproduction. If the issue affects a fork, report it against the Kestrel
repository that carries the change, not upstream (unless upstream is also
affected and has its own process).

## Scope

In scope:

- The installer and live image (privilege boundaries, disk handling, LUKS key
  handling, the deferred-provisioning path).
- The `[kestrel]` package repository: signing, the published database, and the
  package build workflows.
- The Kestrel-owned commands under `kestrel/bin`, the systemd units and the
  `/etc` drop-ins.
- `scx_kestrel`, `scx-tools-kestrel` and the kernel hint ABI.

Out of scope: upstream Arch packages, AUR packages Kestrel only installs, and
upstream Hyprland/Quickshell/foot/Qt bugs that Kestrel does not modify.

## Packaging key

The `[kestrel]` repository is signed by a dedicated packaging key whose public
half ships in the distribution. The private key lives only in the `packages`
repository's CI secret and is never committed. If you believe the key was
exposed, report it privately immediately; the key is rotated by publishing a
new database signed by a new key and shipping that public key in an update.

## Secrets

Kestrel never asks for secrets through the package repository or the ISO. The
installer asks for the account password and, when encryption is enabled, uses it
for LUKS; the deferred path stages a throwaway key and re-keys on first boot.
Nothing else is collected.
