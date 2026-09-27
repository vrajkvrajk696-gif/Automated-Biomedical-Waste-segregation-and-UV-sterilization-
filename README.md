# Automated-Biomedical-Waste-segregation-and-UV-sterilization-
Automated Biomedical Waste Management Using Arduino and UV Sterilization is a smart system that automates waste detection, sterilization, and segregation. Arduino UNO, sensors, conveyor belt, servo motor, and UV sterilization are integrated to reduce manual handling, improve hygiene, enhance safety, and efficiently separate biomedical waste.
#include <Servo.h>

#define moistPin 8   // Moisture sensor digital pin (DO)
#define trig 3       // Ultrasonic trigger pin
#define echo 4       // Ultrasonic echo pin

Servo myServo;

void setup() {
  Serial.begin(9600);

  pinMode(moistPin, INPUT);
  pinMode(trig, OUTPUT);
  pinMode(echo, INPUT);

  myServo.attach(10);   // Servo signal wire connected to pin 10
  myServo.write(90);    // Start at base position (90°)
  delay(3000);
}

void loop() {
  // --- Ultrasonic sensor reading ---
  long duration, distance;
  digitalWrite(trig, LOW);
  delayMicroseconds(2);
  digitalWrite(trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(trig, LOW);

  duration = pulseIn(echo, HIGH);
  distance = duration * 0.034 / 2; // Convert to cm

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // --- Gatekeeper logic ---
  if (distance > 0 && distance <= 20) {
    Serial.println("Object Detected");

    int moistValue = digitalRead(moistPin);
    Serial.print("Moisture Digital Value: ");
    Serial.println(moistValue);

    if (moistValue == HIGH) {
      // Soil is dry → move 90° → 150° and back
      Serial.println("Soil is Dry");
      for (int angle = 90; angle <= 150; angle++) {
        myServo.write(angle);
        delay(15);
      }
      for (int angle = 150; angle >= 90; angle--) {
        myServo.write(angle);
        delay(15);
      }
    } else {
      // Soil is wet → move 90° → 30° and back
      Serial.println("Soil is Wet°");
      for (int angle = 90; angle >= 30; angle--) {
        myServo.write(angle);
        delay(15);
      }
      for (int angle = 30; angle <= 90; angle++) {
        myServo.write(angle);
        delay(15);
      }
    }
  } else {
    // No object → moisture sensor ignored, servo stays at base
    Serial.println("No Object");
    myServo.write(90);
  }

  delay(2000);
}
