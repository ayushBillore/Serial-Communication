# Device Control Using Serial Communication

## Project Overview

In this project, we'll set up a **device control system using two Arduino boards**. One Arduino (Master) will have a push button, and the other Arduino (Slave) will have an LED. When the button on the Master Arduino is pressed, the LED on the Slave Arduino will light up using the **I2C protocol** for communication.

We will cover:
- Circuit setup
- Connection steps
- Code for both Master and Slave

---

## Circuit Setup

### Components Needed:
- Two Arduino boards (one as Master, one as Slave)
- Push button
- LED
- 220-ohm resistor for the LED
- Breadboard and jumper wires

### Connection Steps:

**1. Power Supply:**
- Connect the 5V and GND pins of both Arduinos to the power rails of the breadboard.
- Ensure a common ground connection between both Arduinos.

**2. Push Button Connection:**
- Connect one pin of the push button to digital pin 4 of the Master Arduino.
- Connect the other pin of the push button to the ground.

**3. LED Connection:**
- Connect the anode (longer leg) of the LED to digital pin 4 of the Slave Arduino through a 220-ohm resistor.
- Connect the cathode (shorter leg) of the LED to the ground.

**4. I2C Connection:**
- Connect the A4 pin (SDA) of the Master Arduino to the A4 pin (SDA) of the Slave Arduino (blue wire).
- Connect the A5 pin (SCL) of the Master Arduino to the A5 pin (SCL) of the Slave Arduino (brown wire).

---

## I2C Communication Setup

### Code for Master Arduino

```cpp
#include <Wire.h>
const int pushbutton = 4;
int buttonState = 0;
int x = 0;

void setup() {
  pinMode(pushbutton, INPUT);
  // Join I2C bus as master
  Wire.begin();
  Serial.begin(9600);
}

void loop() {
  buttonState = digitalRead(pushbutton);
  if (buttonState == HIGH) {
    x = 1;
  } else {
    x = 0;
  }
  Wire.beginTransmission(1); // Transmit to device #1
  Wire.write(x);
  Wire.endTransmission();    // Stop transmitting
  delay(500);                // Wait for half a second
}
```

#### Explanation of Master Code:
- `#include <Wire.h>` includes the I2C communication library.
- `const int pushbutton = 4;` defines the push button pin.
- `Wire.begin();` initializes I2C communication.
- `Serial.begin(9600);` initializes serial communication (for debugging).
- The push button state is read, and its value (`1` or `0`) is sent to the Slave Arduino using I2C.

---

### Code for Slave Arduino

```cpp
#include <Wire.h>
const int led = 4;
int x = 0;

void setup() {
  pinMode(led, OUTPUT);
  // Join I2C bus with address #1
  Wire.begin(1);
  Wire.onReceive(receiveEvent);
}

void receiveEvent(int bytes) {
  x = Wire.read(); // Receive byte as an integer
  if (x == 1) {
    digitalWrite(led, HIGH);
  } else {
    digitalWrite(led, LOW);
  }
}

void loop() {
  delay(20); // Small delay to keep the loop running smoothly
}
```

#### Explanation of Slave Code:
- `#include <Wire.h>` includes the I2C communication library.
- `const int led = 4;` defines the LED pin.
- `Wire.begin(1);` sets up the Arduino as a Slave with address `1`.
- `Wire.onReceive(receiveEvent);` attaches a function to handle incoming data.
- In `receiveEvent()`, if the received data is `1`, the LED turns on; otherwise, it turns off.

---

## Simulation Images

**Before running the program:**
<img width="718" height="469" alt="image" src="https://github.com/user-attachments/assets/cfd55075-f3b2-4feb-9f87-f93426c74e3d" />


**After running the program:**
<img width="854" height="479" alt="image" src="https://github.com/user-attachments/assets/85a456b6-aad3-4709-a903-01900c32e689" />

---

## Conclusion

This project demonstrates how to control a device (LED) using **serial communication between two Arduino boards** via the **I2C protocol**. When the push button on the Master Arduino is pressed, it sends a signal to the Slave Arduino, which turns the LED on. This setup can be extended to more complex systems requiring inter-Arduino communication and device control.

---
## Link

https://www.tinkercad.com/things/4xZK8aVe5Yo-spectacular-juttuli?sharecode=UwJLzR9HQuZ5YKGdDX2AkUsyNsp1BonQdcID2n6eLf4
