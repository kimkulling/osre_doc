.. _osre_introduction_cpp:

Introduction
============


Build
============

Installation Prerequisites
--------------------------
At first you have to install the following packages:
 * git
 * CMake Version 3.10 or higher
 * For Windows: Visual-Studio 2017 or 2019
 * For Linux: gcc or clang

Build for Windows
-----------------
 * Open a command-prompt
 * Checkout the code via::
 
   > git clone https://github.com/kimkulling/osre.git
 * Navigate into your folder which contains the OSRE-Repository 
 * We are using a cmake-based build to generate the build files. to generate your project-files for your Build Environment via::
   
   > cmake CMakeLists.txt
   
   - If you want to use a different environment like eclipe or CLion you can generate them as well:
     > cmake CMakeLists.txt -G <Your IDE> 
   - You can get the list of generators via:
     > cmake --help
 * If you have select the VS-Generator open the solution osre.sln with Visual-Studio or build OSRE via::
   
   > cmake --build .
 
 * You will find the samples and tests at::
 
   > osre\bin\Debug
 
 * To run the samples you have to copy the dlls from SDL2 and assimp into your bin-folder::
   
   > cd osre\bin\debug
   > copy ..\..\contrib\assimp\bin\debug\*dll .
   > copy <SDL2-Folder>\libs\x64\*dll .
   
   - This is an open issue which will get fixed as soon as possible.

Build for Linux
---------------
 * Open a terminal
 * Checkout the code via::
 
    > git clone https://github.com/kimkulling/osre.git
 
 * Go into the folder of osre
 * Open the solution osre.sln with the VS or build OSRE via::
 
   > cmake --build .
 
 * You will find the samples and tests at::
 
   > ose/bin
