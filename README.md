# ScrEvolution 2.0 editor and player

## Notes for building in 2023 and after
I've had to patch up the build system so it could continue working. As of now, Macromedia no longer provides Flash globals. I've packaged playerglobal 11_6 with the repository and changed build settings to add it as a dependency. These fixes will probably not work with IntelliJ and FP 10.2-11.5 builds.
You will probably have to install Flash Player 11.6 in order to run ScrEvolution 2.0. It appears to crash newer versions of Flash Player. This means that, unfortunately, running ScrEvolution on IA64 and ARM64 Macs running macOS Catalina 10.15 and above is impossible at the moment. If anyone has a fix for this, feel free to contribute it!

## Overview

This is ScrEvolution 2.0, a block-based code editor and player based on Scratch 2.0. It is intended as an unofficial succesor to Scratch 2.0, since the Scratch Team has shifted development to Scratch 3.0 in 2019.
This project is licensed under the GNU GPLv2 (unfortunately).
Helpful contributions are welcome!

## Quick Start: Building and Debugging with Visual Studio Code

1. Build at least once using the Gradle instructions listed in the "Building" instructions below.
   * TL;DR: run `./gradlew build -Ptarget="11.6"` in a terminal (on Windows, replace `/` with `\` as usual).
   * This will download and unpack the necessary SDKs.
   * You may need to agree to a few licenses (press `y` then `enter`).
   * It may take quite a while and appear to hang at times. Watch disk and network activity to be sure.
2. Install [Visual Studio Code](https://code.visualstudio.com/).
3. Install the "ActionScript & MXML" extension (search for `@ext:as3` in the `Extensions` pane).
   * Reload VS Code when prompted.
4. Add the `scratch-flash` folder to the VS Code workspace.
5. Click "No SDK" then "Add more SDKs to this list...".
6. Browse to your home directory, then go into `.gradle`, then `gradleFx`. Choose `sdks` and close the dialog.
7. Your list of SDKs should now include something starting with "Apache Flex 4.15.0"; choose that.

You should now be able to build and debug using your usual Visual Studio Code hotkeys. The defaults are Ctrl+Shift+B
(or Cmd+Shift+B on Mac) to build and F5 to run.

Note that this will build a SWF which requires a very recent version of Flash, so the IDE build should only be used
for development and debugging. The Gradle builds (see below) are configured for compatibility with a broad range of
Flash versions.

Check `asconfig.json` for the configuration settings used by the IDE build.

## Building

The Scratch 2.0 build process now uses [Gradle](http://gradle.org/) to simplify the process of acquiring dependencies:
the necessary Flex SDKs will automatically be downloaded and cached for you. The [Gradle
wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html) is included in this repository, but you will
need a Java Runtime Environment or Java Development Kit in order to run Gradle; you can download either from Oracle's
[Java download page](http://www.oracle.com/technetwork/java/javase/downloads/index.html). That page also contains
guidance on whether to download the JRE or JDK.

There are two versions of the Scratch 2.0 editor that can be built from this repository. See the following table to
determine the appropriate command for each version. When building on Windows, replace `./gradlew` with `.\gradlew`.

Required Flash version | Features | Command
--- | --- | ---
11.6 or above | 3D-accelerated rendering | `./gradlew build -Ptarget="11.6"`
10.2 - 11.5 | Compatibility with older Flash (Linux, older OS X, etc.) | `./gradlew build -Ptarget="10.2"`

A successful build should look something like this (SDK download information omitted):

```sh
$ ./gradlew build -Ptarget="11.6"
Defining custom 'build' task when using the standard Gradle lifecycle plugins has been deprecated and is scheduled to be removed in Gradle 3.0
Target is: 11.6
Commit ID for scratch-flash is: e6df4f4
:copyresources
:compileFlex
WARNING: The -library-path option is being used internally by GradleFx. Alternative: specify the library as a 'merged' Gradle dependendency
:copytestresources
:test
Skipping tests since no tests exist
:build

BUILD SUCCESSFUL

Total time: 13.293 secs
```

Upon completion, you should find your new SWF in the `build` subdirectory.

```sh
$ ls -R build
build:
10.2  11.6

build/10.2:
ScratchFor10.2.swf

build/11.6:
Scratch.swf
```

Please note that the Scratch trademarks (including the Scratch name, logo, Scratch Cat, and Gobo) are property of MIT.
For use of these Marks, please see the [Scratch Trademark
Policy](http://wiki.scratch.mit.edu/wiki/Scratch_1.4_Source_Code#Scratch_Trademark_Policy).

## Debugging

Here are a few integrated development environments available with Flash debugging support:

* [Visual Studio Code](https://code.visualstudio.com/)
* [Intellij IDEA](http://www.jetbrains.com/idea/features/flex_ide.html)
* [Adobe Flash Builder](http://www.adobe.com/products/flash-builder.html)
* [FlashDevelop](http://www.flashdevelop.org/)
* [FDT for Eclipse](http://fdt.powerflasher.com/)

It may be difficult to configure your IDE to use Gradle's cached version of the Flex SDK. To debug the Scratch 2.0 SWF
with your own copy of the SDK you will need the [Flex SDK](http://flex.apache.org/) version 4.10+, and
[playerglobal.swc files](http://helpx.adobe.com/flash-player/kb/archived-flash-player-versions.html#playerglobal) for
Flash Player versions 10.2 and 11.6 added to the Flex SDK.

After downloading ``playerglobal11_6.swc`` and ``playerglobal10_2.swc``, move them to
``${FLEX_HOME}/frameworks/libs/player/${VERSION}/playerglobal.swc``. E.g., ``playerglobal11_6.swc`` should be located
at ``${FLEX_HOME}/frameworks/libs/player/11.6/playerglobal.swc``.

Consult your IDE's documentation to configure it for your newly-constructed copy of the Flex SDK.

If the source is building but the resulting .swf is producing runtime errors, your first course of action should be to
download version 4.11 of the Flex SDK and try targeting that. The Apache foundation maintains an
[installer](http://flex.apache.org/installer.html) that lets you select a variety of versions.
