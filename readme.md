## μReflow Oven with MicroPython & LittlevGL

[中文版请见于此](./readme_zh.md)。

This project is modifided and rewritten on top of [Adafruit EZ Make Oven](https://learn.adafruit.com/ez-make-oven?view=all).
The original code of EZ Make Oven can be found [here](https://github.com/adafruit/Adafruit_Learning_System_Guides/tree/master/PyPortal_EZ_Make_Oven).

![](./pic/overview.jpg)


The purpose is to make a reflow soldering oven by modifying a kitchen oven with more affordable and widely available hardwares.
Instead of an oven, a heating plate can also be used.

![](./pic/internal.jpg)

The GUI of this project is built with LittlevGL ([lv_binding_micropython][lv]) which is a very powerful and easy to use GUI library.
LittlevGL already comes with drivers for ILI9341 TFT display and XPT2046 touch controller, this project takes the advantage
of both to ease the user operation. 

![](./pic/screen.jpg)

### Bill of Materials
* 1 x regular kitchen oven with 10-12L capacity, like [this][oven] OR a heater plate, like [this][plate].
* 1 x solid state relay rated at least 10A. Like [this][ssr] (the heater plate linked above already includes it).
* 1 x passive piezo buzzer. Like [this][buzzer].
* 1 x ILI9341 TFT display with on-board XPT2046 touch controller. Like [this][tft].
* 1 x Thermocouple amplifier with K-thermocouple. So far, MAX31855 and MAX6675 are supported. [MAX31855][max31855] [MAX6675][max6675]
* 1 x AC-DC5v power supply to power the ESP32 dev board. Like [this][acdc].
* 1 x ESP32 dev board.  Like [this][esp32].

### Oven Modification and Wiring
* WARNING: The mains (220/110V) can be deadly.  Make sure the oven is unplugged from the wall plug before doing any modification
or wiring.
* Ovens are different one from another, but basically all you need to do is to bypass the original switch and timer, and
let the solid state relay control the heating element, hence the ESP32 board can turn the heating element
on and off via the solid state relay.

### The Firmware for ESP32
* Pls refer to [here](./FIRMWARE/readme.md).

### Installation
* All files are under ```MAIN``` folder.
* After flashing the firmware, you need to edit ```config.json``` to change the GPIO pin numbers according 
to how you wiring your TFT display and other components.
* Set sensor_type to either MAX31855 or MAX6675.
* The ACC pin is for switching the power of the display. If not using a transistor to switch, you may
safely ignore this pin (simply wire the display 3V3 to the 3V3 output of the ESP32)
* Some solid state relays will not switch on with the little current supplied by an ESP32 GPIO pin.
In this case you have to use a transistor between the GPIO pin and the SSR.
* Make sure you have configured the right polarity for all pins.
* Make sure ```"has_calibrated": ``` should be ```false```
* Transfer all the files and folder under ```MAIN``` to the ESP32 dev board and you are good to go.

### Usage Guide
* Upon powering on the first time, you will be guided through touch screen calibration after which the ESP32 board
will reboot. Just follow the guide.
* After calibration and reboot, the GUI will load, where you can select Solder Paste type from the
drop-down menu, just choose the type you'll use, and the reflow temperature profile will show down below.
* If your solder paste isn't there in the menu, you can build your own solder profile files.  Pls refer to: 
https://learn.adafruit.com/ez-make-oven?view=all#the-toaster-oven, under chapter "Solder Paste Profiles".
The new solder profile json file should be put under folder ```profiles```.
* All set and click "Start" button to start the reflow soldering procress.
* If you wish to re-calibrate either the temperature curve or touch screen, click the 'Calibration' button
on the screen, and choose from the popup window.  And follow the on-screen instruction.

[lv]:https://github.com/littlevgl/lv_binding_micropython
[oven]:https://www.aliexpress.com/item/4000151934943.html
[plate]:https://www.aliexpress.com/item/32946772052.html
[ssr]:https://www.aliexpress.com/item/4000083560440.html
[buzzer]:https://www.aliexpress.com/item/32808743801.html
[tft]:https://www.aliexpress.com/item/32960934541.html
[max31855]:https://www.aliexpress.com/item/32878757344.html
[max6675]:https://www.aliexpress.com/item/4000465204314.html
[acdc]:https://www.aliexpress.com/item/32821770958.html
[esp32]:https://www.aliexpress.com/item/32855652152.html