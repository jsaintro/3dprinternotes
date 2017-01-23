# Stats
* Weight 57g
* LJC18A3-H-Z/BX
* NPN 6-36V/300mA 1mm - 10mm NO

# Wiring
Blue = Ground
Brown = 12V
Black = Arduino Z Stop Normally Open Pin (With pullup)

# Adjustment
CCW More sensitive
CW Less Sensitive
# Will this cook my arduino?
* Shouldn't.. when untriggered the sense pin is floating
  When you measure it using a multimeter it will show 12v becuase it's an induced current.  Using the pullup on the arduino sense pin will keep it from floating and it will read 5v when untriggered.  When triggered it will go to ground.

# How well will this work on my surface??
1. Mirrored Glass is best
* This is a CAPACITIVE SENSOR so it's basically measuring the distance between two conductors.  Glass is an insulator so bare glass means that you'll be measuring the distance between the sensor and what's under the glass ... the heatbed (Prone to error/warping non uniform conductance.  But a mirror is ideal because a modern mirror is a piece of glass with a thin layer of conductive metal (Which is very uniform ;-).  If you check the back of a mirror for conductivity keep in mind that they normally put a layer of non-conductive paint to protect the metal from corrosion.
* Recommended distance 2 - 4 mm
* If it's too close you run the risk of triggering off the something below the sensor.  Too far and you increate your target area which reduced your accuracy.

* Arduino pin with pullup installed and unattached
  Sense Pullup V 4.897 Vcc 4.921
* with Sendor attached 5.618V

* Unattached 9.90 V


* Signal light glows slightly when hooked up to sense pin.
* 33 Ohm current limiting resistor didn't help
* 50% voltage divider didn't help 
  10K/10K 4.8V when open 1V when grounded.

# Mechanical Placement
Adjust extruder to bed
Adjust sensor position by putting 1 quarter between it and bed
You'll be adjusting for sensor to trigger between 2mm and 3mm

1. With sensor untriggered (faint light)
Send M119 to see if Z endstop is untriggered
```
Reporting endstop status
x_min: open
y_min: open
z_min: open
ok
```
2. Trigger sensor (Strong light)
```
Reporting endstop status
x_min: open
y_min: open
z_min: TRIGGERED
ok
```
3. Adjust pot ccw greater distance from bed cw shorter distance from bed.
Adjust so it triggers at 2mm from bed.  Use stacked business cards to determine 2 mm

Use G92Z{distance) to set position of z independent of probe

# Calculate z height distance
1. Preheat hotend and preheat bed
1. Home Z
```
G28
```
1. Do autolevel procedure
```
G29
```
1. recalibrate current z pos to 10mm (So we can move down 10mm from current)
```
G92Z10
```
1. Move z down in .1mm increments untill .7mm feeler is pinched then backoff .1
1. Determined current position
```
M114
```
Z = 9.16
9.16 - 10 = -.84 -.85
Since trigger is closer than hotend

Offset from ext
Negative means closer to min endstop. pos means further from min endstop

 calibrate current position to 10mm off bed surface (So we can move below)
Move Z down in .1mm increments until hotend just touches feeler guage .7??


# Repeatability
```
ok
Bed x: 25.00 y: 20.00 z: -0.83
Bed x: 92.00 y: 20.00 z: -0.93
Bed x: 159.00 y: 20.00 z: -0.83
Bed x: 160.00 y: 92.00 z: -0.81
Bed x: 93.00 y: 92.00 z: -0.92
Bed x: 26.00 y: 92.00 z: -0.83
Bed x: 25.00 y: 164.00 z: -0.77
Bed x: 92.00 y: 164.00 z: -0.92
Bed x: 159.00 y: 164.00 z: -0.82
Eqn coefficients: a: -0.00 b: 0.00 d: -0.86
planeNormal x: 0.00 y: -0.00 z: 1.00
ok
echo:endstops hit:  Z:-0.82
echo:Home X/Y before Z
ok
ok
echo:Home X/Y before Z
ok
ok
Bed x: 25.00 y: 20.00 z: -0.84
Bed x: 92.00 y: 20.00 z: -0.96
Bed x: 159.00 y: 20.00 z: -0.86
Bed x: 160.00 y: 92.00 z: -0.81
Bed x: 93.00 y: 92.00 z: -0.92
Bed x: 26.00 y: 92.00 z: -0.80
Bed x: 25.00 y: 164.00 z: -0.74
Bed x: 92.00 y: 164.00 z: -0.89
Bed x: 159.00 y: 164.00 z: -0.82
Eqn coefficients: a: -0.00 b: 0.00 d: -0.86
planeNormal x: 0.00 y: -0.00 z: 1.00
ok
echo:endstops hit:  Z:-0.82
```
