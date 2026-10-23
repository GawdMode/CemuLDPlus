# Cemu LD+ for Android

An **unofficial** build of [SSimco's Cemu for Android](https://github.com/SSimco/Cemu) made for **LEGO Dimensions**:

- **Lighting fix:** on the regular build, lighting is stuck in a square around your character and everything else
  goes dark. Here the whole level is lit, at 720p and (new in build 16) higher resolutions.
- **Toy Pad on the second screen** (for example the AYN Thor's bottom screen) with every character and vehicle
  ready to place, pictures, Toy Tags, franchise logos and search.
- **Better sound:** fixed sound filter, balanced left/right with a simple reverb, and an optional volume boost.

Everything else matches SSimco's Android Cemu, including the built-in emulated Toy Pad.

> This project used to be called **Cemu Light Fix**. Old links and the installed app keep working: new builds
> install as an update over Cemu Light Fix and keep your saves and settings.
>
> This is a community build. It is not made by, or affiliated with, SSimco or the Cemu team.
> Please don't report problems with this build to them. Use this repo's [Issues](../../issues) page instead.

## Download

**[Download the latest Cemu-LDPlus.apk](../../releases/latest/download/Cemu-LDPlus.apk)**

All builds are listed on the [Releases](../../releases) page.

## Install

1. Download the APK on your Android device and open it. If Android asks, allow installs from your browser or file manager.
2. It installs as a **separate app called "Cemu LD+"**, next to your regular Cemu. Your regular Cemu and its saves are not touched.
3. Open Cemu LD+ and add your games folder, the same way as in regular Cemu.

Requires a 64-bit (arm64) Android device running Android 11 or newer, the same as SSimco's build.

**Installed build 1 or 2?** Those two were signed with throwaway keys, so Android won't update them
("package conflicts with an existing package"). Copy your save out of Cemu LD+ first (see below,
copying to regular Cemu or your Downloads folder), uninstall Cemu LD+, then install the newest build and
copy the save back. Build 3 and later can all be installed as normal updates.

## Known issues

- The intro logos take about 45 seconds before you can skip. That's the game's own timed logo sequence, not
  loading (the game reads almost nothing from disk while they're shown), so it can't be shortened without
  patching the game itself. Copying your shader cache from regular Cemu (see below) removes extra delay on first runs.

Not a bug: side-quest characters in the Adventure Worlds (for example Hans Moleman in Springfield) only play a short
greeting sound and then speak in text. That's how the original game works too.

## Important: game profile settings (fixes stuttering, crunchy audio)

Cemu LD+ is a separate app, so it starts with its own default settings. With the defaults, LEGO Dimensions
audio can stutter and sound crunchy or rumbly, especially in the hub, even though the frame rate is fine.

On the game list, **long-press LEGO Dimensions → Edit game profile** and set:

- **CPU mode:** Single-core recompiler
- **Thread quantum:** 100000

These are tested on an AYN Thor. If you use a custom GPU driver in your regular Cemu, pick the same one here too.

## Too quiet? Use the volume boost

LEGO Dimensions mixes its sound very quietly, so even at full volume it can be hard to hear on a handheld. In
**Settings → Audio**, first make sure **TV volume** is at 100%, then raise **TV volume boost (Cemu LD+)**
(+3 to +18 dB). A soft limiter keeps loud moments from crackling. Start with +12 dB.

## Higher resolutions (1080p and up)

Build 16 adds support for higher resolutions with the community **Resolution** graphic pack (new, so reports are welcome):

1. Open **Graphic packs**. If LEGO Dimensions isn't listed, download the community graphic packs there first.
2. Open *LEGO Dimensions → Graphics → Resolution*, turn it on and pick a resolution, for example **1920x1080**.

Shadows are drawn at the higher resolution too, so they get sharper. Higher resolutions need more GPU power: if the
game slows down, try **1600x900**, or go back to **1280x720 (Default)**. If lighting looks wrong at a higher
resolution, please open an [Issue](../../issues) with your log.

## Toy Pad on the second screen (dual-screen devices)

On devices with a second screen (for example the AYN Thor's bottom screen), Cemu LD+ shows the emulated
LEGO Dimensions Toy Pad there while you play:

- The three pads with the figure on each spot, glowing in the colors the game sets (like the real pad's lights).
- **Every character and vehicle, ready to place.** No need to create figures first: tap any of them and it's made
  the first time you use it. Figures on the pad get a green check.
- **Franchise logos:** tap a logo to show just that franchise's characters and vehicles (each vehicle's three builds
  together), or **All** for everything A to Z. Logos with black lettering (Harry Potter, Midway, LEGO City Undercover)
  are shown in white so they stand out on the dark background.
- **Search:** tap the search box and type on the small keyboard that appears on the Toy Pad screen.
- When you rebuild a vehicle in the game, the pad shows the new build.
- Tap an empty spot, then a figure from your collection, to place it. Or tap a figure first, then a spot.
- Tap a figure on the pad for **Move** (then tap where it should go) or **Remove**.

It only appears when **emulated Toy Pad** is turned on in Cemu's settings. Your controller keeps controlling the game
while you use the touch screen.

**Pictures and background** (Settings → Emulated USB Devices → *Toy Pad on the second screen*):

- **Figure pictures:** the app doesn't include any game artwork. The first time a figure is shown, its picture is
  downloaded from the community [LEGO-Dimensions-NFC](https://github.com/skylandersNFC/LEGO-Dimensions-NFC) collection
  and kept in the `toypad/figures` folder inside the Cemu LD+ folder. Characters stand on a drawing of their
  Toy Tag from the [LEGO Dimensions Wiki](https://lego-dimensions.fandom.com/wiki/Toy_Tag), kept in `toypad/tags`.
  Franchise logos come from the [LegoToypad](https://github.com/harrysof/LegoToypad) project (`toypad/logos`). Without internet the figure's name is shown
  instead. To use your own picture for a figure, put it in that folder named by figure number (for example `1.png`
  for Batman, `1000.png` for the Police Car). You can turn the downloads off with the **Figure pictures** switch.
- **Background:** tap **Choose background picture** to use any picture on your device behind the Toy Pad, or keep the
  built-in star map.

## Moving your save from regular Cemu

Copying leaves the originals in place, so you won't lose anything.

1. In regular **Cemu**, open the menu on the game list and tap **Open Cemu folder**. Android's Files app opens.
2. Go to `mlc01` → `usr` → `save` and long-press the **`00050000`** folder.
3. Tap **⋮** → **Copy to…** → **☰** → **Cemu LD+**, then open `mlc01` → `usr` → `save` and tap **Copy**.
   If Cemu LD+ already has a `00050000` folder there, delete it first.
4. **Emulated Toy Pad figures:** go back to the main **Cemu** folder, long-press **`emulatedUSBDevices`**,
   then **⋮** → **Copy to…** → **Cemu LD+** (top level) → **Copy**.
5. If you installed the game's update or DLC through Cemu's **Title Manager**, install them again in Cemu LD+.
6. **Shader cache (less stutter):** in the main **Cemu** folder open `shaderCache` → `transferable`, long-press
   **`0005000010194200.bin`** (that's the US game; other regions use their own number), also select the file with the same number
   ending in `_vkpipeline.bin` if there is one, then **⋮** → **Copy to…** → **Cemu LD+** → `shaderCache` →
   `transferable` → **Copy**. Cemu LD+ then starts with every shader your regular Cemu has already built,
   instead of building them again while you play.

## Reporting a problem

Open an [Issue](../../issues) with:

- your device and Android version
- the game and region (US / EU)
- what went wrong (a screenshot helps a lot)
- your log file: in Cemu LD+, open the game list menu, tap **Share log file**, and attach it

## What the fix changes (technical)

Cemu only kept textures in sync when they start at the same memory address. LEGO Dimensions renders its
lighting and shadow data into textures that start partway inside a larger texture, so only the first piece
of the larger texture was ever filled in. That's why lighting only worked in a square around the player.

[`lightfix.patch`](lightfix.patch) adds tracking and copying for these "offset" textures. A pair is only
linked after checking with Cemu's address math that both memory layouts really line up. It also adds
depth↔color copies with offsets on Vulkan. It only links two textures after checking that their memory layouts really line up, and caches those checks.

### Higher resolutions (build 16 and later)

LEGO Dimensions draws its shadow maps as four 960x960 pieces stacked inside one 960x3840 texture. The community
Resolution pack scales the 960x960 pieces but has no rule for the 3840-tall texture, so the two ended up at different
sizes and couldn't be copied into each other: lighting broke above 720p. For LEGO Dimensions only, Cemu LD+ now gives
that texture the same scale as the pieces inside it, so the copies line up at any resolution.

### Sound filter fix (build 7 and later)

Cemu's per-voice low-pass filter subtracted its feedback term instead of adding it. That turned it into a
high-pass filter at roughly a third of the intended volume, so many voices and effects sounded thin and quiet.
LEGO Dimensions uses this filter on most of its sounds. It now works as a proper low-pass filter.

### Reverb (build 12 and later)

LEGO Dimensions sends many sounds to the Wii U's built-in reverb effect, which Cemu doesn't emulate. Cemu's
placeholder was meant to silence that effect but only silenced the left channel, so the right side came back
louder and dry. This build fixes the placeholder and replaces it with a simple room reverb, so the sound is
balanced again and those sounds get some space around them.

### Volume boost

An optional gain on the game's TV sound (Settings → Audio), with a soft limiter so loud moments don't crackle.

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
- Franchise logos and the franchise and vehicle family lists from [LegoToypad](https://github.com/harrysof/LegoToypad)
  by harrysof (logos downloaded on your device, not included here)
- Toy Tag drawings (downloaded on your device, not included here) from the
  [LEGO Dimensions Wiki](https://lego-dimensions.fandom.com/wiki/Toy_Tag) community

Cemu is licensed under the [Mozilla Public License 2.0](https://www.mozilla.org/MPL/2.0/), and the changes in
`lightfix.patch` are offered under the same license.

You need your own legally obtained copies of your games. This repo does not provide games, keys, or game files.
