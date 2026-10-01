Programming
I used examples and resources I found online to learn how to control the stepper motor and ultrasonic sensor. I did not write the entire program from scratch. Instead, I studied existing Arduino examples and tutorials, especially examples using the Stepper library, and adapted the code to work with my own hardware.
I learned how the Arduino can control the stepper motor, trigger the HC-SR04, measure the echo time, and convert that time into a distance. I then combined these ideas into a program for my scanner.
The main process is:
Rotate motor
     ↓
Measure distance
     ↓
Record angle + distance
     ↓
Rotate again
     ↓
Measure again
One of the main things I learned was how the different parts of the code connect together. For example, the stepper motor controls the position of the sensor, while the ultrasonic sensor collects information about what is in front of it. The Arduino coordinates both of these processes.
Here is the final Arduino code:
#include <Stepper.h>
const int stepsPerRevolution = 2048;
// ULN2003: IN1, IN3, IN2, IN4 order for Stepper library
Stepper motor(stepsPerRevolution, 2, 4, 3, 5);
const int trigPin = 9;
const int echoPin = 10;
void setup() {
 Serial.begin(9600);
 pinMode(trigPin, OUTPUT);
 pinMode(echoPin, INPUT);
 motor.setSpeed(10);
 Serial.println("Angle,Distance(cm)");
}
float getDistance() {
 digitalWrite(trigPin, LOW);
 delayMicroseconds(2);
 digitalWrite(trigPin, HIGH);
 delayMicroseconds(10);
 digitalWrite(trigPin, LOW);
 long duration = pulseIn(echoPin, HIGH, 30000);
 if (duration == 0) {
   return -1;
 }
 float distance = duration * 0.0343 / 2;
 return distance;
}
void loop() {
 for (int angle = 0; angle <= 180; angle += 5) {
   int steps = (int)(2048.0 * 5.0 / 360.0);
   motor.step(steps);
   delay(100);
   float distance = getDistance();
   Serial.print(angle);
   Serial.print(",");
   Serial.println(distance);
   delay(100);
 }
 for (int angle = 180; angle > 0; angle -= 5) {
   int steps = (int)(2048.0 * 5.0 / 360.0);
   motor.step(-steps);
   delay(5);
 }
 delay(1000);
}
I also used the Arduino Stepper library examples as a reference, especially the one-step-at-a-time example, to understand how the motor could be controlled. I then modified the example for my motor and added the ultrasonic distance-measuring code.
