# Radar-System
Radar system using Arduino, Ultrasonic sensor and Servo motor

#include <Servo.h>

Servo servoMotor;

const int trigPin = 10;
const int echoPin = 11;

void setup() {
  Serial.begin(9600);
  servoMotor.attach(12);
}

void loop() {
  for (int angle = 0; angle <= 180; angle += 10) {
    servoMotor.write(angle);
    delay(500);
    float distance = getDistance();
    if (distance != -1) {
      float xPos = distance * cos(angle * PI / 180.0);
      float yPos = distance * sin(angle * PI / 180.0);
      Serial.print("Object detected at (");
      Serial.print(xPos);
      Serial.print(", ");
      Serial.print(yPos);
      Serial.println(")");
    }
  }
}

float getDistance() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  float duration = pulseIn(echoPin, HIGH);
  float distance = duration * 0.034 / 2;
  
  if (distance >= 400 || distance <= 0) {
    Serial.println("No object detected");
    return -1;
  } else {
    Serial.print("Distance: ");
    Serial.println(distance);
    return distance;
  }
}
