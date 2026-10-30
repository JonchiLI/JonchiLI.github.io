---
title: ArduinoGameController
---

# Innovator Journal
# Arduino Joystick-Controlled Catch the Star

> **Project Overview:** Turning a simple computer game into a physical interactive experience using an Arduino Uno, an analog joystick, and Python.

---

## 01 | Project Plan

### The Idea

For this project, I wanted to take my original Catch the Star game and make it more interactive. Instead of controlling the player with only a keyboard, I decided to use a physical joystick connected to an Arduino Uno.

My goal was to create a system where moving the joystick would directly control the player on the computer screen.

### Sensor Selection

I chose an analog joystick module (KY-023) as my input device.

The joystick has two axes, X and Y, which measure its horizontal and vertical positions. Since the player in my game only needs to move left and right, I mainly used the X-axis.

I expected this sensor to work well because the Arduino can continuously measure the joystick's position and send the readings to Python. Python can then interpret those readings as movement commands.

### System Design

The planned communication process was:

1. The joystick detects physical movement.
2. The Arduino reads the joystick's analog values.
3. The Arduino sends the values to the computer through USB serial communication.
4. Python receives the values and translates them into player movement.
5. The player moves across the screen to catch falling stars.

### Hardware Connections

| Joystick Pin | Arduino Uno |
|---|---|
| GND | GND |
| VCC | 5V |
| VRx | A0 |
| VRy | A1 |
| SW | D2 (optional) |

**Wiring Reference:**

![Arduino Uno and Joystick Wiring](images/joystick-wiring.png)

---

## 02 | Development Process

### Step 1 — Reading Joystick Values with Arduino

The first challenge was getting the Arduino to recognize the joystick's movement.

I connected the joystick's VRx pin to analog input A0. Using the `analogRead()` function, the Arduino could measure the joystick's horizontal position.

The readings ranged from approximately 0 to 1023:

- **0** represented one extreme of the joystick.
- **512** represented approximately the center.
- **1023** represented the opposite extreme.

I then used `Serial.println()` to send these readings to my computer.

**Arduino Code:**

```cpp
const int joystickX = A0;

void setup() {
    Serial.begin(9600);
}

void loop() {
    int xValue = analogRead(joystickX);

    // Send joystick position to the computer
    Serial.println(xValue);

    delay(20);
}
```

This allowed me to monitor the joystick's readings through the Arduino Serial Monitor and confirm that the sensor was working.

**Serial Monitor Reference:**

![Arduino Serial Monitor Showing Joystick Values](images/serial-monitor.png)

### Step 2 — Establishing Communication with Python

After getting the Arduino to send numerical values, I needed Python to receive them.

I used the PySerial library to establish communication between the Arduino and my computer through a USB serial connection.

On Windows, I identified the Arduino's COM port through the Arduino IDE.

**Python Code:**

```python
import serial

# Connect to Arduino through its COM port
arduino = serial.Serial("COM3", 9600)

while True:
    line = arduino.readline().decode().strip()

    if line:
        x = int(line)
        print(x)
```

This program continuously received the values sent by the Arduino and displayed them in Python.

The important part was making sure that both devices used the same baud rate, which was 9600 in this project.

### Step 3 — Translating Sensor Data into Game Controls

Receiving numbers was not enough. I needed to make those numbers meaningful to the game.

I divided the joystick's readings into three ranges:

| Joystick Reading | Game Action |
|---|---|
| Below 400 | Move Left |
| 400–600 | Stay Still |
| Above 600 | Move Right |

I used a range around the center instead of one exact value because analog readings can fluctuate slightly even when the joystick is not moving.

Python used these conditions to determine the player's direction and update its horizontal position.

### Step 4 — Integrating the Joystick into Catch the Star

Once the Arduino and Python could communicate successfully, I integrated the joystick input into the game.

The final game included:

- **Joystick Movement:** Physically move the joystick to control the player.
- **Falling Stars:** Catch stars as they fall from the top of the screen.
- **Scoring System:** Earn points for every star caught.
- **Increasing Difficulty:** The game becomes harder as the score increases.
- **Game Over:** The game ends when a star is missed.
- **Keyboard Backup:** Keyboard controls remain available as an alternative.

**Game Development Reference:**

![Python Game During Development](images/game-development.png)

### Step 5 — How I Used AI

I used ChatGPT as a learning assistant throughout this project.

It helped me:

- Understand how Arduino and Python communicate.
- Learn how to use the PySerial library.
- Understand unfamiliar functions and sections of code.
- Troubleshoot the hardware-software connection.
- Organize the development process and explain the logic behind my code.

Rather than simply copying code, I used AI explanations to understand how each component worked and how the different parts of the project connected.

---

## 03 | Final Result

### The Finished Project

The final result was a Python version of Catch the Star controlled by a physical Arduino joystick.

When I moved the joystick, the Arduino read its horizontal position and transmitted the corresponding numerical value to Python. Python interpreted that value and moved the player in the corresponding direction.

This created a connection between a physical input device and a digital game.

**Final Game Screenshot:**

![Final Game Screenshot](images/final-game.png)

**Arduino Joystick Setup:**

![Arduino Joystick Setup](images/arduino-setup.png)

### Project Demonstration

- **Demo Video:** [Insert your video link here]
- **Source Code:** [Insert your GitHub repository link here]

---

## 04 | Reflection

### What I Learned

This project helped me understand how hardware and software can communicate with each other.

Before starting, I mostly thought of game controls as inputs from a keyboard or mouse. Through this project, I learned that external hardware, such as an Arduino and joystick, can also provide input to a computer program.

Some of the most important concepts I learned were:

- **Analog Input:** How sensors convert physical movement into numerical readings.
- **Serial Communication:** How two devices exchange information through a connection.
- **Python Libraries:** How external libraries such as PySerial allow Python to communicate with hardware.
- **Data Interpretation:** How raw numerical readings can be translated into meaningful actions.
- **Hardware-Software Integration:** How separate components can work together as one interactive system.

### Final Thoughts

The most interesting part of this project was seeing how a simple physical movement could produce an immediate response inside a computer game.

I also gained a better understanding of what an interface actually is. It is not always a graphical interface that people interact with on a screen. It can also be the communication system that connects different devices and allows them to work together.

Overall, this project gave me a practical introduction to combining programming with physical hardware, and it made me interested in exploring more interactive projects in the future.
