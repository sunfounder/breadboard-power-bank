.. note::

    Hallo und willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Tauchen Sie tiefer ein in die Welt von Raspberry Pi, Arduino und ESP32 mit anderen Enthusiasten.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Anleitungen aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Spezialrabatte**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Sind Sie bereit, mit uns zu erkunden und zu erschaffen? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!


Fließendes Licht mit ESP32
====================================================================

In diesem Projekt werden wir einen fließenden Lichteffekt mit dem ESP32 WROOM 32E zusammen mit einem Hindernisvermeidungsmodul und einem WS2812 RGB LED-Streifen erstellen. Die Konfiguration umfasst 8 LEDs, die zufällig ihre Farben ändern und auf die Hinderniserkennung reagieren, indem sie die Fließrichtung der Lichter umkehren. Die Hinderniserkennung wird von einem Infrarotsensor übernommen, der mit dem ESP32 verbunden ist, und der LED-Streifen wird mit der Adafruit NeoPixel-Bibliothek gesteuert.

**Benötigte Komponenten**

Für dieses Projekt benötigen wir folgende Komponenten:

* ESP32 WROOM 32E
* Hindernisvermeidungsmodul
* WS2812 RGB 8 LEDs Streifen
* |link_breadvolt|
* 400 Löcher Breadboard
* Jumperkabel

**Schaltplan**

.. image:: img/esp32_flow_light.png
    :width: 600
    :align: center

**Code**

.. note::

    Hier wird die ``Adafruit NeoPixel``-Bibliothek verwendet, die Sie über den Bibliotheks-Manager installieren können.


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
