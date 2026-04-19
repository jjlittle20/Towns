# Towns

This repository contains the released source code for **Towns**, together with basic game data and a community-friendly local build setup.

Towns was originally developed over a decade ago and built a small but active community. The source was later released so people could study it, modify it, and create their own versions. The original repository explicitly states that it includes the full source code plus basic `.ini` and `.xml` game data, and that the code is released under GNU GPL v3. :contentReference[oaicite:0]{index=0}

## Status

This fork adds a working local Gradle setup so the game can be compiled and run more easily on a modern machine.

At the time of writing, the original source release is **not a complete standalone packaged game build**. The original README only mentions source code and basic game data, not a full graphics/audio/runtime asset pack. Some runtime assets may need to be provided from an existing Towns installation or another legitimate source. :contentReference[oaicite:1]{index=1}

## Repository contents

This fork currently contains:

- Original Towns source code
- Basic game data files (`.ini`, `.xml`)
- Gradle build setup
- Gradle wrapper
- Local run configuration for LWJGL 2 natives

The original repository states its contents as:

- Full source code of the game
- Basic game data (`.ini` files, `.xml` files) :contentReference[oaicite:2]{index=2}

## Requirements

Recommended:

- Windows
- Java 8 for best compatibility with this older LWJGL 2-era codebase
- Gradle Wrapper (`gradlew` / `gradlew.bat`)

This project uses legacy libraries such as LWJGL 2, JNA, Slick2D/OpenAL helpers, and a PNG decoder, which is consistent with the imports in the source code. :contentReference[oaicite:3]{index=3}

## Getting started

### 1. Clone your fork

```bash
git clone 
cd Towns
./gradlew build
./gradlew run