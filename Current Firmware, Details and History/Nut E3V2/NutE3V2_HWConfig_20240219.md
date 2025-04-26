# CUHAR Ender-3 V2 Printer
## Hardware
- Extruder changed to BMG 3:1, default stepper, E steps need to be changed
- Triangle Labs CHC Pro hotend with ceramic heating element
- Creality V4.2.2

## Software configurations
 - Used Ender3V2S1-20240122 as base code
 - Used [Special_Configurations-main](https://github.com/mriscoc/Special_Configurations/tree/1f203219427769a8674613fe5657bb9468ff2431) to generate Ender3V2-422-BLTUBL-HomeOffs-IS-LA-MPC-Speaker
 - Used [Guide](https://manuelmclure.github.io/ConfiguringLeveling.html#:~:text=The%20world%20of%20UBL%20%2D%20Machine%20limits%20and%20bed%20position,-The%20most%20important&text=It%20has%20a%20physical%20bed,bed%20on%20all%20four%20sides) to set up home offsets
 - Used [G-Code Generator](https://marlinfw.org/tools/lin_advance/k-factor.html) to set up K factor
### Configuration Edits:
- Settings
  - Default
  - Changed

#### Other values:
- Max acceleration XYZE 1000, 1000, 200, 10000
- Default jerk XYZE 20.0, 20.0, 0.8, 10.0
- Retraction distance 0.6 mm


#### platformio.ini
- default_envs
  - `STM32F103RC_creality`
  - `STM32F103RE_creality`

#### Configuration.h
- #define STRING_CONFIG_H_AUTHOR
  - `Miguel Risco-Castillo (MRiscoC)`
  - `Nut E3V2`
- MPC constants
  - `#define MPC_HEATER_POWER { 40.0f }                  // (W) Heat cartridge powers.`
  - `#define MPC_BLOCK_HEAT_CAPACITY { 14.40 }           // (J/K) Heat block heat capacities.`
  - `#define MPC_SENSOR_RESPONSIVENESS { 0.2187 }         // (K/s per ∆K) Rate of change of sensor temperature from heat block`.
  - `#define MPC_AMBIENT_XFER_COEFF { 0.1257 }           // (W/K) Heat transfer coefficients from heat block to room air with fan off.`
  - `#define MPC_AMBIENT_XFER_COEFF_FAN255 { 0.1315 }  // (W/K) Heat transfer coefficients from heat block to room air with fan on full.`
  - to
  - `#define MPC_HEATER_POWER { 50.0f }                  // (W) Heat cartridge powers. // Nut E3V2`
  - `#define MPC_BLOCK_HEAT_CAPACITY { 6.69 }           // (J/K) Heat block heat capacities.`
  - `#define MPC_SENSOR_RESPONSIVENESS { 0.3731 }         // (K/s per ∆K) Rate of change of sensor temperature from heat block.`
  - `#define MPC_AMBIENT_XFER_COEFF { 0.0591 }           // (W/K) Heat transfer coefficients from heat block to room air with fan off.`
  - `#define MPC_AMBIENT_XFER_COEFF_FAN255 { 0.0783 }  // (W/K) Heat transfer coefficients from heat block to room air with fan on full.`
- PID constants
  - `#define DEFAULT_bedKp 462.10`
  - `#define DEFAULT_bedKi  85.47`
  - `#define DEFAULT_bedKd 624.59`
  - to
  - `#define DEFAULT_bedKp 213.89 // Nut E3V2`
  - `#define DEFAULT_bedKi  41.77 // Nut E3V2`
  - `#define DEFAULT_bedKd 730.07 // Nut E3V2`
- Steps/mm
  - `#define DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 400, 93 }  // Ender Configs`
  - `#define DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 400, 415 }  // Nut E3V2`
- Max feedrate
  - `#define DEFAULT_MAX_FEEDRATE          { 500, 500, 10, 45 }`
  - `#define DEFAULT_MAX_FEEDRATE          { 600, 600, 25, 180 }  // Nut E3V2`
- Max acceleration
  - `#define DEFAULT_MAX_ACCELERATION      { 500, 500, 100, 1000 }  // Ender Configs`
  - `#define DEFAULT_MAX_ACCELERATION      { 1000, 1000, 200, 10000 }  // Nut E3V2`
- Max acceleration edit values
  - `#define MAX_ACCEL_EDIT_VALUES       { 9999, 9999, 999, 9999 } // ...or, set your own edit limits  // MRiscoC allows higher limits`
  - `#define MAX_ACCEL_EDIT_VALUES       { 99999, 99999, 999, 99999 } // ...or, set your own edit limits  // MRiscoC allows higher limits`
- Default acceleration
  - `#define DEFAULT_ACCELERATION          500    // X, Y, Z and E acceleration for printing moves  // Ender Configs`
  - `#define DEFAULT_RETRACT_ACCELERATION  800    // E acceleration for retracts  // Ender Configs`
  - `#define DEFAULT_TRAVEL_ACCELERATION   1000    // X, Y, Z acceleration for travel (non printing) moves  // Ender Configs`
  - to
  - `#define DEFAULT_ACCELERATION          1000    // X, Y, Z and E acceleration for printing moves  // Nut E3V2`
  - `#define DEFAULT_RETRACT_ACCELERATION  1500    // E acceleration for retracts  // Nut E3V2`
  - `#define DEFAULT_TRAVEL_ACCELERATION   2000    // X, Y, Z acceleration for travel (non printing) moves  // Nut E3V2`
- Default jerk
  - `#define DEFAULT_XJERK 8.0  // Ender Configs`
  - `#define DEFAULT_YJERK 8.0  // Ender Configs`
  - `#define DEFAULT_ZJERK  0.4  // Ender Configs`
  - to
  - `#define DEFAULT_XJERK 16.0  // Ender Configs`
  - `#define DEFAULT_YJERK 16.0  // Ender Configs`
  - `#define DEFAULT_ZJERK  0.8  // Ender Configs`
- Nozzle to probe offset
  - `#define NOZZLE_TO_PROBE_OFFSET { -41.5, -7, 0 }  // MRiscoC BLTouch offset for support: https://www.thingiverse.com/thing:4605354 (z-offset = -1.80 mm)`
  - `#define NOZZLE_TO_PROBE_OFFSET { 45.5, 13, -1.5 }  // Ender 3 V2 BMG Mount https://www.thingiverse.com/thing:4441537 // Nut E3V2`
- Probe feedrate
  - `#define XY_PROBE_FEEDRATE (200*60)  // MRiscoC increase travel speed between probes`
  - `#define XY_PROBE_FEEDRATE (300*60)  // Nut E3V2`
- PREHEAT_BEFORE_PROBING
  - `#define PROBING_BED_TEMP     50`
  - `#define PROBING_BED_TEMP     60   // Nut E3V2`
- Bed size
  - `#define X_BED_SIZE 235  // MRiscoC Max usable bed size`
  - `#define Y_BED_SIZE 235  // MRiscoC Max usable bed size`
  - to
  - `#define X_BED_SIZE 230  // Nut E3V2 Max usable bed size`
  - `#define Y_BED_SIZE 222  // Nut E3V2 Max usable bed size`
- Travel limits
    - `#define X_MIN_POS 0  // MRiscoC Stock physical limit`
    - `#define Y_MIN_POS 0  // MRiscoC Stock physical limit`
    - `#define X_MAX_POS 248  // MRiscoC Stock physical limit`
    - `#define Y_MAX_POS 231  // MRiscoC Stock physical limit`
    - `#define Z_MAX_POS 250  // Ender Configs`
    - to
    - `#define X_MIN_POS -8   // Nut E3V2 Endstop position`
    - `#define Y_MIN_POS -8   // Nut E3V2 Endstop position`
    - `#define X_MAX_POS X_MIN_POS + 235  // Nut E3V2`
    - `#define Y_MAX_POS Y_MIN_POS + 230  // Nut E3V2`
    - `#define Z_MAX_POS 240  // Nut E3V2`
- Mesh Inset
  - `#define MESH_INSET 25              // Set Mesh bounds as an inset region of the bed  // MRiscoC Center mesh  `
  - `#define MESH_INSET 42              // Set Mesh bounds as an inset region of the bed  // Nut E3V2 Center mesh`
- Probing pattern
  - `//#define UBL_HILBERT_CURVE       // Use Hilbert distribution for less travel when probing multiple points`
  - `#define UBL_HILBERT_CURVE       // Use Hilbert distribution for less travel when probing multiple points`
- Preheat constants
  - `#define PREHEAT_1_LABEL       "PLA"`
    `#define PREHEAT_1_TEMP_HOTEND 195`
    `#define PREHEAT_1_TEMP_BED     60`
    `#define PREHEAT_1_TEMP_CHAMBER 35`
    `#define PREHEAT_1_FAN_SPEED     128 // Value from 0 to 255`

    `#define PREHEAT_2_LABEL       "ABS"`
    `#define PREHEAT_2_TEMP_HOTEND 240`
    `#define PREHEAT_2_TEMP_BED     90`
    `#define PREHEAT_2_TEMP_CHAMBER 35`
    `#define PREHEAT_2_FAN_SPEED     128 // Value from 0 to 255`

    `#define PREHEAT_3_LABEL       "PETG"`
    `#define PREHEAT_3_TEMP_HOTEND 230`
    `#define PREHEAT_3_TEMP_BED     80`
    `#define PREHEAT_3_FAN_SPEED   128`

    `#define PREHEAT_4_LABEL       "CUSTOM"`
    `#define PREHEAT_4_TEMP_HOTEND 190`
    `#define PREHEAT_4_TEMP_BED     50`
    `#define PREHEAT_4_FAN_SPEED   128`
  - to
  - `#define PREHEAT_1_LABEL       "PLA"`
    `#define PREHEAT_1_TEMP_HOTEND 150`
    `#define PREHEAT_1_TEMP_BED     60`
    `#define PREHEAT_1_TEMP_CHAMBER 35`
    `#define PREHEAT_1_FAN_SPEED    128 // Value from 0 to 255`

    `#define PREHEAT_2_LABEL       "ABS"`
    `#define PREHEAT_2_TEMP_HOTEND 150`
    `#define PREHEAT_2_TEMP_BED     90`
    `#define PREHEAT_2_TEMP_CHAMBER 35`
    `#define PREHEAT_2_FAN_SPEED    128 // Value from 0 to 255`

    `#define PREHEAT_3_LABEL       "PETG"`
    `#define PREHEAT_3_TEMP_HOTEND 150`
    `#define PREHEAT_3_TEMP_BED     80`
    `#define PREHEAT_3_FAN_SPEED   128`

    `#define PREHEAT_4_LABEL       "CUSTOM"`
    `#define PREHEAT_4_TEMP_HOTEND 150`
    `#define PREHEAT_4_TEMP_BED     60`
    `#define PREHEAT_4_FAN_SPEED   128`
- Skew Correction
  - `#define SKEW_CORRECTION`
  - `#define SKEW_CORRECTION_GCODE`
- 
#### Configuration_adv.h
- Linear Advance k factor [Use test G-Code generator](https://marlinfw.org/tools/lin_advance/k-factor.html)
  - `#define ADVANCE_K { 0.22 }    // (mm) Compression length per 1mm/s extruder speed, per extruder`
  - `#define ADVANCE_K 0.0        // (mm) Compression length applying to all extruders`
  - to
  - `#define ADVANCE_K { 0.05 }    // (mm) Compression length per 1mm/s extruder speed, per extruder // Nut E3V2`
  - `#define ADVANCE_K 0.05        // (mm) Compression length applying to all extruders // Nut E3V2`

#### Start G-Code
```
M140 S{first_layer_bed_temperature[0]} ; set bed temp and continue
G90       ; use absolute coordinates
M83       ; extruder relative mode
G28       ; home all axis
M190 R{first_layer_bed_temperature[0]} ; set bed temp and wait
;https://marlinfw.org/docs/features/unified_bed_leveling.html
G29 P1                             ; Do automated probing of the bed.
M104 S{first_layer_temperature[0]} ; set extruder temp and continue
G29 P3                             ; Extrapolate values for points that automated probing couldn’t reach.
G29 S1                             ; Save UBL mesh points to slot 1. (There is also 0)
G29 F 10.0                         ; Set Fade Height for correction at 10.0 mm.
G29 A                              ; Activate the UBL System.
M500                               ; Save the mesh to EEPROM
G1 Z10 F300                        ; Move Z to 10mm
G1 X-1.0 Y0 F3000                  ; Move to front left corner, off-bed for priming
M109 S{first_layer_temperature[0]} ; set final nozzle temp and wait
G1 Z0.28 F300                      ; Move Z to 0.28mm
G92 E0                             ; reset extruder
G1 X-1.0 Y140 E10 F1500            ; prime the nozzle
G1 X-1.7 Y140 F5000                ; wipe
G1 X-1.7 Y10 E10 F1200             ; prime the nozzle again
```

#### End G-Code
```
{if max_layer_z < max_print_height}G1 Z{z_offset+min(max_layer_z+2, max_print_height)} F600 ; Move print head up{endif}
G1 X5 Y{print_bed_max[1]*0.85} F{travel_speed*60} ; present print
{if max_layer_z < max_print_height-10}G1 Z{z_offset+min(max_layer_z+70, max_print_height-10)} F600 ; Move print head further up{endif}
{if max_layer_z < max_print_height*0.6}G1 Z{max_print_height*0.6} F600 ; Move print head further up{endif}
M140 S0 ; turn off heatbed
M104 S0 ; turn off temperature
M107 ; turn off fan
M84 X Y E ; disable motors
M500; Save z-offset
```

### \*\*\*Below variables are to be implemented later adter modding, currently not changed***

#### Configuration_adv.h
- TMC2225 can handle up to 1.4A RMS
- Line 2738 Stepper max 0.84A, Recommended 850 mA
  - `    #define X_CURRENT       800        // (mA) RMS current. Multiply by 1.414 for peak current.`
  - `    #define X_CURRENT       850        // (mA) RMS current. Multiply by 1.414 for peak current.`
- Line 2758 Stepper max 0.84A, Recommended 780 mA
  - `    #define Y_CURRENT       800`
  - `    #define Y_CURRENT       850`
- Line 2778 Stepper max 0.84A, Recommended 850 mA
  - `    #define Z_CURRENT       800`
  - `    #define Z_CURRENT       850`
- Line 2878 Stepper max 1A, Recommended 990 mA
  - `    #define E0_CURRENT      800`
  - `    #define E0_CURRENT      1000`
- Line 3043
  - `    #define STEALTHCHOP_XY`
  - `    //#define STEALTHCHOP_XY`
- Line 3044
  - `    #define STEALTHCHOP_Z`
  - `    //#define STEALTHCHOP_Z`
- Line 3051
  - `    #define STEALTHCHOP_E`
  - `    //#define STEALTHCHOP_E`
- Line 3220
  - `  //#define TMC_DEBUG`
  - `  #define TMC_DEBUG`

#### pins_CREALITY_V4.h
 - Add the following to the bottommost line
- `
// Added this section for TMC2208 uart control - Requires hardware modification
#if HAS_TMC_UART
#define X_SERIAL_TX_PIN PA14 // Reuse SCLK 'debug' header pin next to LCD socket, unpopulated on 4.2.7 boards
#define X_SERIAL_RX_PIN PA14
#define Y_SERIAL_TX_PIN PA13 // Reuse SWDIO 'debug' header pin next to LCD socket, unpopulated on 4.2.7 boards
#define Y_SERIAL_RX_PIN PA13
#define Z_SERIAL_TX_PIN PB2 // Use Unused Top Right header pin on LCD socket (Top View, SD card on top right)
#define Z_SERIAL_RX_PIN PB2
#define E0_SERIAL_TX_PIN PC6 // Use Unused Top Left header pin on LCD socket (Top View, SD card on top right)
#define E0_SERIAL_RX_PIN PC6
#define TMC_BAUD_RATE 19600
#endif
`
