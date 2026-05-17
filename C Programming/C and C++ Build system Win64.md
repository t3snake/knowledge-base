---
tags:
 - c_programming
 - programming
 - build
 - cmake
 - debug
---

## Minimal Build Setup - Win

Easy solution is to download the full Visual Studio IDE. Which takes lot of space and has too many slop features.

To use the lightweight version, we can just download the `VS Build Tools`.
For C, we require the build tools main packages and the `Desktop development with C++` option checked.
This downloads `MSVC`, `vpkg`, `cmake` and so on.

It also adds a `Developer Powershell for VS 2022` and also a developer command prompt.
Opening this gives access to the compiler command `cl`

To add it to path and have access to this easily: follow Handmade Hero Episode 1 by Casey [[Handmade Hero index]]

## Direct compile

If a project with solution is made in Visual Studio IDE, then a simple F5 works to compile and debug.
But if you want to maintain your own folder structure and not use solution then it is much less headache to manually use the `cl` option to compile in `Developer Powershell for VS`

See the main readme of [aoc25 - Github](https://github.com/t3snake/aoc25) for steps to make it run in Visual Studio.

Compile with debug info (PDB and O files):

`cl -Zi <filename.cpp>`

## CMake basics

This covers the basics of compiling using cmake.
Enough to compile codecrafter projects and allow debugging.

Following allows to build in a separate dir from source (which contains the `CMakeLists.txt` file)

`cmake -B <build_dir> -S <source_dir>`

For debug build use flag:

`-DCMAKE_BUILD_TYPE=Debug`

There is a generator concept - a config option that links with which compiler is used.
If the build was done in a different VS version and then the VS version is changed, then the following command needs to run. 
(some files from existing file would need to be deleted)

`cmake -G "Visual Studio 17 2022" -B <build_dir> -S <source_dir>`