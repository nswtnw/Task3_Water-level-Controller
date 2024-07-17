# Water level Controller Using Ultrasonic Sensor with ESP32

## Introduction
The "Water Level Controller Using Ultrasonic Sensor with ESP32" project aims to monitor and control the water level in a tank using an ultrasonic sensor, ESP32 microcontroller, LEDs for level indication, and a motor to manage the water flow. The ultrasonic sensor measures the distance to the water surface, and based on the measured level, different LEDs are activated, and the motor is turned on or off accordingly.

## Components
1. ESP32 Microcontroller: The main control unit of the project.
2. HC-SR04 Ultrasonic Sensor: Used to measure the distance to the water surface.
3. 4 LEDs:
   * Low Level LED: Indicates a low water level.
   * Medium Level LED: Indicates a medium water level.
   * High Level LED: Indicates a high water level.
   * Motor Indicator LED: Represents the motor status.
4. Relay Module: Controls the motor based on the water level.
5. 4 Resistors: Current limiting for the LEDs.

## Circuit Digram 
![image](https://github.com/user-attachments/assets/7317d5db-8f8f-4a7a-b0a5-f4c62e2b689f)

## Pin Connections
* HC-SR04 Ultrasonic Sensor:
  * VCC to 5V
  * GND to GND
  * Trig to GPIO 26 (PIN_TRIG)
  * Echo to GPIO 25 (PIN_ECHO)
* LEDs:
  * Low Level LED: GPIO 18 (LOWLED)
  * Medium Level LED: GPIO 19 (MIDLED)
  * High Level LED: GPIO 21 (HIGHLED)
  * Motor Indicator LED: GPIO 27 (MOTOR)
* Relay Module:
  * VCC to 5V
  * GND to GND
  * IN to GPIO 27 (Motor)

## Code 
```
#define PIN_TRIG 26
#define PIN_ECHO 25
#define LOWLED 18
#define MIDLED 19
#define HIGHLED 21
#define MOTOR 27

unsigned int level = 0;

void setup() {
  pinMode(LOWLED, OUTPUT);
  pinMode(MIDLED, OUTPUT);
  pinMode(HIGHLED, OUTPUT);
  pinMode(MOTOR, OUTPUT);
  digitalWrite(LOWLED, HIGH);
  digitalWrite(MIDLED, HIGH);
  digitalWrite(HIGHLED, HIGH);
  digitalWrite(MOTOR, LOW);

  Serial.begin(115200);
  pinMode(PIN_TRIG, OUTPUT);
  pinMode(PIN_ECHO, INPUT);
}

void loop() {
  // Start a new measurement:
  digitalWrite(PIN_TRIG, HIGH);
  delayMicroseconds(10);
  digitalWrite(PIN_TRIG, LOW);

  // Read the result:
  int duration = pulseIn(PIN_ECHO, HIGH);
  Serial.print("Distance in CM: ");
  Serial.println(duration / 58);
  Serial.print("Distance in inches: ");
  Serial.println(duration / 148);
  
  level = duration / 58;

  if (level < 100) {
    digitalWrite(LOWLED, LOW);
    digitalWrite(MOTOR, HIGH);
    digitalWrite(HIGHLED, HIGH);
    digitalWrite(MIDLED, HIGH);
  } else if ((level > 200) && (level < 400)) {
    digitalWrite(LOWLED, HIGH);
    digitalWrite(HIGHLED, HIGH);
    digitalWrite(MIDLED, LOW);
  } else if (level >= 400) {
    digitalWrite(HIGHLED, LOW);
    digitalWrite(MIDLED, HIGH);
    digitalWrite(LOWLED, HIGH);
    digitalWrite(MOTOR, LOW);
  }

  delay(1000);
}
```

## Explanation of the Code
1. Setup:
Pins are configured as inputs or outputs.
LEDs are initially turned off, and the motor is turned off.
Serial communication is initialized for debugging purposes.
The ultrasonic sensor's trigger and echo pins are set up.

2. Loop:
A pulse is sent from the trigger pin of the ultrasonic sensor.
The duration of the echo is measured to determine the distance.
The distance is calculated in centimeters and inches and printed to the serial monitor.
Based on the distance, the water level is categorized into three states:

* Low Level (< 100 cm):
The low-level LED is turned on, the motor indicator LED is turned on (indicating the motor is running), and the medium and high-level LEDs are turned off.

* Medium Level (200 cm < level < 400 cm): 
The medium-level LED is turned on, the motor indicator LED is turned off, and the low and high-level LEDs are turned off.

* High Level (>= 400 cm):
The high-level LED is turned on, the motor indicator LED is turned off, and the low and medium-level LEDs are turned off.
The loop repeats every second to continuously monitor and update the water level.

## Conclusion 
The "Water Level Controller Using Ultrasonic Sensor with ESP32" project demonstrates how to use an ultrasonic sensor to measure water levels and control outputs like LEDs and a motor based on the measured level.



