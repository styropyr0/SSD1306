# **SSD1306 OLED Display Library**

A lightweight and versatile library to control SSD1306-based OLED displays with Arduino. This library offers a wide range of features including custom fonts, progress bars, animated text, bitmap rendering, and more. It works seamlessly with microcontrollers like **Arduino**, **ESP32**, and **ESP8266** over **I2C** communication.

Additionally, the library includes a **Bitmap Generator** tool in Python, which helps convert images to bitmap arrays for easy display on the OLED screen.

Please note that SSD1306 has 128 independently controllable **Columns** along the width, and 8 independently controllable **Pages** along the height. The total pixel count along the height is divided into **Pages** of 8 pixels high. In scenarios such as printing texts, bitmaps, progress bars, you should keep this in mind.

## **Features**
- **Text Display**: Print static text and animated text (typewriter effect).
- **Custom Fonts**: Supports custom fonts and character sets.
- **Progress Bar**: Display progress bars with various styles.
- **Bitmap Rendering**: Draw bitmap images on the OLED display.
- **Brightness Control**: Adjust display brightness (0-100%).
- **I2C Communication**: Built on I2C communication for simple wiring.
- **Custom Preferences**: Customize OLED display setup with a set of options.
- **Operator Overloading**: Use simple operators to display text and bitmaps.
- **Power Modes**: Control the display power for optimized performance.
- **Super Brightness**: Turn super brightness on or off (may be unstable).
- **Display Inversion**: Invert or restore the display colors.
- **Geometric Shapes**: Draw rectangles, circles, and lines on the display.

## **Installation**

### 1. Installing the Library

To use the library, you need to download or clone this repository into your **Arduino libraries** folder.

```bash
git clone https://github.com/styropyr0/oled.h.git
```

Alternatively, you can manually download the ZIP file and add it to the **Arduino IDE** by navigating to **Sketch → Include Library → Add .ZIP Library**.

### 2. Installing Dependencies

The library uses the **Wire** library for I2C communication, which is pre-installed with the Arduino IDE. No additional libraries are required.

## **Hardware Requirements**
- **SSD1306-based OLED display** (typically 128x64 or 128x32 pixels).
- **Arduino Board** (e.g., Arduino UNO, ESP32, or ESP8266).

## **Pin Configuration**
By default, the library uses I2C communication. The I2C pins are:
- **SDA (Data)**: Typically pin `A4` on most Arduino boards (varies by board).
- **SCL (Clock)**: Typically pin `A5` on most Arduino boards (varies by board).

For ESP32 and other microcontrollers, you can configure these pins in the `Wire.begin(SDA_PIN, SCL_PIN);` function.

## **Library Usage**

### 1. **Initializing the OLED Display**

To use the library, instantiate the `OLED` class with the display’s width and height (e.g., 128x64 or 128x32).

```cpp
#include "SSD1306.h"

// Create OLED object with width and height (128x64)
OLED oled(128, 64);

void setup() {
  oled.clearScr();  // Clear the screen
  oled.print("Hello, World!", 0, 0);  // Print text at (0, 0)
  oled << "This method also prints!" << 0 << 3;   // Print text at (0, 3)
}

void loop() {
  // Main loop logic
}
```

### 2. **Custom Fonts**

You can use custom fonts by defining an array of font data. This library uses 5x7 bitmaps for characters by default, but you can change the font with the `setFont()` method.

#### Example: Setting a Custom Font

```cpp
// Define a simple custom font (5x7 pixels)
const uint8_t myFont[5][5] = {
  {0x1F, 0x1F, 0x00, 0x00, 0x00},  // Example character data
  // More characters...
};

OLED oled(128, 64);

void setup() {
  oled.setFont(myFont);  // Set the custom font
  oled.print("Custom Font", 0, 0);  // Display with custom font
}
```

### 3. **Printing Static Text**

You can print static text at a given `(x, y)` coordinate using the `print()` method.

#### Example: Printing Text

```cpp
oled.print("Hello, OLED!", 0, 0);  // Prints "Hello, OLED!" at (0, 0)
```

### 4. **Animated Text (Typewriter Effect)**

Use the `printAnimated()` method for a typewriter effect, where text is displayed one character at a time.

#### Example: Animated Text

```cpp
oled.printAnimated("Welcome!", 0, 0, 100);  // Print text with a 100 ms delay between characters
```

### 5. **Progress Bars**

You can display progress bars in multiple styles (1-10 for progress bars, 11-15 for loaders). The `progressBar()` method accepts the progress value (0-100) and a style number.

#### Example: Progress Bar

```cpp
oled.progressBar(50, 0, 10, 1);  // Displays a 50% progress bar at (0, 10), style 1
```

### 6. **Bitmap Rendering**

The library includes the `draw()` method to display bitmaps on the OLED. You can convert images into bitmap arrays using the **Bitmap Generator** Python tool (explained below).

#### Example: Displaying a Bitmap

```cpp
const uint8_t myBitmap[] = {
  // Your bitmap data here, generated by the Bitmap Generator
};

oled.draw(myBitmap, 0, 0, 16, 16);  // Draw a 16x16 bitmap at coordinates (0, 0)
```

### 7. **Chaining Operators for Display**

The library also includes operator overloading to simplify the process of displaying text and bitmaps. You can use the `<<` operator to print text and the `[]` operator to display bitmaps.

