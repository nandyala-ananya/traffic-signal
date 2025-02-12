# traffic-signal
# Objective: Simulate a simple traffic light using an
Arduino Uno, three LEDs (Red, Yellow, Green), and a push button for pedestrian
crossing.

# Components Needed:

Arduino Uno

Red, Yellow, and Green LEDs

3 × 220Ω resistors

Push button (for pedestrian crossing)

10kΩ resistor

Breadboard & wires

# Working:

The LEDs should cycle like a real traffic light: Green → Yellow → Red → Green.

If the push button is pressed, the system should immediately turn Red for
pedestrian crossing, then return to normal cycle.
# code
const int greenLed = 8;
const int yellowLed = 9;
const int redLed = 10;
const int buttonPin = 2;

volatile bool pedestrianRequest = false; 
unsigned long previousMillis = 0;
unsigned long interval = 0;
int state = 0; 

void setup() {
  pinMode(8, OUTPUT);
  pinMode(9, OUTPUT);
  pinMode(10, OUTPUT);
  pinMode(2, INPUT_PULLUP); 
  attachInterrupt(digitalPinToInterrupt(2), handleButtonPress, FALLING);
 
  digitalWrite(8, HIGH);
  digitalWrite(9, LOW);
  digitalWrite(10, LOW);
  state = 0; 
  interval = 5000; 
}

void loop() {
  unsigned long currentMillis = millis();

  if (pedestrianRequest) {
    digitalWrite(8, LOW);
    digitalWrite(9, LOW);
    digitalWrite(10, HIGH);

    delay(5000); 

    pedestrianRequest = false; 
    state = 0; 
    previousMillis = millis(); 
    digitalWrite(10, LOW);
    digitalWrite(8, HIGH);
    interval = 5000;
    return;
  }

  
  if (currentMillis - previousMillis >= interval) {
    previousMillis = currentMillis;

    if (state == 0) { 
      digitalWrite(8, LOW);
      digitalWrite(9, HIGH);
      interval = 2000; 
      state = 1;
    } else if (state == 1) { 
      digitalWrite(9, LOW);
      digitalWrite(10, HIGH);
      interval = 5000; 
      state = 2;
    } else if (state == 2) { 
      digitalWrite(10, LOW);
      digitalWrite(8, HIGH);
      interval = 5000; 
      state = 0;
    }
  }
}

void handleButtonPress() {
  pedestrianRequest = true;
}
