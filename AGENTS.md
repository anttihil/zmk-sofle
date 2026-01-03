# Project Overview

This is a **ZMK firmware configuration** repository for the **Eyelash Sofle** keyboard, a split keyboard variant of the Sofle. It is built on top of the ZMK firmware (Zephyr RTOS based) and targets the nRF52840 microcontroller.

**Key Features:**
*   **Split Keyboard:** Left and right halves (`eyelash_sofle_left`, `eyelash_sofle_right`).
*   **Peripherals:** Supports rotary encoders, RGB underglow, backlight, and OLED displays (`nice_view`).
*   **ZMK Studio:** Includes configuration for ZMK Studio support.
*   **Power Management:** configured for deep sleep and external power control to optimize battery life.

# Building and Running

The project uses `west` (Zephyr's meta-tool) for building.

## Prerequisites
*   [ZMK Firmware prerequisites](https://zmk.dev/docs/development/setup) (Docker is recommended for a consistent environment).

## Build Commands

To build the firmware locally (assuming a standard ZMK development setup):

1.  **Initialize and Update:**
    ```bash
    west init -l config
    west update
    west zephyr-export
    ```

2.  **Build Left Half:**
    ```bash
    west build -b eyelash_sofle_left -- -DSHIELD=nice_view
    ```

3.  **Build Right Half:**
    ```bash
    west build -b eyelash_sofle_right -- -DSHIELD=nice_view
    ```

4.  **ZMK Studio Build (Left):**
    ```bash
    west build -b eyelash_sofle_left -- -DSHIELD=nice_view -DCONFIG_ZMK_STUDIO=y
    ```

*Note: The actual build command might vary slightly depending on how the `zmk` app is sourced in your workspace. If using the official ZMK container, these commands are run within the container.*

# Development Conventions

*   **Keymap Editing:** The primary keymap is located in `config/eyelash_sofle.keymap`. Modifications to key bindings and layers should be done here.
*   **Configuration:** Kconfig options (e.g., sleep timeouts, RGB settings) are in `config/eyelash_sofle.conf`.
*   **Board Definition:** Hardware-specific definitions (pins, matrices, sensors) are in `boards/arm/eyelash_sofle/`.
*   **GitHub Actions:** The project uses GitHub Actions (`.github/workflows/build.yml`) to automatically build firmware artifacts on push. The build matrix is defined in `build.yaml`.
*   **Keymap Visualization:** The `keymap-drawer` directory and `keymap_drawer.config.yaml` suggest the use of [Keymap Drawer](https://github.com/caksoylar/keymap-drawer) to generate visual representations of the keymap (SVG files).

# Key Files

*   `config/eyelash_sofle.keymap`: The ZMK keymap file defining layers and behaviors.
*   `config/eyelash_sofle.conf`: Application-level Kconfig settings.
*   `config/west.yml`: The West manifest file, defining dependencies (like the ZMK repo itself).
*   `build.yaml`: Configuration for the automated build workflow.
*   `boards/arm/eyelash_sofle/eyelash_sofle.dtsi`: The device tree source include file, defining the hardware abstraction.
