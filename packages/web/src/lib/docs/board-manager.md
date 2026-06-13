---
title: Board Manager
description: Install board platforms and cores for Arduino CLI-compatible hardware.
order: 4
section: Board Manager
---

# Board Manager

Board Manager helps install and manage Arduino CLI board platforms and cores.

## What It Installs

A board core tells Arduino CLI how to compile and upload code for a specific family of boards.

Examples:

- Arduino AVR
- ESP32
- ESP8266
- RP2040
- STM32
- Teensy

## Supported Boards

Exort is intended to work with Arduino CLI-compatible platforms for:

- Arduino
- ESP32
- ESP8266
- RP2040
- STM32
- Teensy

Other Arduino CLI-compatible boards may also work if the correct core and toolchain are installed.

## When You Need To Install A Core

You usually need to install a core when:

- your board does not appear
- compile says board platform is missing
- upload tool is missing
- you are using a new board family

## Common Board And Port Problems

The most common issues are:

- the wrong board family is selected
- the wrong FQBN is selected
- the serial port changes after reconnecting the board
- the board needs a bootloader button press before upload
- another app already owns the port
- the USB cable is charge-only

## Good Recovery Steps

If a board is missing or a port looks wrong:

1. unplug and replug the board
2. try a different data USB cable
3. close other serial apps
4. install or update the board core
5. refresh the port list
6. try the upload again
