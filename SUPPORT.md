# Support

Kestrel is early software. Use the issue tracker of the repository that owns the
problem:

| Problem | Repository |
|---|---|
| Desktop, commands, themes, terminal, shell | [`kestrel`](https://github.com/kestrel-distro/kestrel/issues) |
| Live image, installer, boot | [`iso`](https://github.com/kestrel-distro/iso/issues) |
| Kernel, scheduling, performance | [`kernel`](https://github.com/kestrel-distro/kernel/issues) |
| `scx_kestrel` | [`scx`](https://github.com/kestrel-distro/scx/issues) |
| Packages, signing, `[kestrel]` | [`packages`](https://github.com/kestrel-distro/packages/issues) |
| Documentation | [`docs`](https://github.com/kestrel-distro/docs/issues) |

Before opening an issue, read the manual in `kestrel/manual/` and the
documentation in `kestrel-distro/docs`, and search existing issues.

## What to include

- The Kestrel image name or `kestrel-version`, and the kernel (`uname -r`).
- Whether it is the live session or an installed system, and the hardware or VM.
- If the installer failed: the log from the live session
  (`/var/log/kestrel-install.log`) and the phase it stopped in.
- If the session is wrong: the relevant `~/.config/kestrel/` file, `hyprctl`
  output or `kestrel-bench` log.
- Exact steps to reproduce.

Do not attach private keys, passwords or the LUKS keyfile. Redact serials and
hostnames if they matter to you.
