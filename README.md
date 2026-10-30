# Jibon's Notes patches

Morphe patches that make Samsung Notes run on Android phones and tablets that are not made by Samsung. I made them for my Redmi Pad 2 running the crDroid GSI on Android 16, and every patch below was tested there with Samsung Notes 4.4.48.15 (arm64 APK).

Add these patches to Morphe: https://morphe.software/add-source?github=jibon-k/notes-morphe-patches

## Install

1. Open the link above on your phone or tablet with Morphe installed, or add `jibon-k/notes-morphe-patches` as a patch source in Morphe Manager.
2. Get the Samsung Notes APK, version 4.4.48.15 for arm64. Other versions may work, but the patches were made from this one.
3. In Morphe Manager, pick the APK from storage and keep the default patches. Turn on Morphe's own Clone app patch as well if you want to keep the real Samsung Notes installed next to it.
4. Install the patched APK and open it once. It starts in the note list with finger drawing on.

## What the patches do

These three are always needed, because the app doesn't install or run without them.

| Patch | What it does |
| --- | --- |
| Samsung framework compatibility | Adds stand-ins for One UI framework classes and sends about 2,000 calls to One UI only methods and fields to them instead. |
| Manifest compatibility | Makes the One UI libraries optional so the install doesn't fail, gives the foreground services a type because Android 14 and later crash without one, and drops `multiArch`. |
| Crash fixes | Reports no iris scanner and finds the pen settings layouts and colour palettes after a package rename, which the colour picker needs. |

The rest are optional.

| Patch | Default | What it does |
| --- | --- | --- |
| Treat as pen device | on | Turns on the pen toolbar and hover features for an active pen that is not an S Pen. Leave it off if you don't have an active pen. |
| Finger drawing | on | Turns off "S Pen only" once, so fingers can draw and scroll. |
| Pen button actions | on | The pen's buttons run actions on single, double and long press, set in Settings > Pen button actions. Learn buttons in that screen records any pen's two buttons, so pens that send other keys than the Redmi Smart Pen work as well. |
| Backup to folder | on | Adds a Backup row to the side menu that saves changed notes as `.sdocx` files in a folder you choose. Import from backup brings back the notes the app doesn't have and leaves the rest alone. |
| Folder sync | off | Experimental sync with your other devices through a shared folder, for instance one synced by Syncthing. New notes, edits and deletions sync when you tap Sync now and when the app opens. Needs Backup to folder. |
| Hide Samsung account features | on | Hides Samsung Cloud, sign-in prompts, sharing, Galaxy AI and Lock, because they only work in an app signed by Samsung. |
| Turn off Samsung cloud components | on | Turns off the account, cloud, push and co-editing services that would only run in the background. |
| Hide camera | off | Removes Insert > Camera. |
| Branding | on | Names the app "Notes", uses my pen logo as the launcher icon and a red accent instead of Samsung's blue. The options change the name and the accent colours, and an empty value keeps Samsung's. |

