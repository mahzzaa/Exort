---
title: Download & Install
description: Download Exort for Windows, macOS, or Linux and get the desktop app running.
order: 2
section: Getting Started
---

<script lang="ts">
  import { EXORT_GITHUB_LINK, EXORT_GITHUB_RELEASES_LINK } from "$lib/constant";
</script>

# Download & Install

Exort is a desktop-only Electron app. The main app lives in `packages/desktop`, while the website lives separately in `packages/web`.

## Windows

- Download the Windows installer or portable package from the release page
- If the installer is blocked, right-click and choose the Windows run/unblock flow
- Make sure any antivirus prompt is allowed if Windows quarantines the app

## macOS

- Download the `.dmg` build for Apple Silicon or Intel Macs
- Open the DMG and drag Exort into Applications
- If Gatekeeper warns about the app, allow it in Security & Privacy or the right-click open flow

## Linux

- Download the AppImage build
- Mark the file as executable if your desktop environment requires it
- If the app fails at startup on some systems, try the Linux hardware-acceleration fallback described in the README

## Install From Source

```bash
npm install
npm run dev
```

Use the source path when you want to run the app from the repository instead of a packaged release.

The release page is the quickest starting point:

- <a href={EXORT_GITHUB_RELEASES_LINK}>{EXORT_GITHUB_RELEASES_LINK}</a>
- <a href={EXORT_GITHUB_LINK}>{EXORT_GITHUB_LINK}</a>
