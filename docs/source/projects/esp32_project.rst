.. note::

    Hello, welcome to the SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Community on Facebook! Dive deeper into Raspberry Pi, Arduino, and ESP32 with fellow enthusiasts.

    **Why Join?**

    - **Expert Support**: Solve post-sale issues and technical challenges with help from our community and team.
    - **Learn & Share**: Exchange tips and tutorials to enhance your skills.
    - **Exclusive Previews**: Get early access to new product announcements and sneak peeks.
    - **Special Discounts**: Enjoy exclusive discounts on our newest products.
    - **Festive Promotions and Giveaways**: Take part in giveaways and holiday promotions.

    👉 Ready to explore and create with us? Click [|link_sf_facebook|] and join today!

Flowing Light with ESP32 
====================================================================

This project utilizes the Raspberry Pi Pico W along with various modules to create a real-time distance measuring and alert system. By integrating an ultrasonic sensor, an I2C-connected LCD1602 display, and an active buzzer, the system continuously monitors and displays the current distance to objects and adjusts the buzzer's alarm frequency and duration based on the proximity of the object.


**Required Components**

In this project, we need the following components.

* ESP32 WROOM 32E
* Obstacle Avoidance Module
* WS2812 RGB 8 LEDs Strip
* |link_breadvolt|
* 400 Holes Breadboard
* Jumper Wires

**Wiring Diagram**

.. image:: img/esp32_flow_light.png
    :width: 600
    :align: center

**Code**

.. note::

    The ``Adafruit NeoPixel`` library is used here, you can install it from the Library Manager.

.. code-block:: Arduino

    #include <Adafruit_NeoPixel.h>

    // Set the number of pixels for the running light
    #define NUM_PIXELS 8

    // Set the data pin for the RGB LED strip
    #define DATA_PIN 14

    // Initialize the RGB LED strip object
    Adafruit_NeoPixel pixels(NUM_PIXELS, DATA_PIN, NEO_GRB + NEO_KHZ800);

    // Initialize the avoid sensor
    #define AVOID_PIN 25

    void setup() {
        // Initialize the RGB LED strip
        pixels.begin();
        
        // Initialize the avoid sensor
        pinMode(AVOID_PIN, INPUT_PULLUP);
        
        // Set the initial LED color
        uint32_t color = pixels.Color(random(256), random(256), random(256));
        pixels.fill(color);
        pixels.show();
    }

    void loop() {
        // Read the input from the infrared sensor
        bool avoid_value = digitalRead(AVOID_PIN);

        // Generate a random color for the current pixel
        uint32_t color = pixels.Color(random(256), random(256), random(256));

        // If no obstacle is detected
        if (avoid_value) {
        for (int i = 0; i < NUM_PIXELS; i++) {
            // Turn on the current pixel with the random color
            pixels.setPixelColor(i, color);

            // Update the RGB LED strip display
            pixels.show();

            // Turn off the current pixel
            pixels.setPixelColor(i, 0);
            delay(100);
        }
        }
        // If detects an obstacle, change the direction of the LED strip
        else {
            for (int i = NUM_PIXELS - 1; i >= 0; i--) {
                pixels.setPixelColor(i, color);
                pixels.show();
                pixels.setPixelColor(i, 0);
                delay(100);
            }
        }
    }

This project is to display random colors on the LED strip. Additionally, an obstacle avoidance module has been included to dynamically change the direction of the running light.