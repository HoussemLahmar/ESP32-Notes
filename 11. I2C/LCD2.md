# ESP32-S3 I2C LCD Display

https://wokwi.com/projects/345480558807089748

## Overview

This project demonstrates how to connect and control a 16x2 LCD display with an ESP32-S3 microcontroller using the I2C interface. The setup displays a simple "Hello, world!" message on the first line and "ESP32-S3 I2C" on the second line of the LCD.

## Hardware Requirements

- ESP32-S3 DevKitC-1 development board
- 16x2 LCD display with I2C adapter (PCF8574 I2C adapter)
- Jumper wires for connections

## Connections

| LCD Display (I2C) | ESP32-S3 |
|-------------------|----------|
| VCC               | 5V       |
| GND               | GND      |
| SDA               | GPIO 10  |
| SCL               | GPIO 8   |

## Software Dependencies

- Arduino IDE
- `LiquidCrystal_I2C` library

## Installation

1. Install the Arduino IDE from [arduino.cc](https://www.arduino.cc/en/software)
2. Install the ESP32 board support package in Arduino IDE
3. Install the `LiquidCrystal_I2C` library through the Arduino Library Manager
4. Select the appropriate ESP32-S3 board in the Arduino IDE

## Code Explanation

```cpp
#include <LiquidCrystal_I2C.h>

// Initialize the LCD with I2C address 0x27, 16 columns and 2 rows
LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {
  // Initialize I2C with custom SDA (GPIO 10) and SCL (GPIO 8) pins
  Wire.begin(10, 8);
  
  // Initialize the LCD
  lcd.init();
  
  // Turn on the LCD backlight
  lcd.backlight();
  
  // Set cursor to position (1, 0) - second column, first row
  lcd.setCursor(1, 0);
  
  // Print "Hello, world!" on the first row
  lcd.print("Hello, world !");
  
  // Set cursor to position (2, 1) - third column, second row
  lcd.setCursor(2, 1);
  
  // Print "ESP32-S3 I2C" on the second row
  lcd.print("ESP32-S3 I2C");
}

void loop() {
  // Wait for 100 milliseconds
  delay(100);
}
```

## Key Functions

- `Wire.begin(10, 8)` - Initializes the I2C communication with custom SDA and SCL pins
- `lcd.init()` - Initializes the LCD
- `lcd.backlight()` - Turns on the LCD backlight
- `lcd.setCursor(column, row)` - Positions the cursor on the LCD
- `lcd.print(text)` - Displays text at the current cursor position

## Simulation

This project is simulated using Wokwi online simulator. You can view and interact with it at:
[https://wokwi.com/projects/375092701719122945](https://wokwi.com/projects/375092701719122945)

## Project Files

The project consists of two main files:
1. The Arduino sketch (`.ino` file) containing the code
2. The Wokwi configuration file (`.json`) defining the hardware setup

## Customization

To customize this project:

- Change the displayed text by modifying the `lcd.print()` statements
- Adjust the cursor positions using `lcd.setCursor(column, row)` 
- Try adding scrolling text or custom characters
- Connect additional components like sensors and display their readings

## Common Issues

- **LCD Not Displaying**: Verify the I2C address (default is 0x27, but some displays use 0x3F)
- **Garbled Display**: Check the I2C connections and ensure proper voltage levels
- **No Display**: Confirm the backlight is on and contrast is properly adjusted

## Further Development

- Add sensors (temperature, humidity, etc.) and display readings
- Implement menu systems for user interaction
- Create animations or custom characters
- Connect to WiFi and display dynamic data from the internet

## Resources

- [ESP32-S3 Documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/index.html)
- [LiquidCrystal_I2C Library Documentation](https://github.com/johnrickman/LiquidCrystal_I2C)
- [I2C Communication Basics](https://learn.sparkfun.com/tutorials/i2c/all)
