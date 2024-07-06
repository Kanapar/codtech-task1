#include <LiquidCrystal.h>

LiquidCrystal lcd(12,11,5,4, 3,2); // LCD pins: RS, E, D4, D5, D6, D7

const int tempPin = A1;   // Analog pin for temperature sensor
const int pulsePin = A2;  // Analog pin for pulse sensor
const int buttonPin = 7;  // Digital pin for button

bool displayStandby = true;

void setup() {
  pinMode(buttonPin, INPUT);
  
  lcd.begin(16, 2);   // Initialize LCD: 16 columns, 2 rows
  Serial.begin(9600);
  
  lcd.print("WELCOME TO ");
  lcd.setCursor(0, 1);
  lcd.print("HEALTH MONITOR");
  delay(2000);
  lcd.clear();
  lcd.print("INITIALIZING");
  
  for (int i = 0; i < 4; i++) {
    lcd.print(".");
    delay(1000);
  } 
  
  lcd.clear();
  lcd.print("PRESS BUTTON");
  lcd.setCursor(0, 1);
  lcd.print("TO SCAN DATA");
}

void loop() {
  int buttonState = digitalRead(buttonPin);
  
  if (buttonState == HIGH && displayStandby) {
    displayStandby = false;
    lcd.clear();
    lcd.print("SCANNING...");
    delay(1000); // Optional delay for user feedback
    ScanData();
    displayStandby = true; // Reset display state after scanning
  }
}

void ScanData() {
  lcd.clear();
  lcd.print("PLEASE WAIT...");
  
  // Scan temperature
  float temperature = readTemperature();
  
  lcd.clear();
  lcd.print("TEMP: ");
  lcd.print(temperature);
  lcd.print(" C");
  
  delay(2000);
  lcd.clear();
  
  // Scan pulse rate
  int pulseRate = readPulseRate();
  
  lcd.print("BPM: ");
  lcd.print(pulseRate);
  lcd.print(" bpm");
  
  delay(2000);
  lcd.clear();
  
  lcd.print("SCAN SUCCESSFUL");
  delay(2000);
  
  lcd.clear();
  lcd.print("UPLOADING...");
  
  // Simulate uploading process (placeholder)
  delay(3000);
  
  lcd.clear();
  lcd.print("THANK YOU FOR");
  lcd.setCursor(0, 1);
  lcd.print("USING OUR DEVICE");
  delay(3000);
  lcd.clear();
}

float readTemperature() {
  int sensorValue = analogRead(tempPin);
  float voltage = sensorValue * (5.0 / 1023.0); // Convert analog reading to voltage
  float temperatureC = (voltage - 0.5) * 100.0; // Convert voltage to temperature in Celsius
  return temperatureC;
}

int readPulseRate() {
  int sensorValue = analogRead(pulsePin);
  int pulseRate = sensorValue / 6; // Adjust conversion based on sensor characteristics
  return pulseRate;
}