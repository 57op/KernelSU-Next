# KernelSU-Next Standalone SUSFS

A KernelSU-Next fork with SUSFS built-in as a standalone module — **no kernel source patching required**.

## What is this?

SUSFS (SUS_FS) is a kernel module for hiding modifications from detection. Traditionally, adding SUSFS meant manually patching your kernel source with dozens of hunks across multiple files. This fork integrates SUSFS directly into KernelSU-Next as a self-contained driver under `kernel/susfs/`. You get full SUSFS functionality from a single `curl | bash` setup.

```
curl -LSs https://raw.githubusercontent.com/Youffx/KernelSU-Next/legacy-susfs/kernel/setup.sh | bash -s legacy-susfs
```

## Required kernel config

Enable SUSFS and its features in your kernel config:

```
CONFIG_KSU_SUSFS=y
CONFIG_KSU_SUSFS_SUS_MOUNT=y
CONFIG_KSU_SUSFS_SUS_KSTAT=y
CONFIG_KSU_SUSFS_SPOOF_UNAME=y
CONFIG_KSU_SUSFS_HIDE_KSU_SUSFS_SYMBOLS=y
CONFIG_KSU_SUSFS_OPEN_REDIRECT=y
CONFIG_KSU_SUSFS_SUS_MAP=y
```

## How it works

The setup script integrates KernelSU-Next into your kernel source tree. All SUSFS code lives in `kernel/susfs/` and is compiled as part of the KernelSU driver — no separate patches, no `fs/stat.c` or `kernel/sys.c` modifications needed.

The build system (`kernel/Kbuild`) auto-detects SUSFS files and wires them in. Just set the config symbols above and build.

## SUSFS features

| Feature | Description |
|---------|-------------|
| SUS_MOUNT | Hide suspicious mounts from non-root processes |
| SUS_KSTAT | Spoof file stat (ino, dev, nlink, size, timestamps, blocks) |
| SPOOF_UNAME | Fake kernel release/version in uname |
| OPEN_REDIRECT | Redirect file opens to a decoy path |
| SUS_MAP | Hide suspicious mapped regions |
| HIDE_SYMBOLS | Strip SUSFS symbols from `/proc/kallsyms` |

## License

GPL-2.0-only (kernel code).
