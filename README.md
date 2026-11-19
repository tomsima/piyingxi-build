# Piyingxi - turning it into an app for students

You have a Java program. Students (Windows / Mac) need something they can double-click, with no Java to install.
This kit builds that: a **Windows installer (.exe)**, a **no-install Windows zip**, and **two Mac disk images (.dmg)**
(one for Apple-silicon Macs, one for older Intel Macs). Each contains its own copy of Java.

Windows and Mac apps can only be built on Windows / Mac, so GitHub's free build computers do it for you.

## One-time setup (about 15 minutes)

1. Make a free account at github.com and create a **public** repository, e.g. `piyingxi`.
   (Public matters: students can then download from it without logging in. The source code is visible to everyone.)
2. If you have `flatlaf-3.5.4.jar` and `jlayer-1.0.1.4.jar`, put them in `libs/`. If not, the build downloads them.
3. In a terminal, from this folder (replace YOURNAME):

       git init -b main
       git add .
       git commit -m "Piyingxi"
       git remote add origin https://github.com/YOURNAME/piyingxi.git
       git push -u origin main

   (Use the command line rather than the website's upload button: the hidden `.github` folder must be included.)

## Publishing a version

       git tag v1.0.0
       git push --tags

Open the repository's **Actions** tab and wait (about 10-15 minutes). When it finishes, the repository's **Releases** page
has the downloads. Give students that page's link together with `HOW-TO-INSTALL.txt`.

To publish an update: change `src/Piyingxi.java`, commit, then tag `v1.0.1` (and so on).
You can also run it by hand: Actions > "Build apps" > Run workflow (the files are then under that run, not in Releases,
and downloading them needs a GitHub login).

## What students will see (please read)

The apps are **not signed**. Signing needs paid certificates (Apple's is about $99 a year; Windows ones cost money too).
So each student sees a one-time warning:

* Windows: "Windows protected your PC" -> More info -> Run anyway.
* Mac: the app is blocked the first time -> System Settings > Privacy & Security > Open Anyway.

`HOW-TO-INSTALL.txt` walks them through both. The Mac build is "ad hoc" signed by the script, which Apple-silicon Macs require
before they will run it at all.

## Things I could not test

I tested the packaging on Linux only: it compiles, builds the app, and the packaged app starts with its own bundled Java.
The Windows installer and the Mac disk images are built by GitHub and **I have not seen them run**. Before sending anything to a class,
install it yourself (or ask a friend) on one Windows and one Mac machine and check: it opens, a file loads, and the microphone works.
If the Mac asks for microphone permission and nothing happens, tell me what you see.

## Building on your own Windows or Mac machine instead

Install JDK 21 (Temurin). On Windows also install WiX Toolset 3.14 and use Git Bash. Then:

       ./packaging/build.sh 1.0.0 app-image
       ./packaging/build.sh 1.0.0 installer

Results appear in `dist/`.

## Notes

* Students' work lives next to their audio files (`.srt`, `.piyingxi`); settings and tone systems are in their user folder.
* FlatLaf (Apache 2.0) and JLayer (LGPL) are open-source; bundling them in the app is permitted.
* The workflow pins GitHub's runners (`macos-15`, `macos-15-intel`, `windows-latest`). GitHub retires old runner names from
  time to time; if a build ever fails with "runner not found", update the names in `.github/workflows/build.yml`.
