---
title: Requirements
description: What Exort needs installed before you can compile, upload, and use the agent.
order: 2
section: Requirements
---

# Requirements

Exort depends on two core runtime pieces:

- Arduino CLI for board packages, compile, and upload flows
- OpenCode or an OpenCode-compatible provider setup for the agent

## Arduino CLI

Arduino CLI is the tool Exort uses to discover boards, install board packages, compile firmware, and upload to devices.

You will usually need it installed even if Exort manages it in the background, because board packages and upload tools still come from the Arduino ecosystem.

## OpenCode And Providers

Exort uses OpenCode for AI workspace access.

In the app, this usually means:

- choosing a provider
- adding an API key or signing in
- selecting a model
- confirming the workspace can talk to the agent

If you already use an OpenCode-compatible provider setup, you can bring that into Exort instead of starting from scratch.

## Optional But Common

- USB drivers for your board
- Board platforms or cores for the target hardware
- A stable data USB cable
- Source development tools if you are running Exort from the repo

If something is missing, open the Requirements view in Exort and install the needed component before you start compiling.
