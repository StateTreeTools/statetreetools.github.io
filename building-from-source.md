---
title: Building from Source
---

# Building from Source

If you installed StateTree Tools from Fab, the precompiled binaries are ready to use and no setup is required. This page is for if you need to build the plugin from source. You may want to remove GAS support, or you may be trying to build an older version with special build instructions.

## Overview

The plugin targets Unreal Engine 5.1 and up. Building against **UE 5.6 or later** works with no additional setup — open the solution and build normally. Building against **UE 5.5 or earlier** requires a one-time environment setup before each build session because:

- **UE < 5.6** — UHT in those engine versions cannot evaluate `UE_VERSION_OLDER_THAN` macros inside struct definitions, so two headers that contain UE 5.6 delegate output fields must be swapped for compatibility versions before the build.
- **UE < 5.5** — A stub header (`StateTreeCompat55.h`) must be generated to provide a `FStateTreePropertyFunctionCommonBase` declaration that those engine versions lack.
- **UE 5.1–5.4** — The MSVC toolset must be pinned to a version that does not exceed the compiler cap for that engine (to avoid "compiler newer than expected" warnings).

The `buildenv.sh` script at the plugin root handles all of this.

---

## Setup for UE 5.5 and earlier

From a Git Bash (or any POSIX shell) terminal, run the setup script **before** opening Visual Studio or triggering a build:

```bash
./buildenv.sh setup 5.5   # replace 5.5 with your target engine version
```

This command:
1. Selects the highest installed MSVC toolset that does not exceed the cap for the target engine and writes it to `BuildConfiguration.xml`.
2. Swaps `STTCAnimation.h` and `STTGASGameplayTags.h` with their `.compat55` variants, which omit UE 5.6-only delegate output properties.
3. Generates `StateTreeCompat55.h` from the `.compat` source file (UE < 5.5 only), providing the `FStateTreePropertyFunctionCommonBase` stub that UHT requires.

Original file contents are preserved in `.bak` files next to each modified file.

When you are finished building, restore everything:

```bash
./buildenv.sh teardown
```

This restores all modified files from their `.bak` backups, regenerates an empty `BuildConfiguration.xml` if one did not exist before setup, and removes the generated `StateTreeCompat55.h` if it was installed.

> **Tip:** If a build session is interrupted and teardown was never run, you can always run `./buildenv.sh teardown` manually to restore the working tree.

---

## UE 5.6 and later

No setup is required. Open `FivePointSix.sln` (or whichever host project you are using) and build normally.

---

## Packaging a release

Use `package.sh` to build and package the plugin for distribution. It calls `buildenv.sh` internally, so no separate setup step is needed:

```bash
./package.sh 5.5   # target engine version, default 5.6
```

The packaged output is written to your configured packages directory.

[← Back to home](/)
