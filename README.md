This project is free and open source, and licensed under the GNU General Public License v3 license. You are free to use, modify, and distribute it, provided that any derivative works are also released under the same license.

How to use: Download the release from the releases page, extract the zip, double click the .exe, open your game, click refresh and then click inject. The default clickgui bind is RShift. LionClient currently supports: Vanilla, Forge, Badlion, Lunar. Video guide: https://youtu.be/H-YYx0dQu08

How to build (For developers): 
Prerequisites (one-time setup)
Windows 10/11 x64
JDK 8 (or any JDK) installed with JAVA_HOME pointing at its root — this is what supplies jni.h for the C++ build.
JDK 17+ installed somewhere (Eclipse Adoptium 17 or 21 is what the script probes for). build.bat finds it automatically and uses it to run Gradle, since Gradle 9 refuses older JDKs. You don't have to do anything; just install it.
CMake 3.20+ on PATH.
Visual Studio 2022 Build Tools with "Desktop development with C++". You need a working cl.exe / link.exe.
Gradle 8+ on PATH (no wrapper shipped). Or run gradle wrapper once inside client/.
Ninja (optional, recommended) — build.bat picks it over NMake automatically if found. scoop install ninja is the easy install.
Forge 1.8.9 installed and launched at least once via the Minecraft launcher. This populates %APPDATA%\.minecraft\libraries\ and ~\.gradle\caches\minecraft\ with the deobf jars and MCP mappings the Gradle build needs.
Build
Open an x64 Native Tools Command Prompt for VS 2022 (this is critical — the script expects to already be inside a vcvars x64 environment so cl, link, and nmake are on PATH). Then from the repo root:

build.bat
That's it. The script does the rest:

Sanity checks — verifies cmake, javac, JAVA_HOME, and the presence of jni.h.
Probes for a JDK 17+ under the standard Adoptium / Oracle install roots so Gradle has something it'll accept. Bails out with a clear error if nothing is found.
Builds client.jar via Gradle, using the JDK 17+ for the Gradle daemon while leaving your original JAVA_HOME (which can be JDK 8) untouched for the C++ step.
Builds the injector via CMake — picks Ninja if available, otherwise NMake. Output goes to build\.
Stages everything into dist\:
dist\LionInjectable.exe
dist\payload.dll
dist\client.jar
Ship those three files together — that's the release.

When things go wrong
Error	Fix
cmake not on PATH	Install CMake 3.20+ and add it to PATH.
javac not on PATH / JAVA_HOME is not set	Install a JDK and set JAVA_HOME to its install root.
%JAVA_HOME%\include\jni.h missing	Your JAVA_HOME points at a JRE. Repoint it at a JDK.
No JDK 17+ found	Install Adoptium JDK 21 (or 17). Default install path works automatically.
cl not found / link errors	You're not in an x64 Native Tools prompt. Open the right shortcut.
LWJGL ... not found / forgeBin not found (Gradle warnings)	Install Forge 1.8.9 and launch it once through the Minecraft launcher to populate the caches.
no gradlew.bat in client\ and gradle not on PATH	Install Gradle 8+ globally, or cd client && gradle wrapper once to generate a wrapper.
