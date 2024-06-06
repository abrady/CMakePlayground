# Notes

* <https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html>

# TLDR

In Vulk I can't get my project to build with the static c runtime library (eg it uses MultiThreadedDebugDLL instead of MultiThreadedDebug), but vcpkg is using the static one
I'm trying to figure out how to set it to the static one.

Here's the shit I'm doing:

* "VCPKG_TARGET_TRIPLET": "x64-windows-static",
* "CMAKE_TOOLCHAIN_FILE": "${sourceDir}/vcpkg/scripts/buildsystems/vcpkg.cmake",
* "CMAKE_BUILD_TYPE": "Debug",
* "CMAKE_MSVC_RUNTIME_LIBRARY": "MultiThreadedDebug",
* "CMAKE_MSVC_RUNTIME_LIBRARY_DEFAULT": "MultiThreaded$<$<CONFIG:Debug>:Debug>",
* "CMAKE_INSTALL_PREFIX": "${sourceDir}/out/install"

according to the docs CMAKE_MSVC_RUNTIME_LIBRARY should work. let's find out.

it feels like this should happen during the configuration phase.

e.g. when I look at SimpleCppProject.vcxproj you see:

    Debug
    <RuntimeLibrary>MultiThreadedDebugDLL</RuntimeLibrary>
    ...
    Release
    <RuntimeLibrary>MultiThreadedDLL</RuntimeLibrary>

So what I want is for that to not have the DLL at the end:

    <RuntimeLibrary>MultiThreadedDebug</RuntimeLibrary>

Add this to CMakePresets.json:

    "CMAKE_MSVC_RUNTIME_LIBRARY": "MultiThreadedDebug"

Checking SimpleCppProject.vcxproj, Doesn't do shit!

<https://cmake.org/cmake/help/latest/variable/CMAKE_MSVC_RUNTIME_LIBRARY.html>

The docs say you need to do the following:

    cmake_policy(SET CMP0091 NEW)

son of a bitch.
