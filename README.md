# nrf52840 Blinky

A blinky project for the nrf52840 DK development board.

## Prerequisite

- [CMake version 3.20+](https://cmake.org/download/)
- [ARM GCC toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads) - arm-none-eabi-
- [Segger JLink](https://www.segger.com/downloads/jlink/) - Programmer

## Setup

Clone the repository and enter the folder then follow the instructions below:

    $ mkdir build
    $ cd build
    $ cmake -DCMAKE_TOOLCHAIN_FILE=../cmake/nrf52-toolchain.cmake ..
    $ make flash-blinky -j8 VERBOSE=1 

To erase the nrf52840:

    $ make erase-nrf
