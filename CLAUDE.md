# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a ZMK firmware configuration for the Skinner39, a custom 39-key wireless split mechanical keyboard with trackball support. The keyboard features OLED displays, Bluetooth connectivity via MS88SF2 module, and is inspired by Blade Runner 2049 aesthetics.

## Key Technical Details

- **Microcontroller**: nRF52840 (via MS88SF2 wireless module)
- **Firmware**: ZMK (Zephyr-based Mechanical Keyboard firmware)
- **Left side is the main unit** - USB connections and primary control happen on the left half
- Supports Choc v1, v2, and MX switches
- 25mm/34mm trackball with PMW3610 sensor
- OLED displays on both halves using nice_view shield

## Build Commands

The project uses GitHub Actions for automated builds. To build locally:

```bash
# Build firmware for left half
west build -p -d build/left -b skinner39_left -- -DSHIELD="nice_view" -DZMK_CONFIG=/path/to/zmk-config-Skinner39/config

# Build firmware for right half  
west build -p -d build/right -b skinner39_right -- -DSHIELD="nice_view" -DZMK_CONFIG=/path/to/zmk-config-Skinner39/config

# Flash firmware (connect board via USB first)
west flash
```

## Project Structure

- `build.yaml` - Defines build targets for both keyboard halves with nice_view shield and studio-rpc-usb-uart snippet
- `config/west.yml` - Manages ZMK and custom driver dependencies (trackball, display, input behaviors)
- `config/skinner39.keymap` - Keyboard layout with 8 layers (default, number, symbol, function, mouse, scroll, snipe, custom)
- `config/skinner39.json` - Physical keyboard layout for visualization
- `config/boards/arm/skinner39/` - Board-specific hardware configurations and device tree files

## Common Development Tasks

When modifying the keyboard layout:
1. Edit `config/skinner39.keymap` to change key bindings or add new layers
2. Push changes to trigger GitHub Actions build, or build locally
3. Download firmware artifacts from GitHub Actions or use local builds
4. Flash the .uf2 files to each keyboard half

When updating board configuration:
1. Device tree files are in `config/boards/arm/skinner39/`
2. Pin assignments and hardware features are defined in `skinner39_left.dts` and `skinner39_right.dts`
3. Board-specific Kconfig options are in `Kconfig.board` and `Kconfig.defconfig`

## Important Notes

- The keyboard uses several custom ZMK modules for trackball and display support (see west.yml)
- Bluetooth pairing: Press BT_CLR + number key (as shown in symbol layer)
- Reset procedure: Use the physical reset button on the back of the case
- Battery safety is critical - the keyboard uses ultra-thin lithium batteries that require careful handling