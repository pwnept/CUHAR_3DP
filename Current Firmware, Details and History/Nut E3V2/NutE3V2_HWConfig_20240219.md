# CUHAR Ender-3 V2 Printer
## Hardware
- Extruder changed to BMG 3:1, default stepper, E steps need to be changed
- Triangle Labs CHC Pro hotend with ceramic heating element
- Creality V4.2.2

## Software configurations
 - Used Ender3V2S1-20240122 as base code
 - Used [Special_Configurations-main](https://github.com/mriscoc/Special_Configurations/tree/1f203219427769a8674613fe5657bb9468ff2431) to generate Ender3V2-422-BLTUBL-HomeOffs-IS-LA-MPC-Speaker

### Configuration Edits:
- Settings
  - Default
  - Changed

#### platformio.ini
- default_envs
  - `STM32F103RC_creality`
  - `STM32F103RE_creality`

#### Configuration.h
- #define STRING_CONFIG_H_AUTHOR
  - `Miguel Risco-Castillo (MRiscoC)`
  - `Nut E3V2`
- PREHEAT_BEFORE_PROBING
  - `#define PROBING_BED_TEMP     50`
  - `#define PROBING_BED_TEMP     60   // Nut E3V2`
- MPC constants
  - `#define MPC_BLOCK_HEAT_CAPACITY { 14.40 }           // (J/K) Heat block heat capacities.`
  - `#define MPC_SENSOR_RESPONSIVENESS { 0.2187 }         // (K/s per ∆K) Rate of change of sensor temperature from heat block`.
  - `#define MPC_AMBIENT_XFER_COEFF { 0.1257 }           // (W/K) Heat transfer coefficients from heat block to room air with fan off.`
  - `#define MPC_AMBIENT_XFER_COEFF_FAN255 { 0.1315 }  // (W/K) Heat transfer coefficients from heat block to room air with fan on full.`
  to
  - `#define MPC_BLOCK_HEAT_CAPACITY { 5.42 }           // (J/K) Heat block heat capacities.`
  - `#define MPC_SENSOR_RESPONSIVENESS { 0.4040 }         // (K/s per ∆K) Rate of change of sensor temperature from heat block.`
  - `#define MPC_AMBIENT_XFER_COEFF { 0.0476 }           // (W/K) Heat transfer coefficients from heat block to room air with fan off.`
  - `#define MPC_AMBIENT_XFER_COEFF_FAN255 { 0.0603 }  // (W/K) Heat transfer coefficients from heat block to room air with fan on full.`
- PID constants
  - `#define DEFAULT_bedKp 462.10`
  - `#define DEFAULT_bedKi  85.47`
  - `#define DEFAULT_bedKd 624.59`
  to
  - `#define DEFAULT_bedKp 285.50`
  - `#define DEFAULT_bedKi  55.76`
  - `#define DEFAULT_bedKd 974.51`
- Steps/mm
  - `#define DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 400, 93 }  // Ender Configs`
  - `#define DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 400, 415 }  // Ender Configs  // Nut E3V2`
- Max feedrate
  - `#define DEFAULT_MAX_FEEDRATE          { 500, 500, 10, 45 }`
  - `#define DEFAULT_MAX_FEEDRATE          { 500, 500, 25, 60 }  // Nut E3V2`
- Mesh Inset
  - `#define MESH_INSET 25              // Set Mesh bounds as an inset region of the bed  // MRiscoC Center mesh  `
  - `#define MESH_INSET 46              // Set Mesh bounds as an inset region of the bed  // Nut E3V2 Center mesh`


- Nozzle to probe offset
  - `#define NOZZLE_TO_PROBE_OFFSET { -41.5, -7, 0 }  // MRiscoC BLTouch offset for support: https://www.thingiverse.com/thing:4605354 (z-offset = -1.80 mm)`
  - `#define NOZZLE_TO_PROBE_OFFSET { 45.5, 13, -1 }  // Ender 3 V2 BMG Mount https://www.thingiverse.com/thing:4441537 // Nut E3V2`
- Probe Feedrate
  - `#define XY_PROBE_FEEDRATE (200*60)  // MRiscoC increase travel speed between probes`
  - `#define XY_PROBE_FEEDRATE (300*60)  // Nut E3V2`
- Travel limits
    `#define X_MIN_POS 0  // MRiscoC Stock physical limit`
    `#define Y_MIN_POS 0  // MRiscoC Stock physical limit`
    `#define X_MAX_POS 248  // MRiscoC Stock physical limit`
    `#define Y_MAX_POS 231  // MRiscoC Stock physical limit`
    to
    `#define X_MIN_POS -5  // MRiscoC Stock physical limit // Nut E3V2`
    `#define Y_MIN_POS -5  // MRiscoC Stock physical limit // Nut E3V2`
    `#define X_MAX_POS 230  // MRiscoC Stock physical limit // Nut E3V2`
    `#define Y_MAX_POS 225  // MRiscoC Stock physical limit // Nut E3V2`



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
`
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

#### Other values:
- Max acceleration 500, 500, 100, 1000
- Default jerk 8.0, 8.0, 0.4, 5.0
- Steps/mm 80.2, 80.4, 403.8, 413.0
- PID Hotend 21.02 1.55 71.46
- PID Bed 136.29 24.34 508.80
- Nozzle to probe offset 44.1, 26.4, 0
- Home offset -4.55 -15.6