[TOC]

## Install Arduino SW
1. Download
[Arduino SW Link](https://www.arduino.cc/en/Main/Software)
1. Install
1. Plug in the Arduino via USB cable
* Make sure to install usb driver For windows 10 this should be automatic

# Install U8glib in ARDUINO library
1. Download the latest binary lib for Arduino (NOT THE SOURCE FROM GITHUB)
https://bintray.com/olikraus/u8glib/Arduino
1. Install in Arduino IDE
Sketch/Include Library/Add .ZIP Library
## Install Marlin
1. Download the latest stable Marlin from here
* [Marlin Project](https://github.com/MarlinFirmware/Marlin/releases)
* This is the one you've done in the past
[1.0.2](https://github.com/MarlinFirmware/Marlin/archive/1.0.2-2.zip)
# Configure Marlin for Wilson TS
## Configuration.h
1. Change author
```
#define STRING_CONFIG_H_AUTHOR "(jsaintrocc, default config)" // Who made the changes.
```
1. Select the Arduino Mega Mainboard
```
#ifndef MOTHERBOARD
  #define MOTHERBOARD BOARD_RAMPS_14_EFB
#endif
```
1. Define the printer name
```
#define CUSTOM_MACHINE_NAME "Wilson TS"
```
1. Define the Extruder/Bed Temp thermistor
```
#define TEMP_SENSOR_0 13 //+1 deg at 185, +2 deg at 200, +2.5 deg at 230, +5.5 deg at 240 (Best choice for E3d clone/ebay ntc 3950)
#define TEMP_SENSOR_1 0
#define TEMP_SENSOR_BED 1 //65 at set 60 set 110 at set 110
```
1. Set higher cutoff for extruder heat
```
#define HEATER_0_MAXTEMP 280
```
1. Set PID (Helps stabilze heating quicker)
```
    // Wilson TS
    #define  DEFAULT_Kp 18.68
    #define  DEFAULT_Ki 1.11
    #define  DEFAULT_Kd 78.83
```
1. Set PID for Bed
```
    // Wilson TS Bed
    #define  DEFAULT_bedKp 240.60
    #define  DEFAULT_bedKi 14.98
    #define  DEFAULT_bedKd 965.80
```
1. Tweak the thermal runaway for the heatbed
Edit in Configuration_adv.h
```
#define THERMAL_RUNAWAY_PROTECTION_BED_PERIOD 60 //in seconds for 110 bed temp
#define THERMAL_RUNAWAY_PROTECTION_BED_HYSTERESIS 4 // in degree Celsius for 110 bed temp
```
1. Invert the extruder stepper
```
#define INVERT_E0_DIR true
```
1. Set the travel limits (Probably need to retune this)
```
#define X_MIN_POS -5
#define Y_MIN_POS -10
#define Y_MAX_POS 190
#define Z_MAX_POS 165
```
Note: This is after adjusting your Y table far left

1. Fix Axis Inversion for Wilson
```
#define INVERT_X_DIR true
#define INVERT_Y_DIR false
#define INVERT_Z_DIR true
```
Note Might want to just flip every the stepper cables here
1. Fix Endstop inversion for Wilson
```
#define X_MIN_ENDSTOP_INVERTING true
#define Y_MIN_ENDSTOP_INVERTING true
#define Z_MIN_ENDSTOP_INVERTING true
```
Note: Might want to flip the endstops mechanically in the future
1. Set the homing speeds to something fast yet reasonable
```
#define HOMING_FEEDRATE_XY (150*60)
#define HOMING_FEEDRATE_Z  (3*60)
```
Note: might want to see if z can be set to default (4*60)
1. Define servo steps
```
#define DEFAULT_AXIS_STEPS_PER_UNIT   {80,80,3935,102}  // Wilson TS W/.4 nozzle (Fine Tuned)
// Note: less steps for small, more for bigger
```
1. Define the max feedrates
```
#define DEFAULT_MAX_FEEDRATE          {200, 200, 3, 25}    // (mm/sec)
#define DEFAULT_MAX_ACCELERATION      {2000,2000,100,10000}
```
Note: Need to revisit these settings esp z which could be set at default if we increase stepper current??
1. Jerk settings
```
#define DEFAULT_XYJERK                25.0    // (mm/sec)
#define DEFAULT_ZJERK                 0.6     // (mm/sec) 20% of 3mm/sec max
```
1. Set some preheat settings
```
#define PREHEAT_1_TEMP_BED     50
#define PREHEAT_2_TEMP_HOTEND 263
#define PREHEAT_2_TEMP_BED    105
```
# LCD SEtup
1. Enable the LCD
```
#define REPRAP_DISCOUNT_FULL_GRAPHIC_SMART_CONTROLLER
```
NOTE: I think you can fix the encoder in the config.h now
1. Fix the encoder for the LCD (Edit in Conditionals_LCD.h)
```
#if defined (REPRAP_DISCOUNT_FULL_GRAPHIC_SMART_CONTROLLER)
 #define DOGLCD
 #define U8GLIB_ST7920
 #define REPRAP_DISCOUNT_SMART_CONTROLLER
 #define ENCODER_PULSES_PER_STEP 4
 #define ENCODER_STEPS_PER_MENU_ITEM 1
#endif
```
NOTE: I think you can fix the encoder in the config.h now
1. Disable the sucky beeper and reverse encoder dir
  1. configure pins_RAMPS.h
  2. Search for "REPRAP_DISCOUNT_SMART_CONTROLLER"
  3. Set `#define BEEPER_PIN -1`
  4. Reverse pins for `BTN_EN1` `BTN_EN2`

1. Add SD support
```
#define SDSUPPORT
```

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
