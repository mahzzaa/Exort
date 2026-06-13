---
title: Compile & Upload
description: How manual and automatic compile/upload flows work, plus the common failure points.
order: 6
section: Compile & Upload
---

# Compile & Upload

Exort supports automatic and manual compile/upload flows through Arduino CLI.

## Select Board, FQBN, And Port

Before you build or upload, make sure the target is correct:

- board family
- exact board
- FQBN, if you need to select it manually
- serial port
- required board core
- required libraries

FQBN stands for fully qualified board name. It is the identifier Arduino CLI uses to compile for a specific board definition.

## Manual Compile

Manual compile is the safest way to validate the current sketch.

Use it when:

- you just edited code
- you changed libraries
- you switched boards
- you want compile feedback before upload

Typical compile failures:

- missing library
- wrong board selected
- wrong FQBN
- syntax error
- include path problem
- missing board core

## Manual Upload

Manual upload sends the compiled firmware to the connected board.

If upload fails, check:

- the selected port
- whether the board is powered
- whether another app is using the port
- whether the bootloader needs a reset or button press
- whether your USB cable supports data
- whether the correct board package is installed

## Automatic Compile And Upload

Auto compile/upload is useful when you want Exort to keep the edit loop moving.

It is best for small changes and quick iteration, but you still want to verify the board and port before letting it run.

## Before Compiling

Make sure you selected the correct board, port, project folder, board core, and required libraries.

## Recommended Workflow

1. Ask Exort Agent to review or generate code
2. Compile
3. Fix compile errors
4. Upload
5. Open Serial Monitor
6. Observe logs
7. Iterate

If the upload still fails after those checks, copy the exact error text and let the agent inspect it with the board name and the selected port.
