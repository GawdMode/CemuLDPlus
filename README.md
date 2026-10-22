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

**Installed build 1 or 2?** Those two were signed with throwaway keys, so Android won't update them
("package conflicts with an existing package"). Copy your save out of Cemu Light Fix first (see below,
copying to regular Cemu or your Downloads folder), uninstall Cemu Light Fix, then install the newest build and
copy the save back. Build 3 and later can all be installed as normal updates.

## Known issues

- Some NPC voice lines (for example the mini-quest characters) don't play, and their text may not appear until
  you tap. This also happens in regular Cemu, on Android and PC, and is still being looked into.

## Important: game profile settings (fixes stuttering, crunchy audio)

Cemu Light Fix is a separate app, so it starts with its own default settings. With the defaults, LEGO Dimensions
audio can stutter and sound crunchy or rumbly, especially in the hub, even though the frame rate is fine.

On the game list, **long-press LEGO Dimensions → Edit game profile** and set:

- **CPU mode:** Single-core recompiler
- **Thread quantum:** 100000

These are tested on an AYN Thor. If you use a custom GPU driver in your regular Cemu, pick the same one here too.

## Important: play at 720p

The fix works at the game's native **1280x720** resolution. In **Graphic packs**, set
*LEGO Dimensions → Resolution* to **1280x720 (Default)**, or leave it off. Higher resolutions may still
have broken or missing lighting. Support for those may come in a later build.

## Toy Pad on the second screen (dual-screen devices)

On devices with a second screen (for example the AYN Thor's bottom screen), Cemu Light Fix shows the emulated
LEGO Dimensions Toy Pad there while you play:

- The three pads with the figure on each spot, glowing in the colors the game sets (like the real pad's lights).
- Your figures as round character and vehicle pictures, split into **Characters** and **Vehicles and gadgets**.
  Figures that are on the pad get a green check.
- Tap an empty spot, then a figure from your collection, to place it. Or tap a figure first, then a spot.
- Tap a figure on the pad for **Move** (then tap where it should go) or **Remove**.

It only appears when **emulated Toy Pad** is turned on in Cemu's settings. Your controller keeps controlling the game
while you use the touch screen.

**Pictures and background** (Settings → Emulated USB Devices → *Toy Pad on the second screen*):

- **Figure pictures:** the app doesn't include any game artwork. The first time a figure is shown, its picture is
  downloaded from the community [LEGO-Dimensions-NFC](https://github.com/skylandersNFC/LEGO-Dimensions-NFC) collection
  and kept in the `toypad/figures` folder inside the Cemu Light Fix folder. Without internet the figure's name is shown
  instead. To use your own picture for a figure, put it in that folder named by figure number (for example `1.png`
  for Batman, `1000.png` for the Police Car). You can turn the downloads off with the **Figure pictures** switch.
- **Background:** tap **Choose background picture** to use any picture on your device behind the Toy Pad, or keep the
  built-in star map.

**Mark** button (top right): tap it at the moment something goes wrong, for example right when you talk to an NPC whose
voice doesn't play. It writes a marker into the log and records sound levels for the next 20 seconds, which makes the
log much easier to read.

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
depth↔color copies with offsets on Vulkan. It only links two textures after checking that their memory layouts really line up, and caches those checks.

### Sound filter fix (build 7 and later)

Cemu's per-voice low-pass filter subtracted its feedback term instead of adding it. That turned it into a
high-pass filter at roughly a third of the intended volume, so many voices and effects sounded thin and quiet.
LEGO Dimensions uses this filter on most of its sounds. It now works as a proper low-pass filter.

### Diagnostic logging

These builds also write extra lines to `log.txt` to help track down the missing NPC dialogue and the long intro:
opened files (`[FileOpen]`), missing files (`[FileMissing]`), how long sounds are set up and why they stop
(`[AXVoice]`, `[AXHist]`), sound levels after you tap **Mark** (`[Marker]`, `[AXLevel]`), unsupported system calls and
the game's own debug messages.

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
- Figure pictures (downloaded on your device, not included here) from the
  [LEGO-Dimensions-NFC](https://github.com/skylandersNFC/LEGO-Dimensions-NFC) community collection (Chteupnin's assets)

Cemu is licensed under the [Mozilla Public License 2.0](https://www.mozilla.org/MPL/2.0/), and the changes in
`lightfix.patch` are offered under the same license.

You need your own legally obtained copies of your games. This repo does not provide games, keys, or game files.
