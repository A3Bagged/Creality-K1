![LEDBars](https://github.com/user-attachments/assets/73e892b0-e88e-4a2f-92ea-b266db715b7c)
<div align="center"><img alt="Visitors" src="https://vbr.nathanchung.dev/badge?page_id=https://github.com/A3Bagged/Creality-K1/Led%20Bars"/> <img alt="GitHub Downloads (specific asset, all releases)" src="https://img.shields.io/github/downloads/A3Bagged/Creality-K1/Led Bars/STL/LED_Bars.stl"/> <img alt="Version" src="https://img.shields.io/github/v/release/A3Bagged/Creality-K1/Led Bars"/></div>

# Introduction
I made this mod because i wanted to light up the dark area beneath the print bed.</br>
What's better than to use the empty rear corners? Exactly!

<img src="https://media.discordapp.net/attachments/534181275465154580/1312496302218678375/IMG_4779.jpg?ex=674cb4ef&is=674b636f&hm=4fa48bb488902141febd871787c76929a11f11c8106dbea19a56625086fce5e0&=&format=webp&width=500&height=667"/><img src="https://media.discordapp.net/attachments/534181275465154580/1312496209570824252/IMG_4672.jpg?ex=674cb4d9&is=674b6359&hm=b99ec7ae3fa566dbcbf36b715e002bf136b315244da2ae1a7e91ac0adde939af&=&format=webp&width=500&height=667"/>

# List of items needed.
  - WS2812B 5v Ledstrip (144Leds per meter)
  - ESP32 board.
  - 12-24v to 5v converter.
  - Some 18AWG 3-pin wire.
  - Some 14AWG 2-pin wire.
  - 4x M3x10 screws
  - Optional: Soldering iron.

:warning: If you're using a WB2812B 12v strip use a 24 to 12v converter instead.

# Installation.

> [!CAUTION]
> Applying changes to your wiring at your own risk, be careful and make sure the printer is off and disconnected.

### Step 1.
Turn the printer off, remove the power cable and disassamble the bottom panel off your printer.

### Step 2.
Attach your 14AWG wires to a spare + & - part on your power supply and run them along the existing cable

### Step 3.
Connect the 14AWG wires to the 5v converter and wire one end of the 5v output to ESP32 pin that says "VIN"
The black negative wire should be hooked up to the "GND" Pin. (if you're using 12v strip and converter do not run the 12v output to the "VIN" pin or you'll fry the ESP32).

### Step 4.
Solder some 18AWG wire to your ledstrips and place them in the corners of your frame with the wires towards the top and run the wires behind the steppers into the raceway cover.
It could be hard getting the 18AWG wires through the raceway covers, you might need to remove them and glue them in when you're done or print custom ones.

<img src="https://media.discordapp.net/attachments/534181275465154580/1312496234342256760/IMG_4772.jpg?ex=674cb4df&is=674b635f&hm=802e4cf7d381c3a3f88f2210a76e66f5f5b7f6b593e7e8509a24ebd0134fe897&=&format=webp&width=330&height=440"/><img src="https://media.discordapp.net/attachments/534181275465154580/1312496249382895786/IMG_4773.jpg?ex=674cb4e2&is=674b6362&hm=92b4a4d984bbf6c8c38d6c9ec79d2fc8c44bea45f6ec6a6d36771dccadc3696f&=&format=webp&width=330&height=440"/><img src="https://media.discordapp.net/attachments/534181275465154580/1312496268223975576/IMG_4774.jpg?ex=674cb4e7&is=674b6367&hm=42dd904313b1c313fa8d6d07d6af706045d2e6bf6b2f20fc94cf2d92693f1051&=&format=webp&width=330&height=440"/>

### Step 5
Wire the + and - from your ledstrips to the 5v output of the power converter.  and the green Data wires to free pins on the ESP.
If you want 2 seperate strips wire one to pin "D4" and the other to "D2" else wire both strips to "D4".

<img src="https://media.discordapp.net/attachments/534181275465154580/1312496279284224040/IMG_4775.jpg?ex=674cb4e9&is=674b6369&hm=bdcfd4d21a4eb223b410d0d6d5ca21f6a922814c4e45253c5b3e143221cb5722&=&format=webp&width=500&height=667"/><img src="https://media.discordapp.net/attachments/534181275465154580/1312496291066155008/IMG_4776.jpg?ex=674cb4ec&is=674b636c&hm=729198e32e07a327c0868a6e88f9a9b6af8a7d127b9c4cf25f51a7895313591d&=&format=webp&width=500&height=667"/>

### Step 6.
Install the diffusers using 2x M3x10 screws on the middle side panel hole and the rear panel. (I personally only used 1 screw in the side panel).

## Coding.
### 1. Installing WLED
I won't take you through the whole process, if it's your first time i recommend Looking up a video on Youtube on how to install WLED to an ESP32.
Hook up your ESP32 to your computer by USB and next go to [Install WLED](https://install.wled.me) and install WLED to your ESP32. No need to make presets just yet.

---

### 2. Installing WLED Klipper Helper.
I recommend by starting with installing [WLED Klipper Helper](https://github.com/iamlite/WLED-Klipper-Helper) this will install all the base things you need.
It will take you through all steps including making your presets. I will share my presets later in this article.

---

### 3. The actual coding of the bars.
First of all i recommend deleting the ```WLED_Macros.cfg``` as it's _Read only_.
Next copy [WLED_Macros.cfg](Config/WLED_Macros.cfg) and upload it to your config (or copy the raw and create a new file with the same name).

WLED_Macros.cfg
```jinja2
[gcode_macro UPDATE_WLED]
description: update wled state
gcode:
  {% set brightness = params.BRIGHTNESS|default(-1)|int %}
  {% set intensity = params.INTENSITY|default(-1)|int %}
  {% set speed = params.SPEED|default(-1)|int %}
  {% set PRESET = params.PRESET | default(0) | int %}
  {action_call_remote_method("set_wled_state", strip="chamber", state=True, preset=PRESET, brightness=brightness, intensity=intensity, speed=speed)}

[gcode_macro WLED_OFF]
description: Turn WLED off
gcode:
  {% set strip = params.STRIP|string %}
  {action_call_remote_method("set_wled_state", strip="chamber", state=False)}
```
With these changes we will be able to change Brightness, speed and intensity of the presets.

---

### 4. Optional: Print progress bar
If you want your bars to project the progress of your current print add the following to your ```WLED_Macros.cfg```
```jinja2
[delayed_gcode _update_leds_loop]
initial_duration: 5
gcode:
    _set_leds
    # Loops delayed Gcode every 10 seconds
    UPDATE_DELAYED_GCODE ID=_update_leds_loop DURATION=10

[gcode_macro _set_leds]
gcode:
    {% if printer.extruder.target == 0 %}
    {% else %}
        # the extrude heater is on
        {% if printer.idle_timeout.state == "Printing" %}
            {% set perc = printer.display_status.progress %}
            {% set brightness = params.BRIGHTNESS|default(-1)|int %}
            {% set intensity = params.INTENSITY|default(-1)|int %}
            {% set PRESET = params.PRESET | default(0) | int %}
            # If printer.display_status.progess is more than 0 or less than 100 set right intensity
            # Intensity corresponds to "Percent" preset in WLED
            {% if perc >= 1.00 %}
              {action_call_remote_method("set_wled_state", strip="chamber", state=True, preset=6, intensity=100)}
            {% elif perc > 0.90 %}
              {action_call_remote_method("set_wled_state", strip="chamber", state=True, preset=6, intensity=90)}
            {% elif perc > 0.80 %}
              {action_call_remote_method("set_wled_state", strip="chamber", state=True, preset=6, intensity=80)}
            {% elif perc > 0.70 %}
              {action_call_remote_method("set_wled_state", strip="chamber", state=True, preset=6, intensity=70)}
            {% elif perc > 0.60 %}
              {action_call_remote_method("set_wled_state", strip="chamber", state=True, preset=6, intensity=60)}
            {% elif perc >= 0.50 %}
              {action_call_remote_method("set_wled_state", strip="chamber", state=True, preset=6, intensity=50)}
            {% elif perc > 0.40 %}
              {action_call_remote_method("set_wled_state", strip="chamber", state=True, preset=6, intensity=40)}
            {% elif perc > 0.30 %}
              {action_call_remote_method("set_wled_state", strip="chamber", state=True, preset=6, intensity=30)}
            {% elif perc > 0.20 %}
              {action_call_remote_method("set_wled_state", strip="chamber", state=True, preset=6, intensity=20)}
            {% elif perc > 0.10 %}
              {action_call_remote_method("set_wled_state", strip="chamber", state=True, preset=6, intensity=10)}
            {% elif perc <= 0.00 %}
              {action_call_remote_method("set_wled_state", strip="chamber", state=True, preset=6, intensity=2)}
            {% endif %}
        {% endif %}
    {% endif %}
```
Change *chamber* to whatever your strip is called in your ```Moonraker.conf``` file.

---

### 5. Optional: Reset after END_PRINT.
If you're like me and don't want the print complete anymation running for the rest of the time add the following codes:
(this will wait 1 minute before switching to idle preset of your WLED)

WLED_Macros.cfg
```jinja2
[delayed_gcode backto_idle]
gcode:
  UPDATE_WLED PRESET=4
```

gcode_macro.cfg (or wherever your "END_PRINT" is located) riht at the end before M84:
```jinja2
# Delayed gcode form WLED_macros.cfg, back to idle after 60 secs
  UPDATE_DELAYED_GCODE ID=backto_idle DURATION=60
```

---

## Preset tips:
To get the best out of your presets take a note of the following things:

For a progress bar while printing modify your "printing" preset to ```percent``` with the *intensity* slider all the way to the left 0%.

---

## Images:
Some immages of the build:

---

Enjoy your fancy printer case :)


