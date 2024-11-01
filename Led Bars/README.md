# LED BARS
Picture Here

# Introduction
I made this mod because i wanted to light up the dark area beneath the print bed.</br>
What's better than to use the empty rear corners? Exactly!

<img alt="GitHub Downloads (specific asset, all releases)" src="https://img.shields.io/github/downloads/A3Bagged/Creality-K1/LED_Bars.stl">

# List of items needed.
  - WS2812B 5v Ledstrip (144Leds per meter)
  - ESP32 board.
  - 12-24v to 5v converter.
  - Some 18AWG 3-pin wire.
  - Some 14AWG 2-pin wire.
  - 4x M3x10 screws
  - Optional: Soldering iron.

> [!NOTE]
> If you're using a WB2812B 12v strip use a 24 to 12v converter instead.

# Installation.

> [!CAUTION]
> Applying changes to your wiring at your own risk, be careful and make sure the printer is off and disconnected.

### Step 1.

### Step 2.

## Coding.
### 1. Installing WLED
I won't take you through the whole process, if it's your first time i recommend Looking up a video on Youtube on how to install WLED to an ESP32.
Hook up your ESP32 to your computer by USB and next go to [Install WLED](https://install.wled.me) and install WLEd to your ESP32. No need to make presets just yet.

### 2. Installing WLED Klipper Helper.
I recommend by starting with installing [WLED Klipper Helper](https://github.com/iamlite/WLED-Klipper-Helper) this will install all the base things you need.
It will take you through all steps including making your presets. I will share my presets later in this article.

### 3. The actual coding of the bars.
First of all i recommend deleting the ```WLED_Macros.cfg``` as it's _Read only_.
Next copy [WLED_Macro.cfg](codes/WLED_Macro.cfg) and upload it to your config (or copy the raw and create a new file with the same name).

Next, make the following changes:


