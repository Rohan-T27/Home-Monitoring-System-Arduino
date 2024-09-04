This code is designed for an Arduino-based system that combines multiple sensors and components to monitor temperature, detect motion, and display information on an LCD screen. Here's a breakdown of how the code works:

### Libraries Used
1. **DHT.h** - Library for the DHT11 temperature and humidity sensor.
2. **LiquidCrystal.h** - Library for controlling an LCD display.
3. **Wire.h** and **RTClib.h** - Libraries for interacting with the DS1307 Real-Time Clock (RTC).

### Pin Definitions
- **DHT Sensor:**
  - **DHTPIN (2):** Digital pin connected to the DHT11 sensor.
  - **DHTTYPE (DHT11):** Type of DHT sensor used.
- **PIR Sensor:**
  - **PIRPIN (3):** Digital pin connected to the PIR motion sensor.
- **LED:**
  - **LEDPIN (4):** Digital pin connected to an LED.
- **Fan Control:**
  - **FANPIN (5):** Digital pin controlling the fan via a transistor.

### Sensor and Module Initializations
- **DHT Sensor** (dht): Used to measure temperature.
- **LCD Display** (lcd): Configured with specific Arduino pins.
- **RTC Module** (rtc): Keeps track of the current time.

### Timer Variables
- **previousMillis and interval:** Used to manage the LED on duration. interval is set to 60,000 milliseconds (1 minute).

### setup() Function
1. **Serial Monitor:** Initialized for debugging.
2. **LCD Display:** Initialized and displays "Initializing..." message.
3. **DHT Sensor:** Started for temperature reading.
4. **PIR Sensor:** Configured as an input.
5. **LED and Fan Control Pins:** Configured as outputs, with the fan initially turned off.
6. **RTC Module:** 
   - Checks if the RTC initializes correctly. If it fails, an error is displayed on the LCD, and the program stops.
   - Optionally, sets the RTC to the compile time (this line is commented out after the initial setup).

### loop() Function
The main function repeatedly executes the following tasks:

1. **PIR Sensor Reading:**
   - Reads the state of the PIR motion sensor. If motion is detected (pirVal == HIGH), the LED is turned on for 1 minute and then turned off.

2. **Time Handling:**
   - Retrieves the current time from the RTC and adjusts it for Eastern Time (ET), which is 12 hours behind UTC.
   - Handles time wrap-around to ensure the time is always within 0-23 hours.

3. **LCD Display Update:**
   - Clears the LCD and displays the current time (hour and minute) in the first row.
   - Reads the temperature from the DHT sensor. If there's an error reading the temperature, it displays "Temp Error"; otherwise, it displays the temperature in Celsius on the second row of the LCD.

4. **Fan Control Based on Temperature:**
   - If the temperature is 30 degrees Celsius or higher, the fan is turned on.
   - If the temperature is below 30 degrees Celsius, the fan is turned off.

5. **Delay:**
   - A delay of 2 seconds is added at the end to allow for a pause between each iteration of the loop.

### Key Functionalities
- **Motion Detection:** Turns on the LED when motion is detected by the PIR sensor.
- **Time Display:** Shows the current time on the LCD after adjusting for the correct time zone.
- **Temperature Monitoring and Display:** Continuously reads and shows the temperature. If it reaches a certain threshold, the fan is activated.
  
### Purpose
This code is useful for a simple home automation system where you want to:
- Monitor room temperature.
- Display the time and temperature on an LCD.
- Control an LED based on motion detection.
- Automatically turn on a fan if it gets too hot.
