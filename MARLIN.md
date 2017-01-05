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
  #define MOTHERBOARD 33
#endif
```
1. Define the printer name
```
#define CUSTOM_MENDEL_NAME "Wilson TS"
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
1. Set the thermal runaway feature for the extruder
```
#define THERMAL_RUNAWAY_PROTECTION_PERIOD 40 //in seconds
#define THERMAL_RUNAWAY_PROTECTION_HYSTERESIS 4 // in degree Celsius\
```
1. Set the thermal runaway for the heatbed
```
#define THERMAL_RUNAWAY_PROTECTION_BED_PERIOD 60 //in seconds for 110 bed temp
#define THERMAL_RUNAWAY_PROTECTION_BED_HYSTERESIS 4 // in degree Celsius for 110 bed temp
```
1. Disable max endstops (We don't use them)
```
#define DISABLE_MAX_ENDSTOPS
```
1. Invert the extruder stepper
```
#define INVERT_E0_DIR true
```
1. Set the travel limits (Probably need to retune this)
```
#define X_MAX_POS 210
#define X_MIN_POS -20
#define Y_MAX_POS 190
#define Y_MIN_POS -10
#define Z_MAX_POS 165
#define Z_MIN_POS 0
```
1. Set the homing speeds to something fast yet reasonable
```
#define HOMING_FEEDRATE {150*60, 150*60, 200, 0}
```
Note: might want to see if z can be set to default
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
#define PLA_PREHEAT_HPB_TEMP 50
#define PLA_PREHEAT_FAN_SPEED 0   // Insert Value between 0 and 255

#define ABS_PREHEAT_HOTEND_TEMP 263
#define ABS_PREHEAT_HPB_TEMP 105
#define ABS_PREHEAT_FAN_SPEED 0   // Insert Value between 0 and 255
```
# LCD SEtup
1. Enable the LCD
```
#define REPRAP_DISCOUNT_FULL_GRAPHIC_SMART_CONTROLLER
```
1. Fix the encoder for the LCD
#if defined (REPRAP_DISCOUNT_FULL_GRAPHIC_SMART_CONTROLLER)
 #define DOGLCD
 #define U8GLIB_ST7920
 #define REPRAP_DISCOUNT_SMART_CONTROLLER
 #define ENCODER_PULSES_PER_STEP 4
 #define ENCODER_STEPS_PER_MENU_ITEM 1
#endif
```
1. Disable the sucky beeper and reverse encoder dir
  1. configure pins.h
  2. Search for "REPRAP_DISCOUNT_SMART_CONTROLLER"
  3. Set `#define BEEPER -1`
  4. Reverse pins for `BTN_EN1` `BTN_EN2`
1. Fix the annoyingly long delay for the stop button
  1. Configure Marlin_main.cpp
  2. Set `const int KILL_DELAY = 750;`
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

# Auto Bed levelling
1. disable the Z Axis
```
#define DISABLE_Z true
```
1. Enable auto bed levelling
```
#define ENABLE_AUTO_BED_LEVELING
```
1. Measure at 9 points
```
#define AUTO_BED_LEVELING_GRID_POINTS 3
```
1. Set the extruder offset
```
#define X_PROBE_OFFSET_FROM_EXTRUDER -43 // Default -25
#define Y_PROBE_OFFSET_FROM_EXTRUDER -17 // -29
#define Z_PROBE_OFFSET_FROM_EXTRUDER -12.4 // Mechanical issue is causeing this to vary + for further - for closer
```
1. Raise before homing
```
#define Z_RAISE_BEFORE_HOMING 20
```
Note: This is probably too paranoid and could be down at the default of 4
1. Travel speed (This can probably be left at the default of 8000)
```
#define XY_TRAVEL_SPEED (100*60)
```
1. Only enable the servo for movement (prevent noise issues)
```
#define PROBE_SERVO_DEACTIVATION_DELAY 300
```
1. Enable the servo endstop and set the angles
```
#define NUM_SERVOS 3
#define SERVO_ENDSTOPS {-1, -1, 0}
#define SERVO_ENDSTOP_ANGLES {0,0, 0,0, 71,155}
```
