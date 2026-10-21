# Cemu Light Fix (test build)

Experimental test build of [SSimco's Cemu Android port](https://github.com/SSimco/Cemu) with a fix attempt for
LEGO Dimensions lighting/shadows only working in a square around the player.

**Download:** grab `Cemu-LightFix.apk` from the newest entry under [Releases](../../releases).
It installs as a separate app called "Cemu Light Fix", next to the regular Cemu app.

## What the fix does

Cemu only synchronized textures that start at the same memory address. Some games render into
textures that begin partway inside a larger texture (for example shadow-map cascades stacked in one
tall atlas), so only the first region ever received data. `lightfix.patch` adds tracking and copying
for those "offset" textures (after checking the memory layouts really line up), including depth/color
copies with offsets on Vulkan. It also writes `[TexCreate]` and `[OffsetTexSync]` notes to `log.txt`
for diagnosis.

## How it's built

`.github/workflows/build.yml` downloads SSimco/Cemu at a fixed commit, applies `lightfix.patch`,
builds the APK and publishes it as a release.

Cemu is licensed under the Mozilla Public License 2.0; `lightfix.patch` is offered under the same license.
