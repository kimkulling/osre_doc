.. _osre_introduction_cpp:

Introduction
============


Build
============

Installation Prerequisites
--------------------------
You need to install the following packages:

 * git
 * CMake Version 3.1 or higher
 * For Windows: Visual-Studio 2017 or 2019
 * For Linux: gcc or clang

Build for Windows
-----------------
 * Open a command-prompt
 * Checkout the code via 
   > git clone https://github.com/kimkulling/osre.git
 * Navigate into your folder with the OSRE-Repository 
 * Generate the project-siles via cmake:
   > cmake CMakeLists.txt
 * Open the solution osre.sln with the VS or build OSRE via
   > cmake --build .
 * You will find the samples and tests at 
   > ose\bin\Debug
   

Build for Linux
---------------
 * Open a terminal
 * Checkout the code via 
    > git clone https://github.com/kimkulling/osre.git
 * Go into the folder of osre
 * Open the solution osre.sln with the VS or build OSRE via
   > cmake --build .
 * You will find the samples and tests at 
   > ose/bin

