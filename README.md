<h1 align="center">Indie Cross — Android Port</h1>

<p align="center"><b>Current version:</b> 0.1.0 &nbsp;·&nbsp; unofficial Android port, built on the <a href="https://github.com/jereidk/NightmareVision-Android-Support">NightmareVision Android backend</a></p>

<p align="center">
	Original PC mod repo:
	<a href="https://github.com/jereidk/Indie-Cross-Public">
		jereidk/Indie-Cross-Public
	</a>
</p>

> [!NOTE]
> APKs for this port are generated automatically via GitHub Actions on every push — check the [Actions tab](../../actions) for the latest build artifacts.

> [!WARNING]
> This port is under active development — `source/` and `assets/` currently still contain the Impostor Legacy Android template this repo was bootstrapped from, not Indie Cross's own content yet.

---

**FNF: Indie Cross** is a Friday Night Funkin' crossover mod bringing together Bendy and the Ink Machine, Cuphead, and Sans (Undertale) into one rhythm game experience.

This repository adapts the mod to run natively on **Android**, reusing the mobile backend and tooling originally built for the VS Impostor: Legacy Android port.

---

## Credits

**Indie Cross**
* Team 375 — mod creators

**Original IPs featured**
* [ninjamuffin99](https://twitter.com/ninja_muffin99), [PhantomArcade3K](https://twitter.com/phantomarcade3k), [Evilsk8r](https://twitter.com/evilsk8r), [Kawai Sprite](https://twitter.com/kawaisprite) — Friday Night Funkin'
* Joey Drew Studios — Bendy and the Ink Machine
* Studio MDHR — Cuphead
* Toby Fox — Undertale

**Android Port**
* jereidk — port maintainer

**NightmareVision Android backend (this port's engine base)**
* NMVTeam — engine authors
* FNF BR (LumiCoder) — original mobile port base
* StarNovaBR (StarNova) — mobile port contributions

**Engine upstream credits**
* ShadowMario and Co. — [Psych Engine](https://github.com/ShadowMario/FNF-PsychEngine)
* Nebula_Zorua — Modchart backend and the [Psych Engine fork](https://github.com/nebulazorua/exe-psych-fork) NMV is built off
* Rozebud — chart editor ([FPS Plus](https://github.com/ThatRozebudDude/FPS-Plus-Public))
* Codename Engine crew — camera rotation support
* FunkinCrew — [Lime](https://github.com/FunkinCrew/lime), [OpenFL](https://github.com/FunkinCrew/openfl), [hxcpp](https://github.com/FunkinCrew/hxcpp) forks
* MaybeMaru — [MoonChart](https://github.com/MaybeMaru/moonchart) and [flixel-animate](https://github.com/MaybeMaru/flixel-animate)

---

## How to compile locally

### Prerequisites

**All platforms:**
* [Haxe 4.3.6+](https://haxe.org/download/) and Haxelib 4.2.0+
* [Git](https://git-scm.com/downloads)

**Windows (PC build):**
* [VS Community Build Tools](https://aka.ms/vs/17/release/vs_BuildTools.exe) — install `Desktop development with C++`

**Android build:**
* [Android Studio](https://developer.android.com/studio) — for SDK/NDK setup

> [!NOTE]
> This project uses **hxpkg** to manage library versions. The expected versions are listed in `.hxpkg`.

---

### 1. Install libraries

```sh
haxelib git hxpkg https://github.com/ADA-Funni/hxpkg add-hmm-compatibility
haxelib run hxpkg install
```

<details>
<summary>Faster method (requires Rust)</summary>

```sh
haxelib git hxpkg https://github.com/ADA-Funni/hxpkg add-hmm-compatibility
haxelib run hxpkg to-hmm

cargo install --git https://github.com/ninjamuffin99/hmm-rs hmm-rs
hmm-rs clean
hmm-rs install

haxelib fixrepo

haxelib install hmm
haxelib remove grig.audio
haxelib run hmm reinstall grig.audio

haxelib fixrepo
```
</details>

---

### 2. Compile

#### Windows
```sh
haxelib run lime rebuild cpp -release
haxelib run lime build windows -release
```

#### Android

First, configure your SDK/NDK paths:
```sh
lime setup android
```
Provide absolute paths to your Android SDK, NDK, and JDK. Leave Apache Ant blank.

Then go to **Android Studio → SDK Manager → SDK Tools** and install:
- Android SDK Build-Tools
- NDK (Side by side) — r21e recommended
- Android SDK Platform-Tools

Then build:
```sh
haxelib run lime build android -release
```

To install directly on a connected device via USB debugging:
```sh
haxelib run lime test android -release
```

> [!TIP]
> You can add `-D ASSET_REDIRECT` to either build command so in-game assets update live as they're changed in the `assets` folder. Do **not** include this flag when making a release build.
