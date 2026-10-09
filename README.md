[![.github/workflows/build.yml](https://github.com/280Zo/charybdis-wireless-mini-zmk-firmware/actions/workflows/build.yml/badge.svg)](https://github.com/280Zo/charybdis-wireless-mini-zmk-firmware/actions/workflows/build.yml)

## Intro

This repository offers pre-configured ZMK firmware. It's designed for the [Wireless Charybdis keyboards](https://github.com/280Zo/charybdis-wireless-mini-3x6-build-guide?tab=readme-ov-file), but is easily adaptable to other platforms. It supports the latest stable ZMK release (v0.4.1) with full Bluetooth/USB split support, and uses the latest input listeners and processors for responsive pointer and scroll behavior.

## Overview & Usage

<!-- ![stacked keymap](keymap-drawer/stacked/stacked.svg)
![combos keymap](keymap-drawer/stacked/combos.svg) -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="keymap-drawer/stacked/stacked-combos-dark.png">
  <source media="(prefers-color-scheme: light)" srcset="keymap-drawer/stacked/stacked-combos-light.png">
  <img alt="stacked-combos keymap" src="keymap-drawer/stacked/stacked-combos-dark.png">
</picture>


To see all the layers check out the [full render](keymap-drawer/all_layers/all_layers.svg).


**Keyboard Layers**
| # | Layer      | Purpose                                                          |
| - | ---------- | ---------------------------------------------------------------- |
| 0 | **BASE**   | Standard typing with timeless home-row mods                      |
| 1 | **NUM**    | Combined digits + F-keys (tap for numbers, shift for functions)  |
| 2 | **NAV**    | Arrow keys, paging, TMUX navigation, mouse pointer               |
| 3 | **SYM**    | Symbols, punctuation, and a couple of helpers                    |
| 4 | **GAME**   | Gaming layer (just key-codes, no mods)                           |
| 5 | **EXTRAS** | Shortcuts, functions & snippets                                  |
| 6 | **MOUSE**  | Full mouse-key layer (pointer + wheel)                           |
| 7 | **SLOW**   | Low-speed pointer for precision pointer                          |
| 8 | **SCROLL** | Vertical/Horizontal scroll layer                                 |


**Home-Row Mods**
| Side                | Hold = Modifier              | Tap = Letter / Key  |
| ------------------- | ---------------------------- | ------------------- |
| Left                | **Gui / Alt / Shift / Ctrl** | `A S D F`           |
| Right               | **Ctrl / Shift / Alt / Gui** | `J K L ;`           |


**Combos**
| Trigger Keys              | Result                                  |
| ------------------------- | --------------------------------------  |
| `K17 + K18`                | **Caps Word** (one-shot words in CAPS) |
| `K25 + K26`                | **Left Mouse Button**                  |
| `K26 + K27`                | **Middle Mouse Button**                |
| `K27 + K28`                | **Right Mouse Button**                 |
| `K13 + K22`                | Toggle **MOUSE** layer                 |
| `K38 + K39` (thumb cluster)| Layer-swap **BASE / EXTRAS**           |


**Other Highlights**
- **Timeless-inspired home row mods:** Based on [urob's](https://github.com/urob/zmk-config#timeless-homerow-mods) work and configured on the BASE layer.
- **Thumb-scroll mode:** Hold the left-most thumb button (K36) while moving the trackball to turn motion into scroll.
- **Precision cursor mode:** Double-tap, then hold K36 to drop the pointer speed, release to return to normal speed.
- **K37 - Multifunction**
  - Tap: Left mouse click
  - Tap & Hold: Layer 3 (symbols) while the key is held
  - Double-Tap & Hold: holds the left mouse button
  - Tripple-Tap: Double mouse click
- **K38 - Multifunction**
  - Tap: Backspace
  - Hold: Layer 1 (numbers) while the key is held
  - Quick tap, then hold: Repeats Backspace instead of dropping into Layer 1
- **Bluetooth profile quick-swap:** Jump to the EXTRAS layer and tap the dedicated BT-select keys to pair or switch among up to four saved hosts (plus BT CLR to forget all).
- **PMW3610 low power trackball sensor driver:** Provided by [badjeff](https://github.com/badjeff/zmk-pmw3610-driver)
  - Patched to prevent cursor jump on wake
- **Hold-tap side-aware triggers:** Each HRM key only becomes a modifier if the opposite half is active, preventing accidental holds while one-handed.
- **Timeless HRM with selective exceptions:** Base home-row mods use the timeless-style `balanced + hold-trigger-on-release` setup, while A, I, and O (on a Colemak-DH layout) keep tap-preferred variants to reduce accidental mod triggers during fast rolls.
- **ZMK Studio:** Supported on the Bluetooth builds for quick keymap adjustments.


## Flash the Firmware

Download the firmware from the Releases page, then follow the steps below to flash it to your keyboard

1. Unzip the firmware bundle
2. One at a time, plug the devices into the computer through USB
3. Double press the reset button on the nice!nano
4. The keyboard will mount as a removable storage device
5. Copy the applicable uf2 file into the storage device
6. It will take a moment, then it will unmount and restart itself.
7. Repeat these steps for all devices.

> [!NOTE]
> If you are flashing the firmware for the first time, or switching away from a previous configuration, flash the reset firmware to all the devices first


## Customization

### Modify Key Mappings

**ZMK Studio**

[ZMK Studio](https://zmk.studio/) allows users to update functionality during runtime. It is supported on the Bluetooth builds. For more details on how to use ZMK Studio, refer to the [ZMK documentation](https://zmk.dev/docs/features/studio).


**Edit Keymap Directly**

To change a key layout, choose a behavior you'd like to assign to a key, then choose a parameter code. This process is more clearly outlined on ZMK's [Keymaps & Behaviors](https://zmk.dev/docs/features/keymaps) page. All keycodes are documented [here](https://zmk.dev/docs/codes).

Modify the [miryoku_colemak_dh.keymap](config/keymaps/miryoku_colemak_dh.keymap) or one of the behaviors, combos, or macros in the [keymap_features](config/keymap_features) folder, then follow the instructions below to build and flash the firmware to your keyboard.

### Modifying Trackball Behavior

The trackball uses ZMK's modular input processor system, making it easy to adjust pointer behavior to your liking. All trackball-related configurations and input processors are conveniently grouped in the [charybdis_pointer.dtsi](config/trackball/charybdis_pointer.dtsi) file. Modify this file to customize tracking speed, acceleration, scrolling behavior, etc. Then rebuild your firmware.

### Modify Build Format Selection

Build formats are also selected in [build.yaml](build.yaml).

To change which firmware families are built:

1. Open [build.yaml](build.yaml)
2. Find the build entry or entries you want to keep
3. Comment out or remove the entries you do not want

The main build families are:

- `bt`: Bluetooth split builds

### Modify Keymap Selection

Keymaps live in [config/keymaps](config/keymaps) and are selected in [build.yaml](build.yaml).

To change which keymaps are built:

1. Open [build.yaml](build.yaml)
2. Find the `keymap:` list under the build entry you have picked
3. Keep the keymaps you want and comment out or remove the others

### Build the Firmware

To build the firmware follow either of the build processes below:

**Local Build - Single Command**

1. Clone this repo
2. Update the config files to match your use case
3. Follow the instructions in the [local-build README](local-build/README.md)
4. Firmwares will be available in the firmwares folder

**Pipeline Build - GitHub Actions**

1. Fork this repo
2. Update the config files to match your use case
3. Push changes and confirm the workflows are running
4. Firmwares will be available in the action artifacts


## Credits

- [badjeff](https://github.com/badjeff) for the PMW3610 ZMK driver used as the basis for the trackball sensor integration
- [eigatech](https://github.com/eigatech) for useful reference patterns around split trackball/input-listener integration
- [nickcoutsos](https://github.com/nickcoutsos/keymap-editor) for the browser-based keymap editor workflow
- [caksoylar](https://github.com/caksoylar/keymap-drawer) for the keymap rendering workflow and physical layout conversion tooling
- [urob](https://github.com/urob/zmk-config#timeless-homerow-mods) for the timeless home-row mod approach this keymap builds on and the stacked layer SVG inspiration
