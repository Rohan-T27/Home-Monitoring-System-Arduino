#include <DHT.h>
#include <LiquidCrystal.h>
#include <Wire.h>
#include <RTClib.h>

// Define DHT sensor type and pin
#define DHTPIN 2          // Digital pin connected to the DHT sensor
#define DHTTYPE DHT11     // DHT 11

// Define PIR sensor pin
#define PIRPIN 3          // Digital pin connected to the PIR sensor

// Define LED pin
#define LEDPIN 4          // Digital pin connected to the LED

// Define fan control pin
#define FANPIN 5          // Digital pin connected to the fan control (through a transistor)

// Initialize DHT sensor
DHT dht(DHTPIN, DHTTYPE);

// Initialize the library for LCD with the numbers of the interface pins
LiquidCrystal lcd(7, 8, 9, 10, 11, 12);

// Initialize the RTC
RTC_DS1307 rtc;

// Timer variables
unsigned long previousMillis = 0;     // Stores the last time LED was turned on
const long interval = 60000;          // LED on duration (1 minute)
unsigned long previousTempMillis = 0; // Stores the last time temperature was read
const long tempInterval = 2000;       // Temperature update interval (2 seconds)

void setup() {
  // Initialize Serial for debugging
  Serial.begin(9600);
  
  // Initialize LCD
  lcd.begin(16, 2);   
  lcd.print("Initializing...");  // Display initial message

  // Initialize DHT sensor
  dht.begin();

  // Initialize PIR sensor
  pinMode(PIRPIN, INPUT);  // Set PIR pin as input

  // Initialize LED
  pinMode(LEDPIN, OUTPUT);  // Set LED pin as output

  // Initialize fan control pin
  pinMode(FANPIN, OUTPUT);  // Set fan control pin as output
  digitalWrite(FANPIN, LOW); // Ensure fan is off initially

  // Initialize RTC
  if (!rtc.begin()) {
    lcd.clear();
    lcd.print("RTC Error");
    Serial.println("RTC Error"); // Debug message
    while (1);  // Stop if RTC initialization fails
  }

  // Set the time (This should only be done once, comment out after setting)
  // rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));  // Sets RTC to the time the sketch was compiled

  delay(2000);  // Delay to allow the system to stabilize
  lcd.clear();  // Clear the initial message
}

void loop() {
  unsigned long currentMillis = millis();  // Get the current time

  // Read PIR sensor state
  int pirVal = digitalRead(PIRPIN);

  // If motion is detected
  if (pirVal == HIGH) {
    digitalWrite(LEDPIN, HIGH);  // Turn on LED
    previousMillis = currentMillis;  // Reset timer for LED
  }

  // Turn off LED after the interval
  if (currentMillis - previousMillis >= interval) {
    digitalWrite(LEDPIN, LOW);   // Turn off LED
  }

  // Get current time from RTC
  DateTime now = rtc.now();

  // Adjust for Eastern Time (ET) offset from UTC
  int hourET = now.hour() - 12;  

  // Handle time wrap around (if time becomes negative after adjustment)
  if (hourET < 0) {
    hourET += 24;
  }

  // Display Eastern Time and temperature
  lcd.setCursor(0, 0);  // Set cursor to first column, first row

  lcd.print("Time: ");
  if (hourET < 10) lcd.print('0'); // Add leading zero if hourET is less than 10
  lcd.print(hourET);
  lcd.print(':');
  if (now.minute() < 10) lcd.print('0'); // Add leading zero if minutes are less than 10
  lcd.print(now.minute());

  // Check if it's time to update the temperature
  if (currentMillis - previousTempMillis >= tempInterval) {
    previousTempMillis = currentMillis;  // Update the time of the last temperature read

    // Read and display temperature
    float temperature = dht.readTemperature();
    lcd.setCursor(0, 1);  // Set cursor to first column, second row
    if (isnan(temperature)) {
      lcd.print("Temp Error     ");
    } else {
      lcd.print("Temp: ");
      lcd.print(temperature);
      lcd.print(" ");
      lcd.print((char)223);  // Degree symbol
      lcd.print("C  ");  // Clear any extra characters
    }

    // Control fan based on temperature or button state
    if (temperature >= 30) {
      digitalWrite(FANPIN, HIGH);  // Turn on the fan
    } else {
      digitalWrite(FANPIN, LOW);  // Turn off the fan
    }
  }

}