#### Example: Printing Text Using Chaining Operators

```cpp
oled << "Hello, World!" << 0 << 0;  // Prints "Hello, World!" at (0, 0)
```

#### Example: Displaying a Bitmap Using Chaining Operators

```cpp
oled[myBitmap] << 0 << 0 << 16 << 16;  // Draws a 16x16 bitmap at (0, 0)
```

### 8. **Clearing the Screen**

Use the `clearScr()` method to clear the screen.

```cpp
oled.clearScr();  // Clears the display
```

### 9. **Adjusting Brightness**

Use the `setBrightness()` method to adjust the display’s brightness. It accepts a percentage value (0-100).

```cpp
oled.setBrightness(80);  // Set the brightness to 80%
```

### 10. **Custom OLED Setup**

The `manualSetup()` method allows you to pass an array of settings to configure the OLED display manually.

```cpp
uint8_t customSettings[] = {
  0xA8, 0x3F,  // Multiplex ratio
  0xD3, 0x00,  // Display offset
  // More settings...
};

oled.manualSetup(customSettings);  // Apply custom settings
```

### 11. **Turn Display Off on Clear**

You can disable the display when you clear the screen using the `turnOffOnClr()` method.

```cpp
oled.turnOffOnClr(true);  // Turn off display when cleared
```

### 12. **Power Modes**

The display can operate in different power modes. Choose between low power, balanced, or performance mode to optimize energy consumption or display quality.

#### Example: Setting Power Mode

```cpp
oled.setPowerMode(LOW_POWER_MODE);  // Set the display to low power mode
```

### 13. **Super Brightness**

Enable or disable super brightness for high-intensity display, though note it may cause instability.

#### Example: Super Brightness

```cpp
oled.superBrightness(true);  // Turn on super brightness
```

### 14. **Inverting the Display**

Invert the pixels on the display to change all white pixels to black and vice versa.

#### Example: Inverting Display

```cpp
oled.invertDisplay();  // Invert the display colors
```

### 15. **Entire Display ON/OFF**

You can turn the entire display on or off.

#### Example: Entire Display ON/OFF

```cpp
oled.entireDisplayON();  // Turn all pixels on
oled.entireDisplayOFF();  // Revert back to the content
```

### 16. **Drawing Geometric Shapes**

You can draw shapes such as rectangles, circles, and lines on the OLED display.

#### Example: Drawing a Rectangle

```cpp
oled.rectangle(10, 10, 50, 30, 5);  // Draw a rectangle at (10, 10) with width 50, height 30, and 5px corner radius
```

#### Example: Drawing a Circle

```cpp
oled.circle(64, 32, 20);  // Draw a circle at (64, 32) with radius 20
```

#### Example: Drawing a Line

```cpp
oled.line(0, 0, 127, 63);  // Draw a line from (0, 

0) to (127, 63)
```

---

## **Constants**

### 1. **Display Settings Constants**:

- **OLED_OFF**: `0xAE` – Turns the display off.
- **OLED_ON**: `0xAF` – Turns the display on.
- **DISP_CLOCK_DIV_RATIO**: `0xD5` – Set the display clock division ratio.
- **MULTIPLEX**: `0xA8` – Set the multiplex ratio for the display.
- **CHRG_PUMP**: `0x8D` – Charge pump command for OLEDs that require it.
- **DISP_OFFSET**: `0xD3` – Display offset setting.
- **MEM_ADDRESS_MODE**: `0x20` – Memory addressing mode.
- **COM_CONFIG**: `0xDA` – Common pins configuration.
- **CONTRAST**: `0x81` – Set the contrast level.
- **PRE_CHRG**: `0xD9` – Pre-charge period configuration.
- **VCOMH_DESEL**: `0xDB` – VCOMH deselect level.

---

## **Bitmap Generator Tool**

### Overview

The **Bitmap Generator** tool helps you convert images to bitmaps that can be displayed on your SSD1306 OLED screen. This tool is written in **Python** and uses the **Pillow** library to process images.

### Running the Bitmap Generator

#### Command-Line Usage

You can run the **Bitmap Generator** directly from the command line:

```bash
python image_to_bitmap.py <image_path> <output_file>
```

For example, to convert an image located at `images/logo.png` to a bitmap:

```bash
python image_to_bitmap.py images/logo.png output_logo.h
```

#### Python Script Usage

You can also run the script interactively:

```bash
python image_to_bitmap.py
```

This will prompt you for the image path and the output file name:

```
Enter the path to the image: images/logo.png
Enter the output file name: output_logo.h
```

The script will generate the bitmap data and save it in the output file.

#### Script Details

- **Image Format**: The script expects the image to be in `.png`, `.jpg`, or other common formats supported by **Pillow**.
- **Threshold**: The `threshold` parameter (default `50`) controls how dark the pixels should be to be considered “on” for the OLED. You can adjust this value to get the desired result.

### Example Bitmap Output

The Python script will generate output in the following format:

```cpp
const uint8_t PROGMEM splash[] = {
  0b11111111, 0b00000000, 0b11111111, 0b00000000,  // Row 1
  0b00000000, 0b11111111, 0b00000000, 0b11111111,  // Row 2
  // More rows...
};
```

This array can then be used directly in your Arduino code to display the image.

## **License**

This library is licensed under the **MIT License**.

