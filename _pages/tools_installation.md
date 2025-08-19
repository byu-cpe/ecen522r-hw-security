---
layout: lab
toc: true
title: "Tools Installation"
order: 6
sidebar: true
under_construction: false
icon: fas fa-screwdriver-wrench
---

## GOWIN FPGA Tools

1. Go to <https://www.gowinsemi.com/en/support/download_eda/>.  You will need to create a free account and login. 
1. Click the "Software for Linux" tab, and download the *Gowin V1.9.11.03 Education (Linux x64)* software.
1. Unpack the downloaded archive to your home directory:

        mkdir -p ~/GOWIN
        tar -xvf Gowin_V1.9.11.03_Education_Linux.tar.gz

1. Disable the built-in libraries for the IDE:

        cd ~/GOWIN/IDE
        mkdir -p ./lib/disabled
        [ -f ./lib/libfreetype.so.6 ]   && mv ./lib/libfreetype.so.6   ./lib/disabled/
        [ -f ./lib/libfontconfig.so.1 ] && mv ./lib/libfontconfig.so.1 ./lib/disabled/
        [ -f ./lib/libstdc++.so.6 ] && mv ./lib/libstdc++.so.6 ./lib/disabled/
        [ -f ./lib/libgcc_s.so.1 ]  && mv ./lib/libgcc_s.so.1  ./lib/disabled/

1. Disable the built-in libraries for the programmer:

        cd ~/GOWIN/Programmer/bin
        mkdir -p disabled
        [ -f ./libz.so.1 ] && mv ./libz.so.1 disabled/

1. Run the Gowin IDE like so:

        export LD_LIBRARY_PATH=$HOME/GOWIN/IDE/lib
        ~/GOWIN/IDE/bin/gw_ide