# Auto Bed levelling
1. disable the Z Axis
```
#define DISABLE_Z true
```
1. Enable auto bed levelling
```
#define AUTO_BED_LEVELING_BILINEAR
```
1. Enable safe homing
```
#define Z_SAFE_HOMING
```
1. Specify a fixed probe
```
#define FIX_MOUNTED_PROBE
```
1. Set the extruder offset
```
#define X_PROBE_OFFSET_FROM_EXTRUDER -38
#define Y_PROBE_OFFSET_FROM_EXTRUDER -0
#define Z_PROBE_OFFSET_FROM_EXTRUDER -1.15
```
1. Define the probe box
```
#define LEFT_PROBE_BED_POSITION 25
#define RIGHT_PROBE_BED_POSITION 160
#define BACK_PROBE_BED_POSITION 165
#define FRONT_PROBE_BED_POSITION 20
```
1. Set some speed improvements
```
#define Z_CLEARANCE_DEPLOY_PROBE   0 // Z Clearance for Deploy/Stow
#define Z_CLEARANCE_BETWEEN_PROBES  2 // Z Clearance between probe points
```
