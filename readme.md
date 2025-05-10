**Bluetooth Controlled Car**

A simple Arduino-powered car you can steer via Bluetooth using the "Arduino Car" Android app (by One Day of Code) or a custom Python script with `pySerial`. This project demonstrates wireless motor control with a dual H-bridge (L298N) driver.

---

## 🚗 Features

* **Forward / Backward**: Drive straight ahead or reverse.
* **Left / Right Turns**: Pivot the car by driving one side only.
* **Diagonal Movement**: Support for forward-right, forward-left, backward-right, backward-left.
* **Speed Adjustment**: Increase or decrease speed via app commands.
* **Electronic Braking**: Brief reverse applied for quick stops.
* **Configurable Offsets**: Fine-tune motor speed balance with offset values.

---

## 🔌 Hardware Connections

| Component            | Pin / Connection   | Purpose                   |
| -------------------- | ------------------ | ------------------------- |
| L298N IN1            | Arduino 11         | Motor A direction control |
| L298N IN2            | Arduino 10         | Motor A direction control |
| L298N IN3            | Arduino 9          | Motor B direction control |
| L298N IN4            | Arduino 8          | Motor B direction control |
| L298N ENA (Enable A) | Arduino 5 (PWM)    | Motor A speed control     |
| L298N ENB (Enable B) | Arduino 6 (PWM)    | Motor B speed control     |
| Bluetooth GND        | Arduino GND        | Module ground             |
| Bluetooth VCC        | Arduino 5V         | Module power              |
| Bluetooth TX         | Arduino RX (pin 0) | Serial data from module   |

> **Tip:** Orient the L298N so the heat sink is facing you, with IN1–IN4 pins in order.

---

## ⚙️ Software Setup

1. **Install Arduino IDE**:

   * Download and install from [arduino.cc](https://www.arduino.cc).
2. **Clone or Copy** the provided `BluetoothCar.ino` into your sketchbook.
3. **Install Python (Optional)**:

   * If using a Python controller, install `pySerial` with:

     ```bash
     pip install pyserial
     ```
4. **Pair Bluetooth**:

   * Open your phone’s Bluetooth settings, pair with the HC-05/HC-06 module (default PIN `1234`).
5. **Upload Sketch**:

   * Select the correct COM port and Arduino board, then upload the sketch.

---

## 📝 Code Overview

```cpp
#define SERIAL_RATE 9600      // Baud rate for Bluetooth
#define FRONT 'F'             // Forward command
#define BACK 'B'              // Reverse command
#define RIGHT 'R'             // Turn right
#define LEFT 'L'              // Turn left
#define STOP 'S'              // Stop / Brake
#define ENABLE_A 5            // PWM pin for Motor A
#define ENABLE_B 6            // PWM pin for Motor B

int m1a = 8, m1b = 9;        // Motor A control pins
int m2a = 10, m2b = 11;      // Motor B control pins
int speed = 100;             // Base speed (0-255)
int brakeForce = 100;        // Braking pulse duration (ms)
int m1SpeedOffset = 0;       // Motor A speed calibration
int m2SpeedOffset = 0;       // Motor B speed calibration

void setup() {
  analogWrite(ENABLE_A, speed + m1SpeedOffset);
  analogWrite(ENABLE_B, speed + m2SpeedOffset);
  pinMode(m1a, OUTPUT); pinMode(m1b, OUTPUT);
  pinMode(m2a, OUTPUT); pinMode(m2b, OUTPUT);
  Serial.begin(SERIAL_RATE);
}

void loop() {
  while (Serial.available() > 0) {
    analogWrite(ENABLE_A, speed + m1SpeedOffset);
    analogWrite(ENABLE_B, speed + m2SpeedOffset);
    char cmd = Serial.read();
    moveMotor(cmd);
  }
}
```

Key functions:

* **`moveMotor(char dir)`**: Sets motor direction or adjusts `speed`.
* **`brakeMotor()`**: Briefly reverses and stops motors for quick braking.

---

## 📱 Controlling the Car

### Android App

1. Install **Arduino Car** by *One Day of Code*.
2. Connect to your HC-05/06 module.
3. Use on-screen buttons to send commands:

   * **F**: Forward
   * **B**: Backward
   * **L**: Left
   * **R**: Right
   * **S**: Stop
   * **G/H/I/J**: Diagonal movements
   * **X/Y**: Speed up/down


```

Adjust `'COM3'` to your port and send the same command characters.

---

## 🛠️ Calibration & Tuning

* **Speed Offsets**: Modify `m1SpeedOffset` and `m2SpeedOffset` to correct drift.
* **Brake Force**: Tweak `brakeForce` duration for sharper or softer stops.
* **Base Speed**: Change `speed` to set your car’s default velocity.

---

## 🤝 Contributing

Feel free to fork, improve movement logic, or integrate sensors (e.g., ultrasonic for obstacle avoidance).

---

## 📝 License

Released under the MIT License © Liladhar Bhatta
