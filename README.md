# NotificationEgg
 
This repository contains all files needed to build a notification egg.

![Video](https://giant.gfycat.com/CourteousFlatCygnet.mp4)

![Cracked egg](images/egg_cracked.jpg)

[Link to original reddit post](https://www.reddit.com/r/homeassistant/comments/vv33gt/i_made_this_egg_that_shows_multiple_notifications/)

## BOM
| Picture | Role | Component | Comment |
|-|-|-|-|
|![Picture of ESP32](images/d1mini-esp32.jpg)|Microcontroller | ESP32 | ESP8266 is sufficient for displays with smaller resolution/color space due to their RAM requirements.<br> I used a WeMos D1 Mini ESP32 for its compact form.|
|![Picture of GC9A01](images/gc9a01.png)| Display | GC9A01 | I love the look of this round 240x240 screen, but it requires a lot of RAM to drive. In an earlier version, I used a SSD1306 0.96" OLED (STL included)|
|![Picture of SK6812 144/m](images/sk6812-144.jpg)| LED Strip | SK6812 (144LED/m) | More LEDs make for a smoother effect. If you want to go with a non-addressable LED strip, maybe my horrible ESPHome effect can serve as a starting point for you|

### Miscellaneous/optional components
- MicroUSB Breakout Module: Helps so I don't have to position the ESP near the hole for the wire, also provides some sort of strain relief. Hotglued to the bottom of the inside.
- 80mm x 2mm Aluminium Tube: Cut to 18mm length and added a slot for the wires of the LED strip. Serves as unnecessary heatsink for the LEDs, but the adhesive sticks better to it than to the printed ring...
- 2 M5 threaded heat inserts: Can be inserted into the top to attach the halves together with some ~30mm+ M5 bolts. In hindsight not necessary, my friction fit of the aluminium tube is strong enough so it doesn't come apart easily.
- Perfboard: For connecting ESP and the rest. I don't like to solder stuff directly to my MCUs, mostly for misplaced reusability.

## Wiring
Wiring is mostly self-explanatory. All pins of the components are connected to the ESP according to the ESPHome YAML configuration. Change at your own will. Since I was using a MicroUSB breakout board, I connected the 5V of the LED strip directly to it instead of through the ESP board.

## Software
The device is controlled via MQTT. It subscribes to a topic `esphome/notification_egg/notify/{COLOR}` for each `COLOR` in `red`,`green`,`blue`,`cyan`,`yellow` and `magenta`. When a timestamp (in: seconds since 01.01.1970) is published to that topic, the device interprets this timestamp as an "expiration date" for the notification of that color. If a timestamp arrives which has an earlier expiration date than a previous notification of this color, it is ignored. When a timestamp of `0` is received, the notification gets cancelled immediately, no matter its original expiration date.
### Firmware
The firmware for the ESP is built and uploaded by [ESPHome](https://esphome.io) using `esphome run notification_egg.yaml`. If you don't know about ESPHome, make sure to check it out, it is a simple and awesome way to create firmware for ESP based devices that provides many handy features, like automatic MQTT exposure for your components as well as seamless integration with Home Assistant via their native API or MQTT discovery.

### Adapting the configuration
If you want to add or change colors, feel free. I will update this section with more detailed instructions later.

## STL Files
As the printed parts are only decorative, perimeters, infill and layer height can be varied to one's own liking.

I printed the top and bottom halves with the "cut" side facing down on the print bed. I added supports in the center, but I'm not sure if that was necessary, however my Voron V0.1 prints fast enough and with bad enough overhangs that I didn't put to much thought into it.
### Top
I uploaded a version for a GC9A01 1.28" 240x240 round display (with holes like [in this picture](images/gc9a01.png)) and a SSD1306 0.96" OLED. Both are mounted either with small self-tapping plastic screws or just copious amounts of hotglue.

M5 threaded inserts can be added to clamp the halves together.

### Diffusor
I printed this part in spiral vase mode (to prevent a seam) from white PLA. You might want to experiment with your extrusion width settings to get a good fit. The bottom of the part can be hard to fit into the slot, in which case I would just scale the height a little higher and cut the first "layer" off. 

I experienced that printing this part in its original size was sometimes tricky to get to fit into the groove without distorting the circle, which I solved by printing it again after scaling it down to ~99.5% in X and Y directions. Fortunately it only takes a few minutes to print and consumes almost no filament, so there is room for experimentation

### LED ring
You can substitute this part for a 80mm aluminium tube with up to 2mm wall strength, cut to 18mm length. I thought this would serve as a heatsink for the LEDs, but they are off most of the time so the only benefit of the aluminium is that the adhesive of the LED strip sticks a little better.