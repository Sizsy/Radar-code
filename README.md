#include <Servo.h>
#include <Ultrasonic.h>

Servo myservo;
Ultrasonic ultrasonic(9, 8);

int pos = 0;
int step = 2;
int distance;

const int buzzerPin = 4;
const int alertDistance = 15;

void setup() {
  myservo.attach(3);
  Serial.begin(9600);
  pinMode(buzzerPin, OUTPUT);
}

void loop() {

  // Move servo
  myservo.write(pos);

  // Read ultrasonic
  distance = ultrasonic.read();

  // Buzzer logic
  if (distance > 0 && distance <= alertDistance) {
    digitalWrite(buzzerPin, HIGH);
    Serial.print("Object detected: ");
    Serial.println(distance);
  } else {
    digitalWrite(buzzerPin, LOW);
  }

  Serial.print("Distance in CM: ");
  Serial.println(distance);

  delay(20); 


  pos += step;


  if (pos >= 180 || pos <= 0) {
    step = -step;
  }
}
