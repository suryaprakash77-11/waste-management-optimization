# waste-management-optimizationIOT-WASTE-MANAGEMENT-OPTIMIZATION.
Waste Management System Using ESP32 Overview This project implements an intelligent waste management system using an ESP32 microcontroller, ultrasonic sensors, a soil moisture sensor, and a servo motor. The system continuously monitors two types of trash cans — dry waste and wet waste — to detect when they are full and whether the trash is wet or dry.

The system automatically controls a gate (via the servo motor) to allow wet trash disposal and provides real-time feedback through the serial monitor, making it ideal for small-scale smart garbage management applications such as in smart cities or communities.

Features Ultrasonic Sensors to measure the level of trash in both dry and wet waste bins.

Soil Moisture Sensor to detect if the trash being disposed is wet or dry.

Servo Motor controls a gate to allow disposal of wet waste only when detected.

Full Trash Detection: Alerts (via serial print) when any trash bin is full.

Automatic Gate Control: Opens the gate for wet trash disposal and closes it after a delay.

Real-time Monitoring through Serial Monitor on a connected PC or display.

Hardware Components Component Quantity Description ESP32 Development Board 1 Microcontroller to run the system Ultrasonic Sensor (HC-SR04 or similar) 2 Measure distance to detect fullness of trash bins Soil Moisture Sensor 1 Detects moisture level in trash Servo Motor 1 Controls gate to allow wet trash disposal Connecting wires As needed For connecting sensors and servo to ESP32 Power supply 1 To power ESP32 and peripherals

Pin Configuration Peripheral ESP32 Pin Purpose Dry Bin Ultrasonic Trigger (Dry_Trig) GPIO 19 Trigger pin for dry bin ultrasonic sensor Dry Bin Ultrasonic Echo (Dry_Echo) GPIO 18 Echo pin for dry bin ultrasonic sensor Wet Bin Ultrasonic Trigger (Wet_Trig) GPIO 23 Trigger pin for wet bin ultrasonic sensor Wet Bin Ultrasonic Echo (Wet_Echo) GPIO 22 Echo pin for wet bin ultrasonic sensor Soil Moisture Sensor GPIO 33 Analog input for soil moisture measurement Servo Motor Control GPIO 21 PWM output to control servo motor position

How It Works Ultrasonic Distance Measurement Two ultrasonic sensors are used to measure the distance from the sensor to the surface of the trash inside each bin (dry and wet). The sensors send ultrasonic pulses, and the time it takes for the echo to return is converted into a distance in centimeters.

If the measured distance is less than 4 cm, the bin is considered full.

The system reports this status through the serial monitor.

Moisture Detection The soil moisture sensor reads analog voltage values indicating the moisture level of the trash being disposed.

The raw sensor values are calibrated and converted into a percentage moisture level.

If the moisture level exceeds a set threshold, the trash is classified as wet.

Servo Motor Gate Control If wet trash is detected (moisture level above threshold):

The servo motor rotates to open the gate (moves from 90° to 0°).

The gate remains open for 5 seconds to allow disposal.

The servo then returns to the closed position (90°).

This prevents dry trash from being disposed through the wet trash gate.

Software Design The program runs on the ESP32 and performs the following main tasks:

Initialization:

Sets pin modes for ultrasonic sensors, soil moisture sensor, and servo motor.

Attaches servo motor control to the designated GPIO pin.

Main Loop:

Reads soil moisture sensor and calibrates the value.

Checks if trash is wet by comparing moisture to threshold.

Controls servo motor to open/close gate for wet trash.

Reads distance from both ultrasonic sensors to check if dry or wet bins are full.

Prints status messages to the serial monitor for monitoring.

Waits 10 seconds before repeating the cycle to allow sensor stability and motor movement.

Functions:

calculate_distance(Trig, Echo, can) — calculates distance based on ultrasonic sensor pulses.

is_full(Trig, Echo, can) — returns true if the measured distance indicates a full bin.

calibrate(moisture) — converts raw moisture sensor readings into a percentage value.

Calibration and Thresholds The soil moisture sensor outputs analog values between approximately 1095 (dry) and 4095 (wet).

The code calibrates these values linearly to a 0-100% scale.

The wet trash threshold is dynamically calculated based on a high moisture value (4000), which sets when the system considers trash "wet".

The fullness threshold for ultrasonic sensors is set at 4 cm — bins are full if trash is within 4 cm of the sensor.

Usage Instructions Hardware Setup:

Connect ultrasonic sensors to the ESP32 pins as specified.

Connect the soil moisture sensor to the analog input.

Attach the servo motor to the designated GPIO pin.

Ensure power supply is stable and sufficient for servo and sensors.

Programming:

Open the Arduino IDE.

Select the ESP32 board.

Copy the provided source code into a new sketch.

Adjust WiFi credentials or remove if not needed.

Upload the program to your ESP32 board.

Monitoring:

Open the Serial Monitor at 115200 baud rate.

Observe real-time messages about moisture levels, bin fullness, and servo activity.

Operation:

The system will automatically open the wet trash gate when wet trash is detected.

It will continuously monitor bins and notify via serial print when any bin is full.

You can extend the code to integrate with other interfaces (LCD, LEDs, notifications) as needed.

Potential Improvements Add visual indicators (LEDs) for bin fullness.

Integrate an LCD display for local status updates.

Add WiFi or Bluetooth notifications.

Implement power-saving features for battery operation.

Scale up with additional sensors and bins.

Troubleshooting Servo motor not moving: Check servo power and PWM wiring. Confirm servo is correctly attached in code.

Incorrect moisture readings: Calibrate sensor using dry and wet known samples. Adjust max_moisture and min_moisture.

Ultrasonic sensor readings erratic: Ensure sensors face directly into the bins without obstructions. Check wiring and sensor orientation.

Serial Monitor shows no output: Verify baud rate is set to 115200. Confirm correct COM port is selected.

Code Snippet Highlights cpp Copy Edit float calculate_distance(int Trig, int Echo, char can){ digitalWrite(Trig, LOW); delay(2); digitalWrite(Trig, HIGH); delay(10); digitalWrite(Trig, LOW); int duration = pulseIn(Echo, HIGH); float distance = duration * 0.034 / 2; Serial.print(can); Serial.print(" free space : "); Serial.print(distance); Serial.println(" CM"); return distance; }

bool is_full(int Trig, int Echo, char can){ float distance = calculate_distance(Trig, Echo, can); return distance < 4; }

float calibrate(int moisture){ int x = (max_moisture - min_moisture) / 100; return (max_moisture - moisture) / x; } License This project is open source and free to use and modify under the MIT License.
