
[TOC]

# Software
## Install Arduino SW
1. Download
[Arduino SW Link](https://www.arduino.cc/en/Main/Software)
1. Install
2. Plug in the Arduino via USB cable
* Make sure to install usb driver For windows 10 this should be automatic

# Install U8glib in ARDUINO library
1. Download the latest binary lib for Arduino (NOT THE SOURCE FROM GITHUB)
https://bintray.com/olikraus/u8glib/Arduino
1. Install in Arduino IDE
Sketch/Include Library/Add .ZIP Library
Maybe run the install??
## Install Marlin
1. Download the latest stable Marlin from here
* [Marlin Project](https://github.com/MarlinFirmware/Marlin/releases)
* This is the one you've done in the past
[1.0.2](https://github.com/MarlinFirmware/Marlin/archive/1.0.2-1.zip)
1. Download the Reprap Wilson specific config
* Using GIT

        ```
        git clone https://github.com/mjrice/wilson.git
        ```

* Download the zip


Marlin

https://www.youtube.com/watch?v=d0S5nmVPSyU

Look at 28:15

config Deltas
# Configure Arduino
1. Configure Configuration.h
```
#define STRING_CONFIG_H_AUTHOR "(mrice, default config)" // Who made the changes.
#define BAUDRATE 38400 // being conservative here, mrice recommends 115200
#define MOTHERBOARD 33
#define CUSTOM_MENDEL_NAME "Wilson TS"
#define TEMP_SENSOR_0 13 //186 at 185 set 233 deg at 230
#define TEMP_SENSOR_1 0
#define TEMP_SENSOR_BED 1 // This is an educated guess so verify with temp gun
// Utilmaker
//    #define  DEFAULT_Kp 22.2
//    #define  DEFAULT_Ki 1.08
//    #define  DEFAULT_Kd 114
#define THERMAL_RUNAWAY_PROTECTION_PERIOD 40 //in seconds
#define THERMAL_RUNAWAY_PROTECTION_HYSTERESIS 4 // in degree Celsius
#define THERMAL_RUNAWAY_PROTECTION_BED_PERIOD 20 //in seconds
#define THERMAL_RUNAWAY_PROTECTION_BED_HYSTERESIS 2 // in degree Celsius
const bool X_MIN_ENDSTOP_INVERTING = true; // set to true to invert the logic of the endstop.
const bool Y_MIN_ENDSTOP_INVERTING = true; // set to true to invert the logic of the endstop.
const bool Z_MIN_ENDSTOP_INVERTING = true; // set to true to invert the logic of the endstop.
#define DISABLE_MAX_ENDSTOPS
#define INVERT_X_DIR true    // for Mendel set to false, for Orca set to true
#define INVERT_Y_DIR false    // for Mendel set to true, for Orca set to false
#define INVERT_Z_DIR true     // for Mendel set to false, for Orca set to true
#define INVERT_E0_DIR true   // for direct drive extruder v9 set to true, for geared extruder set to false
// Travel limits after homing
#define X_MAX_POS 220
#define X_MIN_POS 0
#define Y_MAX_POS 215
#define Y_MIN_POS 0
#define Z_MAX_POS 175
#define Z_MIN_POS 0
#define DEFAULT_MAX_FEEDRATE          {200, 200, 5, 25}    // (mm/sec)
#define DEFAULT_MAX_ACCELERATION      {3000,3000,100,10000}    // X, Y, Z, E maximum start speed for accelerated moves. E default values are good for Skeinforge 40+, for older versions raise them a lot.
#define DEFAULT_XYJERK                10.0    // (mm/sec)
#define PLA_PREHEAT_HPB_TEMP 50
#define PLA_PREHEAT_FAN_SPEED 0   // Insert Value between 0 and 255
#define ABS_PREHEAT_FAN_SPEED 0   // Insert Value between 0 and 255

#define ABS_PREHEAT_HOTEND_TEMP 220
#define REPRAP_DISCOUNT_FULL_GRAPHIC_SMART_CONTROLLER
#define HOMING_FEEDRATE {200, 200, 100, 0}  // set the homing speeds (mm/min)
#define DEFAULT_AXIS_STEPS_PER_UNIT   {80,80,4000,100}  // Wilson TS W/.4 mm extruder
// Fix the encoder for the LCD
#if defined (REPRAP_DISCOUNT_FULL_GRAPHIC_SMART_CONTROLLER)
 #define DOGLCD
 #define U8GLIB_ST7920
 #define REPRAP_DISCOUNT_SMART_CONTROLLER
 #define ENCODER_PULSES_PER_STEP 4
 #define ENCODER_STEPS_PER_MENU_ITEM 1
#endif
```
# Fix the beeper/reverse encoder dir
1. Configure pins.h
2. Search for "REPRAP_DISCOUNT_SMART_CONTROLLER"
3. Set `#define BEEPER -1`
4. Reverse pins for `BTN_EN1` `BTN_EN2`
# Fix the annoyingly long delay for the stop button
1. Configure Marlin_main.cpp
2. Set `const int KILL_DELAY = 750;`
Note: Baudrate of 38400 based on ... some website caluclator
1. Save the project
    File/Save
# Configure Ardino IDE to talk to arduino
1.  Plug in the arduino via the USB cable (Should power up (Including LCD)
2. Select the model
3. Tools/Board/Arduino/Genuino Mea or Mega 2560
4. CPU should be Mego 2560
4. Select the port (Should only have one in the dropdown)
5. Select Board Info (Should verify above settings)

# Compile the code
1. Sketch/Verify/compile
# Upload
1. Sketch/Upload (This will take about 1 minute) You'll see the LCD screen blink when it's done

# Install Printrun
http://www.pronterface.com/
http://koti.kapsi.fi/~kliment/printrun/

Extract in a directory and run 
pronterface.exe

# Leveling the bed 1st time
1. Tighten all bed screws to middle of travel
3. Position limit switch/adj screw so that extruder will home just below bed ~5mm
3. Adj screw should ideally be in the middle of its travel
3. Make sure extruder is positioned to the side of the bed (So it wont crash into bed)
4. In proterface home Z
5. Now move Z up and positon above center of bed
6. Put piece of paper on bed (Business card is the right thickness)
7. Slowly lower extruder untill you feel it starting to pin the paper (slighly)
8. Adjust limit screw so this pisition triggers limit
9. Test by homing the Z axis
10. If paper is too pinched back off limit switch and repeat last step
Note: Just get close here (No need to be exact)
11. Disable the steppers
12. Move x/y to each corner with paper (You want it to just barely pinch)
13. Adjust each corner until all just barely pinch
14. Heat up hot end
15. Do the paper test and adjust the z set screw to make the paper barely pinch
Note: Might want to try and 

# Precision Bed Leveling
1. Heat up both hot end and bed to ABS temps 230/105 should do
2. use .1 feeler guage (Should be rough but .6 should pass under freely)
3. Start on one corer adjusting until feeler gauge just passes underneath 
4. Adjust so .152 has trouble (This is hard to detect maybe try .203)
5. Now to bodge it by rotating the z adj one whole turn (Skip this with .1 feeler guage)

# Extruder Calibration
1. mark filament 150mm above extruder body
 (Relative mesurement is ok) 
2. Heat up the extruder to ABS temp (For PLA)
   (note just doing this as I think the temp is off)
2. Extrude 100 mm at 100mm/min
Start with 100 mm at 50 mm/min (for our .4 nozzle)

initial measurement 150
final measurement 63 52 54 53 56 53   
hotend at 230 extruder free floating 55 55 54
hotend at 185 cant extrude pla 
hotend 220 62 61
hotend 250 52 52 52
hotend 250 @ 120 mm/s 52 
hotend 250 @ 150 mm/s 52 52 53
diff 150-64=87

60 60 58

at 50 mm/a 53 51 52
at 40 mm/s 51
initial steps per mm * 100/diff
(100 * 100)/87 = 115
 
# Acceleration
1. download toms accelleration test part
https://www.youmagine.com/designs/resonance-test
1. Load part in cura
2. Print this part with one shell and 100% rectangular infill 1 shell = wall thinkness = to nozzle aka.4
1. Set print speed to 100

https://www.youtube.com/watch?v=7HsIZuj9vOs

## Jerk (3:00 in video)
Set to 20% - 30% of print speed
For 60mm/sec (cura) set to 12 - 18 (15 mm/s is toms rec for this)

1. Set in marlin configuration.h
```
#define DEFAULT_XYJERK                15.0    // (mm/sec)
```
`#define DEFAULT_XYJERK                25.0    // (mm/sec) for 100mm/s`


## Acceleration (5:10 in video)
1. Set max speed so something low for x and y say 100
```
M203 X100 Y100
```
1. Set acceleration
```
M201 X1000 Y1000
```
1. if it works increase by 10% - 20% and try again

If you see resonance decrease both by 20% and repeat
`#define DEFAULT_MAX_ACCELERATION      {1000,1000,100,10000}`

*Fan M106 S255*

## Max Speed

Set this for something reasonable like 100

 # Set the Travel Limits
 Because you're limit switch is offset from the build plate you need to adjust these for the real values.

1. Home the z axis
1. Home the x axis
2. measure from the extruder tip to the build platform (along the x axis)
    This is your X min pos
    measure from the tip to the other side of the build platform (along the x axis)
    This is your X max pos
3. Repeat for the y axis

```
 // Travel limits after homing
#define X_MAX_POS 220
#define X_MIN_POS 0
#define Y_MAX_POS 215
#define Y_MIN_POS 0
#define Z_MAX_POS 175
#define Z_MIN_POS 0
```
# Fixes
Need to make sure extruder is aligned with filament hole
extruder clamp springs are way to weak
extruder clamp screws need to be smooth near head
thermistor for extruder is way off.
# Slicer
Use Ultimaker Cura
https://ultimaker.com/en/products/cura-software
Select Prusa i3
Name it Wilson TS

Tried .06 mm layer height
60mm speed
120 mm travel (This probably needs to be reduced)


# Controller board Docs
http://reprap.org/wiki/RAMPS_1.4

Arduino MEGA 2560 R3

# Hotend
Guessing it has a ntc 3950 thermistor Assumed based on other ebay adds for similar hotends

# Todo
## PID Auto tune Extruder
1. In proterface run the following gcode
    ```
M301 P0.00 I0.00 D0.00
M303 S150
    ```
You will get a temp too high error (because the values are so far out of whack
Now set S to the current temp of your hotend and it'll work
    ```
M303 S180
    ```

 Take the output and put in the PID section Under UTILMAKER
```
// Wilson TS
    #define  DEFAULT_Kp XXX
    #define  DEFAULT_Ki XXX
    #define  DEFAULT_Kd XXX
```
Once initially configured run the marlin PID autotune
http://reprap.org/wiki/PID_Tuning
## PID Auto tune Bed
Same process as above accept use this gcode
    ```
M303 E-1 C8 S90


    ```
    // Wilson TS Bed
    #define  DEFAULT_bedKp XXX
    #define  DEFAULT_bedKi XXX
    #define  DEFAULT_bedKd XXX
    ```
## Set Travel limits
use the prepare/montion controls to do this (Make sure it won't bumb into stuff)

## Set Axis Steps and feed rate 
```
#define DEFAULT_AXIS_STEPS_PER_UNIT   {78.7402,78.7402,200.0*8/3,760*1.1}  // default steps per unit for Ultimaker
#define DEFAULT_MAX_FEEDRATE          {200, 200, 5, 25}    // (mm/sec)
#define DEFAULT_MAX_ACCELERATION      {3000,3000,100,10000}    // X, Y, Z, E maximum start speed for accelerated moves.
```
## Figure out how to tune default_max_freedrate default_max_acceleration

3.98 mm for 40.1

X And Y Calc
Motor
1.8 DEGREE
X Pulley 20T
2mm GT2 pulley
1/16 micro stepping
If the jumpers set it to a higher number of micro steps than supported by the driver it will operate at the maximum number of micro steps for that driver. For now the default is maximum micro stepping (all jumpers installed under drivers), which results in 1/16 micro stepping for A4988 drivers and 1/32 for DRV8825

= 80

Z Calc
Motor
1.8 DEGREE
1/16 micro stepping
.8 M5 Lead Screw
=4000

Extruder Calc
.2 layer height
.8 M5 Lead Screw
=50


