# GestureAirDraw

GestureAirDraw is an Arduino library that lets you **draw in the air** using an IMU (MPU6050 / MPU9250). The library computes pitch/roll using a complementary filter, maps orientation to 2D coordinates, records strokes, and exports **SVG** polylines over Serial or to an SD card (optional).

## Features
- Read raw accel + gyro via I2C (Wire)
- Complementary filter for stable orientation
- Smooth position mapping (calibration with initial pose)
- Gesture start/stop via a button or automatic threshold
- Export SVG path to Serial or SD (optional)
- Example sketch for Arduino/PlatformIO

## Wiring (MPU6050 typical)
- VCC -> 3.3V or 5V (board dependent)
- GND -> GND
- SDA -> A4 (Uno) or SDA pin on your board
- SCL -> A5 (Uno) or SCL pin on your board
- Optional Start/Stop button -> D2 (pulled to GND; library uses INPUT_PULLUP)

See `extras/wiring.png` for a simple diagram.

## Quickstart
1. Connect your IMU as above.
2. Open `examples/Basics/Basics.ino`
3. Upload, open Serial at 115200, press the button to start/stop recording.
4. When stopped, the sketch prints an SVG string. Copy into a file `drawing.svg` and open in a browser.

## API (short)
```cpp
#include <GestureAirDraw.h>
GestureAirDraw g;
g.begin(); // optionally pass Wire and settings
g.update(); // call in loop
g.startRecording();
g.stopRecording();
g.exportSVG(Serial);
```

## Notes & Limitations
- This is a physics-free mapping (orientation→screen). It intentionally avoids double-integrating acceleration to limit drift. For true 3D tracking, use external sensors or SLAM.
- For better accuracy, attach the IMU rigidly to the drawing object and stand in front of a neutral calibration pose when starting the library.
