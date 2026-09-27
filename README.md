Automated Biomedical Waste segregation and UV sterilization-
📌 Project Overview------
Automated Biomedical Waste Management Using Arduino and UV Sterilization is a smart waste-management system designed to automate the detection, sterilization, and segregation of biomedical waste. The system uses an Arduino UNO, sensors, conveyor belt, servo motor, and UV sterilization to reduce manual handling and improve safety and hygiene.
🎯 Objectives------------
Automate biomedical waste segregation.
Reduce direct human contact with potentially contaminated waste.
Use UV sterilization before waste segregation.
Detect different characteristics of waste using sensors.
Improve the efficiency and safety of biomedical waste handling.
Develop a low-cost and compact automated system.
⚙️ Working Principle-----------
Biomedical waste is placed on the conveyor belt.
The conveyor transports the waste through the system.
The waste passes through a UV sterilization chamber.
Sensors detect the presence and characteristics of the waste.
Arduino UNO processes the sensor signals.
The servo motor operates the sorting mechanism.
Waste is directed into the appropriate collection category.
The segregated waste can then be handled or disposed of appropriately.
🔧 Components Used---------
Arduino UNO
Ultrasonic Sensor
Moisture Sensor
IR Sensor
Metal Sensor
Servo Motor
BO DC Motor
Conveyor Belt
UV Sterilization Lamp
PWM Motor Speed Controller
12V DC Adapter
Jumper Wires
🧠 Technologies Used--------
Arduino UNO
Embedded C/C++
Sensor Interfacing
Servo Motor Control
DC Motor Control
PWM Motor Speed Control
UV Sterilization
Automated Waste Segregation
📊 Applications-------
Hospitals
Clinics
Diagnostic Centers
Medical Laboratories
Pharmacies
Research Laboratories
Biomedical Waste Storage Areas
✅ Advantages------
Reduces manual handling of biomedical waste.
Improves workplace safety and hygiene.
Provides automated waste segregation.
Reduces human sorting errors.
Uses a relatively low-cost system.
Provides faster and more organized waste handling.
⚠️ Limitations------
UV sterilization mainly affects exposed surfaces.
Some microorganisms may survive the sterilization process.
Sensors may have difficulty identifying mixed or unusual waste.
The system depends on electrical power.
The current design supports a limited number of waste categories.
🔮 Future Scope-------
The system can be further improved by adding:
IoT-based monitoring.
Real-time waste-level monitoring.
Cloud/database storage.
Camera-based waste classification.
AI/ML-based waste identification.
Automatic alerts for full waste bins.
More waste categories.
Mobile application monitoring.
👨‍💻 Project Type-------
Academic / Final Year Project
Domain: Embedded Systems | Automation | Biomedical Waste Management
📄 Project Description-------
This project integrates Arduino-based automation, sensors, conveyor technology, servo control, and UV sterilization to develop a safer and more efficient biomedical waste management system.

code------

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
