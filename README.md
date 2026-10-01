# Smart Parking System

An Arduino UNO based six-slot parking prototype documented for Project Based Learning (5th semester). The repository preserves the submitted Arduino sketch and report. The README describes only behavior visible in those sources.

## Repository contents

- `Arduino/smart_parking_system.ino` - running Arduino sketch supplied with the project.
- `Documentation/Smart_Parking_System_Report.pdf` - original 35-page project report.
- `Proteus/Proteus Simulation.pdsprj` - supplied Proteus project package.
- `Hardware/images/` - reserved for prototype photos; no separate image files were included. The report includes a hardware implementation image (page 32).

## Hardware described by the report and sketch

- Arduino UNO
- Six parking-slot IR sensors
- Entry and exit IR sensors
- 20x4 I2C LCD, configured at address `0x27`
- Servo motor used as the gate actuator
- Prototype parking model with six slots/cars, shown in the report

The sketch does not define sensor polarity electrically beyond treating a LOW (`0`) reading as detected/occupied. Confirm the specific sensor module output behavior and wiring before powering the circuit. The report may describe additional parts; see [Source consistency notes](#source-consistency-notes).

## Pin connections from the sketch

| Arduino pin | Connection |
| --- | --- |
| D2 | Entry IR sensor (`ir_enter`) |
| D3 | Servo signal |
| D4 | Exit IR sensor (`ir_back`) |
| D5 | Slot 1 IR sensor |
| D6 | Slot 2 IR sensor |
| D7 | Slot 3 IR sensor |
| D8 | Slot 4 IR sensor |
| D9 | Slot 5 IR sensor |
| D10 | Slot 6 IR sensor |
| A4 (SDA), A5 (SCL) | I2C LCD on Arduino UNO, per standard UNO I2C pins |
| 5V, GND | Power/ground as required by modules; wiring specifics are not included in the sketch |

Use the LCD backpack's actual I2C address; this sketch expects `0x27`. The sketch does not provide a wiring diagram or sensor model.

## Software

- Arduino IDE
- Arduino AVR Boards support for Arduino UNO
- `Servo` library
- `Wire` library (included with Arduino IDE)
- `LiquidCrystal_I2C` library (install a compatible version if not already available)

## Working principle in the supplied code

At startup, the sketch reads all six slot sensors. A slot is treated as occupied when its sensor reads LOW; that initial occupied count is subtracted from the capacity of six. During the main loop, the LCD shows the stored available-slot count and each slot as `Fill` or `Empty`. When the entry sensor reads LOW, the sketch opens the servo to 180 degrees and decrements the count if it is above zero. When the exit sensor reads LOW, it opens the servo and increments the count. The servo is returned to 90 degrees only when the entry and exit flags are both set, after a one-second delay.

## Setup and run

1. Install Arduino IDE and the Arduino UNO board package.
2. Install a compatible `LiquidCrystal_I2C` library. `Servo` and `Wire` are included with the standard Arduino environment.
3. Open `Arduino/smart_parking_system.ino`.
4. Connect the components according to the pin table and your modules' voltage requirements. Verify the LCD address and sensor output polarity.
5. Select **Arduino Uno** and the correct serial port in the IDE.
6. Compile and upload the sketch. The sketch starts the serial port at 9600 baud, initializes the LCD, samples the slot sensors, and then updates the display in its loop.

## Proteus simulation

The report says the project was implemented/simulated using Proteus and Arduino IDE. The supplied `Proteus/Proteus Simulation.pdsprj` is included. It is a packaged Proteus project file containing project data; the simulation has not been opened or verified in Proteus here, so confirm it loads with your Proteus version and any required libraries.

## Project results

The report describes a six-slot physical prototype and reports that the system detects empty/occupied slots, displays availability, and operates the gate during entry/exit. Its conclusion claims reduced parking search/waiting time, but no numerical timing measurements or test dataset are provided. The report's hardware implementation photograph appears on page 32.

## Source consistency notes

These discrepancies are retained and documented rather than silently corrected:

- The report's abstract and other passages mention ultrasonic sensors, while the supplied sketch reads IR sensors for the entry, exit, and six parking slots.
- The report discusses slot LEDs, but the supplied sketch controls an LCD and servo and has no LED pin definitions or LED logic.
- The report mentions digital payment and other broad smart-parking capabilities; those functions do not appear in the supplied sketch or demonstrated hardware description.
- The sketch calculates the initial available count from slot sensors once in `setup()`. Later sensor reads update the displayed per-slot states but do not recalculate the available count. Entry/exit events change the count independently, so the number can drift from actual occupancy.
- The entry/exit flags are cleared only when both are set. If only an entry or only an exit event occurs, its flag remains set, affecting subsequent gate events. This is the behavior of the supplied code as written.
- The report does not provide enough detail to confirm exact sensor wiring, sensor polarity, Proteus project configuration, or measured performance.

## Source

Prepared from the supplied running sketch and the report titled **Smart Parking System**, submitted as a Project Based Learning report in November 2024. The original report is included in `Documentation/`.

