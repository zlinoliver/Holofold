# Installing Holofold on Apple Vision Pro

Holofold is a 3DS emulator for Apple Vision Pro that shows the top screen in real stereoscopic 3D, with the bottom screen in its own touch window. It's a fork of [Bifold](https://github.com/Bifold-Emu/Bifold), built on the [Azahar](https://github.com/azahar-emu/azahar) emulator core, and runs on the headset itself.

## Requirements

- An Apple silicon Mac with Xcode 27 or later and the visionOS SDK
- Apple Vision Pro on visionOS 26 or later (the developer tests on an M5 headset with visionOS 27)
- An Apple ID (a free developer account works; sideloaded apps then need re-signing every 7 days)
- Your own 3DS games and their game files (see Game files)
- `brew install cmake ninja xcodegen` (CMake 3.28 or later) and Python 3
- About 5 GB free on the Mac. The first core build takes 10 to 20 minutes.
- Optional: a game controller (PS VR2 Sense, DualSense, DualShock 4, Xbox, Switch Pro and other MFi controllers)

## Game files

You need to supply your own game files, legally obtained from a copy you own. This repository contains none.

- Each game as a **decrypted** file from your own cartridge: `.cia`, `.3ds`, `.cci`, `.cxi` or `.3dsx`. Encrypted files are rejected.
- To check a file before copying it, run `python3 scripts/tools/ciacheck.py your-game.cia`. Every content should report `decrypted`, and the file `✅ usable`.
- Put the files in the Files app under *On My Apple Vision Pro > Holofold*, or copy one from the Mac with `./scripts/push-game.sh your-game.cia`. `.cia` files install themselves on the next launch.
- With no game of your own, a fresh build boots the bundled open-source homebrew StarCruiser7, so you can check the install first.

## Build

Start from a checkout of this repository. (The AVP Ports Index gives players a command that clones this repo at the exact commit it scanned, with submodules, so these steps should work from any existing checkout.)

1. Run `./scripts/bootstrap.sh`. It fetches Azahar 2126.2 and its submodules (about 1.5 GB) and applies this repo's patches.
2. Run `./scripts/build-core-ios.sh --sdk xros` to build the emulator core for the headset.
3. Copy `App/Config/Local.xcconfig.example` to `App/Config/Local.xcconfig` and set `DEVELOPMENT_TEAM` to your Team ID and `HOLOFOLD_BUNDLE_ID` to a bundle ID of your own (such as `com.yourname.holofold`; `com.example.holofold` won't sign). Leave `HOLOFOLD_INCREASED_MEMORY = NO` unless you have a paid developer account.
4. Run `xcodegen generate`, open `Holofold.xcodeproj` and select the **Holofold** scheme.

## Install on Apple Vision Pro

1. Pair the headset with Xcode: on the headset open *Settings > General > Remote Devices*, then pair it from *Window > Devices and Simulators* in Xcode.
2. Turn on Developer Mode on the headset (*Settings > Privacy & Security > Developer Mode*) and restart it.
3. Select your Vision Pro as the destination and Run.
4. With a free account, trust your certificate after the first install: *Settings > General > VPN & Device Management*, your Apple ID, **Trust**. Free-account builds expire after 7 days; Run again from Xcode, and your games and saves are kept.

## Controls and known issues

- Look at the bottom screen and pinch to tap; pinch and drag to slide. The control bar above the game window has Pause, 3D Depth, Curved, Bottom Screen and Exit Game.
- On a PS VR2 Sense controller the left stick is the Circle Pad and the right stick the D-pad. On other controllers the right stick is the C-Stick, and the controller's gyro stands in for the 3DS's motion controls. The PS and Xbox buttons are reserved by visionOS.
- Hold **Options** or **Menu** for about a second, or press the Digital Crown, to exit. The game auto-saves first and resumes from there next time.
- Early and experimental. Super Mario 3D Land plays at about 60 fps on M5 without JIT; Ocarina of Time 3D boots. A game can freeze after several minutes of play; see the README's troubleshooting section.
- The headset's screen recorder captures one eye only, so recordings look flat.