<!-- PATCHES_START -->
> **[v1.0.0](https://github.com/jibon-k/notes-morphe-patches/releases/tag/v1.0.0)**&nbsp;&nbsp;•&nbsp;&nbsp;`main`&nbsp;&nbsp;•&nbsp;&nbsp;12 patches total
<details open>
<summary>📦 Samsung Notes&nbsp;&nbsp;•&nbsp;&nbsp;12 patches</summary>
<br>

**🎯 Supported versions:**

| 4.4.48.15 |
| :---: |

| 💊&nbsp;Patch | 📜&nbsp;Description | ⚙️&nbsp;Options |
|----------|----------------|-----------|
| [Backup to folder](#backup-to-folder) | Adds a Backup row to the bottom of the side menu: backs up changed notes as .sdocx files to a folder you choose (any folder, including SD card or a synced folder). |  |
| [Branding](#branding) | Your own app name, the Notes pen logo as launcher icon, and an accent colour in place of Samsung's blue. | • App name<br>• Accent colour (light theme)<br>• Accent colour (dark theme) |
| [Crash fixes](#crash-fixes) | Reports no iris scanner, and finds the pen settings layouts and colour palettes after a package rename (otherwise the colour picker crashes). |  |
| [Finger drawing](#finger-drawing) | Turns off "S Pen only" by default (once, at first start), so fingers can draw and scroll. Without it, the app assumes an S Pen and ignores finger input in notes. |  |
| [Folder sync](#folder-sync) | Experimental. Syncs notes with other devices through a shared folder (for example one kept in sync by Syncthing): "Sync now" in the Backup menu, and automatically each time the app opens. |  |
| [Hide Samsung account features](#hide-samsung-account-features) | Hides Samsung Cloud sync, sign-in prompts, sharing, Galaxy AI and note locking. They need a Samsung-signed app; this matters mostly on Samsung phones, where the account exists. |  |
| [Hide camera](#hide-camera) | Removes Insert > Camera from the editor. |  |
| [Manifest compatibility](#manifest-compatibility) | Makes One UI-only libraries optional (otherwise the install fails), gives foreground services a type (Android 14+ crashes without one) and drops multiArch. |  |
| [Pen button actions](#pen-button-actions) | Lets the pen's two buttons run actions (pen, eraser, undo, colour picker, page up/down...) with single, double and long press. Set up in Settings > Pen button actions. Works with pens whose buttons send key presses (the Redmi Smart Pen sends Page Up / Page Down). | • Upper button key code<br>• Lower button key code<br>• Pen device name |
| [Samsung framework compatibility](#samsung-framework-compatibility) | Adds stand-ins for One UI framework classes and redirects calls to One UI-only methods and fields, so the app runs on non-Samsung Android. |  |
| [Treat as pen device](#treat-as-pen-device) | Turns on the app's pen features (pen toolbar, hover) on a device with an active pen that isn't a Samsung S Pen. Leave it off on devices without an active pen. |  |
| [Turn off Samsung cloud components](#turn-off-samsung-cloud-components) | Turns off the Samsung account, cloud sync, push and co-editing services and receivers. They can't work in a patched app and would only run in the background. |  |

</details>

<!-- PATCHES_END -->

## Options

| Patch | Option | Default |
| --- | --- | --- |
| Pen button actions | `upperKey`, `lowerKey` | 92 and 93 (Page Up and Page Down, as the Redmi Smart Pen sends) |
| Pen button actions | `deviceName` | `Redmi Smart Pen`. Use `@stylus` for any device that reports itself as a stylus, or leave it empty for any device. |
| Branding | `appName` | `Notes` |
| Branding | `accentLight`, `accentDark` | `#C8372A` and `#FF7A6B` |

Buttons learned in the app replace the pen options.

## Troubleshooting

- The pen only scrolls and never draws. Keyboard mode is on in the editor, so switch back to a pen in the toolbar.
- The pen buttons do nothing. Open Settings > Pen button actions, tap Learn buttons and press both buttons.
- "The backup folder was deleted or moved" or the same for the sync folder. Choose the folder again from the Backup row.
- A crash. Run `adb logcat -b crash -d` and open a bug report with the output.

## Patch on a PC

You need Java 21, `gh` logged in, Android build-tools 35 and apktool.

```sh
tools/patch.sh notes-4.4.48.15.apk
tools/patch.sh notes-4.4.48.15.apk -e "Folder sync" -e "Pen button actions" -O upperKey=24 -O lowerKey=25
PKG=com.example.notes tools/patch.sh notes-4.4.48.15.apk -e Branding -O appName="My Notes" -O accentLight="#2F5597"
```

The first command applies the default patches. Add `-e` or `-d` with a patch name to turn it on or off, and `-O key=value` right after a patch to set its option. `PKG` sets the package name, which is `com.jibon.samsungnotes.patched` by default.

The APK lands in `out/`, signed with `tools/test.keystore`. The key is created on the first run, and updates need the same key, so keep it.

## Build

- `./gradlew buildAndroid` builds `patches/build/libs/patches-*.mpp`. It needs a GitHub token with `read:packages`, for instance `GITHUB_ACTOR=jibon-k GITHUB_TOKEN=$(gh auth token) ./gradlew buildAndroid` after `gh auth refresh -s read:packages`.
- `tools/build-local.sh` builds `build/notes-patches.mpp` without Gradle, using the Morphe CLI jar.
- `tools/build-stubs.sh <notes.apk>` rebuilds `compat-stubs.mpe` from `stubs/smali/`.

Releases are made by `release.yml` from `feat:` and `fix:` commits, `main` for stable and `dev` for pre-releases.

## Author

Jibon Krishna Joy, [GitHub](https://github.com/jibon-k)

These patches are for use with Morphe, and they are not made by or affiliated with the Morphe project or Samsung. No Samsung code is included, because the patches change an APK you provide.
