# CUHAR Ender-3 V2 Printer
## Hardware
- Replaced Z axis aluminum extrusion with 300mm long pair
- Extruder changed to BMG 3:1 with big stepper, E steps need to be changed
- Added foam and aluminum foil insulation under the heated bed
- Hotend and Temperature Probe changed - Ask Patrick
  
- Creality V2.4.7 board modifications (Board is oriented with sd card slot top-right):
  - With a cutter knife, cut the DIAG pin trace on the right vertical line of the "U" loop from X, Y, Z, and E TMC2225 stepper drivers that connect it to the PDN_UART signal
  - Solder to PDN_UART signal 
    - (Resistor Name, Stepper Signal, MCU Pin, Soldering Location)
    - R49, X stepper driver side, to A14, SWCLK on ICSP port (pin 3)
    - R50, Y stepper driver side, to A13, SWDIO on ICSP port (pin 2)
    - R51, Z stepper driver side, to B2, Top Right of LCD connector (pin 5), unused by DWIN screen
    - R51, E stepper driver side, to C6, Top Left of LCD connector (pin 6), unused by DWIN screen
  - Test continuity by checking if PDN_UART pin (pin 17) is connected to the other side of the jumper wire
  - Also check that the DIAG pin is cut

## Software configurations
 - Using Ender3V2-427-BLTUBL-LA-MPC as base

### Configuration Edits:
- Upper line is default, lower line is changed

#### platformio.ini
- Line 16
  - `STM32F103RC_creality`
  - `STM32F103RE_creality`

#### Configuration.h
- Line 66 Config Author STRING_CONFIG_H_AUTHOR
- Line 142 CUSTOM_MACHINE_NAME - "CUHAR Ender3V2-427-BLTUBL-LA-MPC-IS-LOUD"`
- Line 1238
  - `#define DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 400, 93 }  // Ender Configs`
  - `#define DEFAULT_AXIS_STEPS_PER_UNIT   { 80.2, 80.4, 403.8, 413.0 }  // CUHAR Ender Configs`
- Line 1245
  - `#define DEFAULT_MAX_FEEDRATE          { 300, 300, 25, 60 }  // Ender Configs`
  - `#define DEFAULT_MAX_FEEDRATE          { 500, 500, 25, 60 }  // CUHAR Ender Configs`
- Line 1553
  - `#define NOZZLE_TO_PROBE_OFFSET { -41.5, -7, 0 }  // MRiscoC BLTouch offset for support: https://www.thingiverse.com/thing:4605354 (z-offset = -1.80 mm)`
  - `#define NOZZLE_TO_PROBE_OFFSET { 44.1, 26.4, 0 }  // CUHAR E3V2 BLTouch Mount`
- Line 1560
  - `#define XY_PROBE_FEEDRATE (200*60)  // MRiscoC increase travel speed between probes`
  - `#define XY_PROBE_FEEDRATE (300*60)  // CUHAR increase travel speed between probes`

#### Configuration_adv.h
  - Line 1089
    - `//#define INPUT_SHAPING_X`
    - `#define INPUT_SHAPING_X`
  - Line 1090
    - `//#define INPUT_SHAPING_Y`
    - `#define INPUT_SHAPING_Y`
  - Line 2306
    - `//#define BEZIER_CURVE_SUPPORT        // Requires ~2666 bytes`
    - `#define BEZIER_CURVE_SUPPORT        // Requires ~2666 bytes`
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
