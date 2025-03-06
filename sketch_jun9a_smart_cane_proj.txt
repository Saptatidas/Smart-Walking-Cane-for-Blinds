const int trigPin = 9;
const int echoPin = 10;
const int buzzer =11;
const int ledPin =13;

long duration;
int distance;
int safetyDistance=50;

void setup() {
  
  // put your setup code here, to run once:
  pinMode (trigPin, OUTPUT);
  pinMode (echoPin, INPUT);
  pinMode (buzzer, OUTPUT);
  pinMode (ledPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  // put your main code here, to run repeatedly:
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);

  distance = duration*0.034/2;

  if (distance <= safetyDistance) {
    int blinkDelay;
    if (distance < 5) {
      blinkDelay = 10;
      digitalWrite(buzzer, HIGH);
      digitalWrite(ledPin, HIGH);
      delay(blinkDelay);
      digitalWrite(buzzer, LOW);
      digitalWrite(ledPin, LOW);
      delay(blinkDelay);
    } 
    else if (distance <= safetyDistance / 2) {
      blinkDelay = map(distance, 5, safetyDistance / 2, 10, 500);
      digitalWrite(buzzer, HIGH);
      digitalWrite(ledPin, HIGH);
      delay(blinkDelay);
      digitalWrite(buzzer, LOW);
      digitalWrite(ledPin, LOW);
      delay(blinkDelay);
    } 
    else {
    blinkDelay = map(distance, safetyDistance / 2, safetyDistance, 300, 1000);
    

    //int  blinkDelay =map(distance, 0 ,safetyDistance, 15, 1000);
    digitalWrite(buzzer, HIGH);
    digitalWrite(ledPin, HIGH);
    delay(blinkDelay);
    //digitalWrite(buzzer, LOW);
    //digitalWrite(ledPin, LOW);
    //delay(blinkDelay);
    }
  }
  else{
    digitalWrite(buzzer, LOW);
    digitalWrite(ledPin, LOW);
  }

  Serial.print("Distance: ");
  Serial.println(distance);
  delay(100);

}
