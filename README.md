# CLAIM: Awaits (Android APK)

This version uses **Kivy** (not Tkinter) and is designed to be packaged as an **Android APK** using **Buildozer**.

## Project files
- `main.py` — Kivy app (touch-friendly UI)
- `biomes.py` — biome monsters + loot
- `guardians.py` — guardians for all biomes
- `player_core.py` — player save/load + base stats (no GUI dependencies)
- `buildozer.spec` — APK build configuration (pinned Python/Kivy + Android settings)
- `.github/workflows/android-apk.yml` — GitHub Actions workflow to build APK

## Build the APK (recommended: Linux)
Buildozer works best on Linux (Ubuntu 20.04/22.04 is common).

## GitHub Actions (APK build in the cloud)
Commit/push to `main`, then go to **Actions** and run **Build Android APK (Buildozer)**.
The workflow installs Android SDK/NDK + Build-Tools (AIDL), patches `buildozer.spec` with the runner SDK paths for CI, builds a debug APK, and uploads it as an artifact.

### 1) Install system dependencies
```bash
sudo apt update
sudo apt install -y python3 python3-pip git zip unzip openjdk-17-jdk \
  build-essential autoconf libtool pkg-config \
  zlib1g-dev libncurses5-dev libncursesw5-dev libtinfo5 cmake
```

### 2) Install Buildozer + Cython
```bash
python3 -m pip install --user --upgrade pip
python3 -m pip install --user buildozer cython
```

Make sure `~/.local/bin` is on your PATH.

### 3) Build a debug APK
From the folder that contains `buildozer.spec`:
```bash
buildozer -v android debug
```

The first build downloads the Android SDK/NDK and can take a while.
When it finishes, your APK will be in:
`bin/`

### 4) Install to a phone (USB debugging)
```bash
buildozer android deploy run
```

## Saves
On Android the save file is stored in the app’s private storage (no special permissions needed).
