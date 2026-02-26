# zclaw-web

Web-based firmware installer for [zclaw](https://github.com/tnm/zclaw), the smallest AI assistant for ESP32.

## How it works

Uses [ESP Web Tools](https://esphome.github.io/esp-web-tools/) and the Web Serial API to flash zclaw firmware directly from the browser. No drivers, no CLI tools required.

## Supported boards

- ESP32-C3 (recommended: Seeed XIAO ESP32-C3)
- ESP32-S3
- ESP32-C6

## Browser support

Chrome or Edge on desktop. Web Serial is not available on mobile or in Firefox/Safari.

## Building firmware

Firmware binaries are built automatically via GitHub Actions:

1. Go to Actions > Build Firmware
2. Click "Run workflow"
3. Optionally specify a zclaw git ref (branch, tag, or SHA)
4. CI builds for all three targets, merges binaries, and commits them to `firmware/`

## Local development

Just open `index.html` in Chrome. No build step needed.
