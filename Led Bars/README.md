<h1>LED BARS</h1>
Picture Here

<h2>Introduction</h2>
<p>I made this mod because i wanted to light up the dark area beneath the print bed.</br>
What's better than to use the empty rear corners? Exactly!</p>
<img alt="GitHub Downloads (specific asset, all releases)" src="https://img.shields.io/github/downloads/A3Bagged/Creality-K1/LED_Bars.stl">

<h2>List of items needed.</h2>
<ul>
  <li>WS2812B 5v Ledstrip (144Leds per meter)</li>
  <li>ESP32 board.</li>
  <li>12-24v to 5v converter.</li>
  <li>Some 18AWG 3-pin wire.</li>
  <li>Some 14AWG 2-pin wire.</li>
  <li>4x M3x10 screws</li>
  <li>Optional: Soldering iron.</li>
</ul>

> [!NOTE]
> If you're using a WB2812B 12v strip use a 24 to 12v converter instead.

<h1>Installation.</h1>

> [!CAUTION]
> Applying changes to your wiring at your own risk, be careful and make sure the printer is off and disconnected.

<h3>Step 1.</h3>

<h1>Coding.</h1>
<h3>1. Installing WLED</h3>
<p>I won't take you through the whole process, if it's your first time i recommend Looking up a video on Youtube on how to install WLED to an ESP32.</br>
Hook up your ESP32 to your computer by USB and next go to <a href="https://install.wled.me">Install WLED</a> and install WLEd to your ESP32. No need to make presets just yet.</p>

<h3>2. Installing WLED Klipper Helper.</h3>
<p>I recommend by starting with installing <a href="https://github.com/iamlite/WLED-Klipper-Helper">WLED Klipper Helper</a> this will install all the base things you need.</br>
It will take you through all steps including making your presets. I will share my presets later in this article.</p>

<h3>The actual coding of the bars.</h3>
<p>First of all i recommend deleting the <code>WLED_Macros.cfg</code> as it's <i>Read only</i>. <br>
Next copy <a href="">WLED_Macro.cfg</a> and upload it to your config (or copy the raw and create a new file with the same name).</p>

<p>Next, make the following changes:</p>


