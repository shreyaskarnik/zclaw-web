# zclaw Web Installer — Design

## Summary

A static web page that flashes zclaw firmware to ESP32 devices directly from the browser using ESP Web Tools and the Web Serial API. Hosted on GitHub Pages from a private repo.

## Architecture

**Approach:** Static site with ESP Web Tools (no framework, no build step for the site).

### Repository Structure

```
zclaw-web/
├── index.html              # Landing page with esp-web-tools install button
├── manifest.json           # ESP Web Tools firmware manifest
├── firmware/               # Merged .bin files (produced by CI)
│   ├── zclaw-esp32c3.bin
│   ├── zclaw-esp32s3.bin
│   └── zclaw-esp32c6.bin
├── .github/
│   └── workflows/
│       └── build-firmware.yml
└── README.md
```

### Landing Page

- Minimal dark theme (`#1a1a2e` background, light text)
- zclaw name + tagline: "The smallest AI assistant for ESP32"
- Single `<esp-web-install-button>` with auto chip detection
- Supported boards list (C3, S3, C6)
- Browser compatibility note (Chrome/Edge only)
- Footer linking to main zclaw repo
- All CSS inline in the HTML file

### Firmware Manifest

Standard ESP Web Tools manifest with one build entry per chip family (ESP32-C3, ESP32-S3, ESP32-C6). Each build references a single merged binary at offset 0x0.

### GitHub Actions CI

- **Trigger:** `workflow_dispatch` (manual) to start; can add release tag trigger later
- **Matrix:** [esp32c3, esp32s3, esp32c6]
- **Docker image:** `espressif/idf` (official Espressif image)
- **Steps per target:**
  1. Clone `tnm/zclaw`
  2. `idf.py set-target <target> && idf.py build`
  3. `esptool.py merge_bin` to combine bootloader + partition table + app
  4. Upload merged binary as artifact
- **Post-matrix:** Commit merged binaries to `firmware/` on `main` (or open PR)

### Improv Wi-Fi

The web side is ready for Improv out of the box — ESP Web Tools auto-detects it. The firmware side (adding Improv serial support to zclaw) is a separate future PR to `tnm/zclaw`. Not blocking for v1.

## Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Site framework | None (static HTML) | Single page, no build complexity |
| Flash library | ESP Web Tools | Handles chip detection, flashing, Improv — battle tested |
| Hosting | GitHub Pages | Free HTTPS, deploys from repo |
| Binary production | GitHub Actions CI | Automated, reproducible builds |
| Wi-Fi provisioning | Improv-ready, deferred | Web side works automatically; firmware change is separate |
| Merged binary | esptool.py merge_bin | Required for single-offset web flashing |
