# SoundWave for Windows

SoundWave is a music player for Windows 10 and 11. It is the desktop version of the
[SoundWave Android app](https://github.com/Rajat-Solankii/SoundWaveApp).

This repository holds the Windows installers (under **Releases**) and `update.json`, the small
file the app reads to find out whether a newer version exists.

> **Status:** the first installer has not been published yet. Until a release appears under
> [Releases](https://github.com/Rajat-Solankii/SoundWavePc/releases), there is nothing to download.

## What it does

- Search and play from YouTube Music and JioSaavn, with a home feed, albums, playlists and artists
- Sign in to YouTube Music to see your own playlists and Liked Music
- Play music files from your PC, including lossless FLAC, WAV and ALAC
- Queue with drag-to-reorder, shuffle, repeat, and autoplay of similar songs
- Synced lyrics
- **Jam**: listen together in sync with friends, on the same Wi-Fi or over the internet, and
  together with the Android app
- 10-band equalizer, sleep timer, mini player, close-to-tray
- Windows media keys and the Windows media overlay

## Requirements

- Windows 10 or 11, 64-bit
- [VLC media player](https://www.videolan.org/vlc/) 3.x, 64-bit (SoundWave uses it to play audio)

## Install

1. Open [Releases](https://github.com/Rajat-Solankii/SoundWavePc/releases) and download the latest `SoundWave-x.y.z.msi`.
2. Run it. It installs for your Windows user only and needs no administrator rights.
3. Install VLC if you don't have it.

Windows may show a SmartScreen warning because the installer is not code-signed. Choose
**More info → Run anyway** if you trust this source.

## Updates

When SoundWave opens it reads
[`update.json`](https://raw.githubusercontent.com/Rajat-Solankii/SoundWavePc/main/update.json)
from this repository, once. If the file lists a newer version, SoundWave shows what changed and a
**Download** button that opens the new installer in your browser. Running that installer replaces
the old version; your library and settings stay.

- Nothing is downloaded or installed without you.
- **Skip this version** hides the prompt until the next version.
- You can turn the check off, or run it by hand, in **Settings → About and updates**.

### `update.json`

```json
{
  "version": "1.0.0",
  "versionCode": 1,
  "minSupportedVersionCode": 1,
  "releaseDate": "2026-11-11",
  "downloadUrl": "https://github.com/Rajat-Solankii/SoundWavePc/releases/download/v1.0.0/SoundWave-1.0.0.msi",
  "releasePage": "https://github.com/Rajat-Solankii/SoundWavePc/releases/tag/v1.0.0",
  "notes": ["First version of SoundWave for Windows."]
}
```

| Field | Meaning |
|---|---|
| `version` | Version name shown to people: `MAJOR.MINOR.PATCH`. |
| `versionCode` | A whole number that goes up by one with every release. **This is what the app compares**: a prompt appears when it is higher than the installed app's own code. |
| `minSupportedVersionCode` | Installed versions with a lower code must update (the prompt can't be postponed). Leave it alone unless an old version has to be retired. |
| `releaseDate` | `YYYY-MM-DD`, for reference. |
| `downloadUrl` | Direct link to the `.msi`. The **Download** button opens it. |
| `releasePage` | The GitHub release page, used if `downloadUrl` is empty. |
| `notes` | Short "what's new" lines shown in the prompt. |

## Version numbers

| Change | Example | Rule |
|---|---|---|
| Fixes only | `1.0.0` → `1.0.1` | raise PATCH |
| New features | `1.0.1` → `1.1.0` | raise MINOR, reset PATCH |
| Big redesign or breaking change | `1.1.0` → `2.0.0` | raise MAJOR, reset the rest |

`versionCode` goes up by exactly one every time, whatever kind of release it is
(`1.0.0` = 1, `1.0.1` = 2, `1.1.0` = 3, …). The Windows app's version is separate from the
Android app's version.

## Publishing a new version (for the maintainer)

The app's source lives in the `desktopApp` module of the SoundWave project.

1. In `desktopApp/build.gradle.kts`, raise `desktopVersionName` and `desktopVersionCode`.
2. Build the installer: `./gradlew :desktopApp:packageMsi`
   (output: `desktopApp/build/compose/binaries/main/msi/SoundWave-x.y.z.msi`).
3. Install it once on your own PC and check that it starts and plays.
4. Create a GitHub release in this repository with the tag `vx.y.z` and attach the `.msi`.
5. **Only after the release is live**, edit `update.json` here: set `version`, `versionCode`,
   `releaseDate`, both links and `notes`. Every installed copy sees the update the next time it opens.

Do step 5 last: if `update.json` points at an installer that isn't uploaded yet, the Download
button leads to a 404 page.
