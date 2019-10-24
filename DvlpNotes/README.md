# Developer Notes

## Overview

Source Configuration Management, Embedded Tool Chain, and what I did to make it work.


## AVR Cross Compiler

One stop shop that covers setting up SSH, Samba, Git, and the toolchain. 

[AVRCrossCompiler](./AVRCrossCompiler.md/)


## Visual Studio Code

I am starting to use VSC more, an editor works fine, but IntelliSense is nice. The other nice trick is that it can remote (SSH) into an x86_64bit Ubuntu machine (e.g., a containor or test bench computer) and use the toolchain on that computer. All in all it looks good to me.

https://code.visualstudio.com/remote-tutorials/ssh/getting-started

https://code.visualstudio.com/remote-tutorials/wsl/getting-started

Important note WSL with the Ubuntu install and the AVR toolchain will actually show the correct path when VSC does its remote trick. I suspect that any of the toolchains (from Debain) will work with this Ubuntu install and this VSC setup.
