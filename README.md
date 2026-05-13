# Dmhuisma Keyboard ZMK Module

This is my ZMK module for the split keyboard I generated with the [Cosmos keyboard generator](https://ryanis.cool/cosmos/). This is version 2 of the keyboard ([version 1](https://github.com/dmhuisma/dmhuisma-keyboard-zmk-module/tree/v1) is shown in the image below as I have not built it yet). After I built [Splitcore](https://github.com/dmhuisma/splitcore-zmk) I found that I never used this one, so I decided to repurpose its parts into something new.

![Cosmos Keyboard Screenshot](assets/images/cosmos_keyboard_screenshot.jpg)

The cosmos generator configuration is saved in the URL, here is [my configuration](https://ryanis.cool/cosmos/beta#cm:CpgCCiISCBCAbyAniAEUEgIgExICIAASA4gBCjgeQICmi4AFSLMDCisSChCAYyAnQASIARQSAiATEgIgABIDELA7EggQsGsgKIgBDzgKQIC0XUgACikSCQgNEIBXICdIABICIBMSAiAAEgMQsC8SBRCwXyAoOAlAgNSG2AJIAAoWEggQgEsgJ4gBMhICIBMSAiAAEgA4HQocEggQgD8gJ4gBMhICIBMSAiAAEgIIASICKAA4MQo1EgggJ2IEVEVSTRIFEJB3IBMSBhCgzgogABIMEDBiBVNISUZUiAEUODJAktqKgAVIs4OAoAYKHhIaCJmAAhAgGF8gEUCAgLwcSIOPrKzB8AGIATI4RRgAQOiFoK7wVUja6KKQAQqzAQokEhMIARBBGBMgDkAASICAjP0DiAEBEggIgCBQa4gBIzCBMDgTCjcSEQiBMBDBgAIYAEiAgIz9A1ByEhQIARAxGAQgKkCA+KAOSISW1oi+BxIIEEFQhQGIAQcwATgACi8SDRBBIAxAgFBIgICM/QMSBiANQABIABISEDEYGCAiQIOAoA5IgJbW+IkHMAE4FBgCIgwIyAEQyAEYACAAKAAoAzALQIeJzKywNkipjYC2kbMcCp8CCiISCBCAAyAniAEUEgIgExICIAASA4gBCjgdQICmi4AFSLMDCiYSCBCADyAniAEUEgIgExICIAASABIIELBrICiIAQ84CUCAtF1IAAokEgcIDRCAGyAnEgIgExICIAASABIFELBfICg4CkCA1IbYAkgAChYSCBCAJyAniAEyEgIgExICIAASADgeChwSCBCAMyAniAEyEgIgExICIAASAggBIgIoADgyCi8SERBAIBNAqKhQSNGHgLgviAE8EggIGyAAQICGCjAdOEZAnfiG2AVI/o/wpKG3AQo1EgggJ2IEVEVSTRIFEJB3IBMSBhCgzgogABIMEDBiBVNISUZUiAEUODFAkdqKgAVIs4OAmAYYAUDnhaCu8FVI2uaiiAEYjaACIgIoHDAfggEBA0gIWEhgA2gAcigoZDATQFpY4gmIAeIJYIQHcNAFmAEUoAEAoAEAoAEAoAEAoAEAqAFLePSHnKXxOA==).

## Features
- Ergonomic
    - Split
    - Concave key well
    - Thumb clusters
    - Staggered to accommodate my hand/finger sizes
- Trackpad
- Display
- Hotswappable keys
- Potentiometer (with directional clicking) for media controls.
- [Custom DPAD](https://github.com/dmhuisma/1u_mx_dpad) on the left side for cursor navigation or gaming.

## Building

The firmware is built by Github actions on every commit. It can also be built with a local install of ZMK with the following commands.

> west build -p auto -d build/left -b nice_nano//zmk -- -DZMK_EXTRA_MODULES="[path to module]/dmhuisma-zmk;[path to module]/zmk_arbitrary_split_data_channel;[path to module]/zmk-split-status-relay;/[path to module]/nice-view-peripheral" -DSHIELD="dmhuisma_left nice_view_peripheral"

> west build -p auto -d build/right -b nice_nano//zmk -- -DZMK_EXTRA_MODULES="[path to module]/dmhuisma-zmk;[path to module]/zmk-driver-azoteq-iqs5xx;[path to module]/zmk_arbitrary_split_data_channel;[path to module]/zmk-split-status-relay;" -DSHIELD="dmhuisma_right"

> west build -p auto -d build/dongle -b nrf52840_mdk_usb_dongle -- -DZMK_EXTRA_MODULES="[path to module]/dmhuisma-zmk;/[path to module]/zmk-pointing-acceleration-alpha;[path to module]/zmk_arbitrary_split_data_channel;[path to module]/zmk-split-status-relay" -DSHIELD="dmhuisma_dongle"

If building locally, the following external zmk modules are required on your machine:

- [zmk-driver-azoteq-iqs5xx](https://github.com/AYM1607/zmk-driver-azoteq-iqs5xx)
- [zmk-pointing-acceleration-alpha](https://github.com/nuovotaka/zmk-pointing-acceleration-alpha)
- [zmk-arbitrary-split-data-channel](https://github.com/dmhuisma/zmk_arbitrary_split_data_channel)
- [zmk-split-status-relay](https://github.com/dmhuisma/zmk-split-status-relay)
- [nice-view-battery-peripheral](https://github.com/dmhuisma/nice-view-battery-peripheral)

## Pinout

This keyboard uses the [nice!nano V2](https://nicekeyboards.com/nice-nano/) on each side. However, there is not enough GPIO pins for this keyboard, so it also uses shift registers (74HC595) for the output pins.

### Left Keyboard

#### Nice!Nano V2 Left Side GPIO
|                       |                                               |
|-----------------------|-----------------------------------------------|
|Battery-               ||
|**[D1]** P0.06         |RKJXT1F42001 Encoder A|
|**[D0]** P0.08         |RKJXT1F42001 Encoder B|
|GND                    |NiceView GND|
|GND                    |DPAD GND, RKJXT1F42001 GND|
|**[D2]** P0.17         |NiceView MOSI|
|**[D3]** P0.20         |RKJXT1F42001 down|
|**[D4]** P0.22         |NiceView SCK|
|**[D5]** P0.24         |74HC595 SCK|
|**[D6]** P1.00         |74HC595 MOSI|
|**[D7]** P0.11         |DPAD Up|
|**[D8]** P1.04         |DPAD Down|
|**[D9]** P1.06         |Column 4|

#### Nice!Nano V2 Right Side GPIO
|                       |                                               |
|-----------------------|-----------------------------------------------|
|Battery+               ||
|Battery+               ||
|GND                    |74HC595 GND, 74HC595 OE|
|Reset                  ||
|3.3V Vcc               |NiceView Vcc, 74HC595 Vcc|
|**[D21]** P0.31 (ADC)  |Row 0|
|**[D20]** P0.29 (ADC)  |Row 1|
|**[D19]** P0.02 (ADC)  |Row 2|
|**[D18]** P1.15        |Row 3|
|**[D15]** P1.13        |74HC595 CS/RCLK|
|**[D14]** P1.11        |NiceView CS|
|**[D16]** P0.10        |DPAD Left|
|**[D10]** P0.09        |DPAD Right|

#### Nice!Nano V2 Back GPIO
|                       |                                               |
|-----------------------|-----------------------------------------------|
|P1.01                  |RKJXT1F42001 left|
|P1.02                  |RKJXT1F42001 right|
|P1.07                  |Row 4|

#### Shift Register (74HC595) GPIO
|                       |                                               |
|-----------------------|-----------------------------------------------|
|QA                     |Column 0|
|QB                     |Column 1|
|QC                     |Column 2|
|QD                     |Column 3|
|QE                     ||
|QF                     |Column 5|
|QG                     |Column 6|
|QH                     |Column 7|
|QH`                    ||

### Right Keyboard

#### Nice!Nano V2 Left Side GPIO
|                       |                                               |
|-----------------------|-----------------------------------------------|
|Battery+               ||
|**[D1]** P0.06         |Trackpad reset|
|**[D0]** P0.08         |Trackpad ready|
|GND                    |Trackpad GND|
|GND                    ||
|**[D2]** P0.17         |Trackpad SDA|
|**[D3]** P0.20         |Trackpad SCL|
|**[D4]** P0.22         ||
|**[D5]** P0.24         |74HC595 SCK|
|**[D6]** P1.00         |74HC595 MOSI|
|**[D7]** P0.11         ||
|**[D8]** P1.04         ||
|**[D9]** P1.06         |Column 9|

#### Nice!Nano V2 Right Side GPIO
|                       |                                               |
|-----------------------|-----------------------------------------------|
|Battery+               ||
|Battery+               ||
|GND                    |74HC595 GND, 74HC595 OE|
|Reset                  ||
|3.3V Vcc               |74HC595 Vcc, Trackpad Vcc|
|**[D21]** P0.31 (ADC)  |Row 0|
|**[D20]** P0.29 (ADC)  |Row 1|
|**[D19]** P0.02 (ADC)  |Row 2|
|**[D18]** P1.15        |Row 3|
|**[D15]** P1.13        |74HC595 CS/RCLK|
|**[D14]** P1.11        ||
|**[D16]** P0.10        ||
|**[D10]** P0.09        ||

#### Nice!Nano V2 Back GPIO
|                       |                                               |
|-----------------------|-----------------------------------------------|
|P1.01                  ||
|P1.02                  ||
|P1.07                  |Row 4|

#### Shift Register (74HC595) GPIO
|                       |                                               |
|-----------------------|-----------------------------------------------|
|QA                     |Column 8|
|QB                     ||
|QC                     |Column 10|
|QD                     |Column 11|
|QE                     |Column 12|
|QF                     |Column 13|
|QG                     |Column 14|
|QH                     |Column 15|
|QH`                    ||

### Nice!Nano V2 pinout for reference

![Nice!Nano V2 Pinout](https://nicekeyboards.com/static/1788ac663060fd510f4894b286cd97b1/a6d36/pinout-v2.png)
