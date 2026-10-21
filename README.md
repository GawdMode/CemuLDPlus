# Cemu Light Fix for Android

An **unofficial** build of [SSimco's Cemu for Android](https://github.com/SSimco/Cemu) with a fix for
**LEGO Dimensions lighting being stuck in a square around the player**. On the regular build, everything
outside a small box around your character goes dark. With this build, the whole level is lit.

Everything else matches SSimco's Android Cemu, including the built-in emulated Toy Pad.

> This is a community build. It is not made by, or affiliated with, SSimco or the Cemu team.
> Please don't report problems with this build to them. Use this repo's [Issues](../../issues) page instead.

## Download

**[Download the latest Cemu-LightFix.apk](../../releases/latest/download/Cemu-LightFix.apk)**

All builds are listed on the [Releases](../../releases) page.

## Install

1. Download the APK on your Android device and open it. If Android asks, allow installs from your browser or file manager.
2. It installs as a **separate app called "Cemu Light Fix"**, next to your regular Cemu. Your regular Cemu and its saves are not touched.
3. Open Cemu Light Fix and add your games folder, the same way as in regular Cemu.

Requires a 64-bit (arm64) Android device running Android 11 or newer, the same as SSimco's build.

## Important: play at 720p

The fix works at the game's native **1280x720** resolution. In **Graphic packs**, set
*LEGO Dimensions → Resolution* to **1280x720 (Default)**, or leave it off. Higher resolutions may still
have broken or missing lighting. Support for those may come in a later build.

## Moving your save from regular Cemu

Copying leaves the originals in place, so you won't lose anything.

1. In regular **Cemu**, open the menu on the game list and tap **Open Cemu folder**. Android's Files app opens.
2. Go to `mlc01` → `usr` → `save` and long-press the **`00050000`** folder.
3. Tap **⋮** → **Copy to…** → **☰** → **Cemu Light Fix**, then open `mlc01` → `usr` → `save` and tap **Copy**.
   If Cemu Light Fix already has a `00050000` folder there, delete it first.
4. **Emulated Toy Pad figures:** go back to the main **Cemu** folder, long-press **`emulatedUSBDevices`**,
   then **⋮** → **Copy to…** → **Cemu Light Fix** (top level) → **Copy**.
5. If you installed the game's update or DLC through Cemu's **Title Manager**, install them again in Cemu Light Fix.

## Reporting a problem

Open an [Issue](../../issues) with:

- your device and Android version
- the game and region (US / EU)
- what went wrong (a screenshot helps a lot)
- your log file: in Cemu Light Fix, open the game list menu, tap **Share log file**, and attach it

## What the fix changes (technical)

Cemu only kept textures in sync when they start at the same memory address. LEGO Dimensions renders its
lighting and shadow data into textures that start partway inside a larger texture, so only the first piece
of the larger texture was ever filled in. That's why lighting only worked in a square around the player.

[`lightfix.patch`](lightfix.patch) adds tracking and copying for these "offset" textures. A pair is only
linked after checking with Cemu's address math that both memory layouts really line up. It also adds
depth↔color copies with offsets on Vulkan. The patch writes `[TexCreate]` and `[OffsetTexSync]` lines to
`log.txt` to help with diagnosing problems.

So far it has only been tested with LEGO Dimensions on one device (AYN Thor). Other games that use the same
technique might also benefit, and might also have new issues. Reports are welcome either way.

## How it's built

[`.github/workflows/build.yml`](.github/workflows/build.yml) downloads SSimco/Cemu at a fixed commit, applies
`lightfix.patch`, builds the APK on GitHub Actions, and publishes it to Releases. The full source for every
build is SSimco's code at that commit plus the patch in this repo.

## Credits and license

- [Cemu](https://github.com/cemu-project/Cemu) by the Cemu team
- [Android port](https://github.com/SSimco/Cemu) by SSimco
- Lighting fix by GawdMode

Cemu is licensed under the [Mozilla Public License 2.0](https://www.mozilla.org/MPL/2.0/), and the changes in
`lightfix.patch` are offered under the same license.

You need your own legally obtained copies of your games. This repo does not provide games, keys, or game files.
