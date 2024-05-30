.. note::

    Hallo und willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Tauchen Sie tiefer ein in die Welt von Raspberry Pi, Arduino und ESP32 mit anderen Enthusiasten.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Anleitungen aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Spezialrabatte**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Sind Sie bereit, mit uns zu erkunden und zu erschaffen? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

Umweltüberwachungssystem mit Arduino
====================================================================

Dieses Projekt zeigt, wie man ein einfaches, aber effektives Überwachungssystem mit einem Arduino Uno R4 Minima, einem DHT11 Temperatur- und Feuchtigkeitssensor, einem Fotowiderstand und einem I2C LCD1602 Display erstellt. Das System liest die Temperatur und Lichtintensität und zeigt die Werte auf dem LCD-Bildschirm an, was es zu einem praktischen Projekt macht, um über Sensorintegration und I2C-Kommunikation in eingebetteten Systemen zu lernen.

.. image:: img/r4_project.jpg
    :width: 600
    :align: center

**Benötigte Komponenten**

Für dieses Projekt benötigen wir folgende Komponenten:

* Arduino Uno R4 Minima
* I2C LCD1602
* DHT11 Feuchtigkeits- und Temperatursensor
* Fotowiderstand
* 10k Widerstand
* |link_breadvolt|
* 400 Löcher Breadboard
* Jumperkabel

**Schaltplan**

.. image:: img/r4_circuit.png
    :width: 600
    :align: center

**Code**

.. note::

    Hier werden die Bibliotheken ``LiquidCrystal I2C`` und ``DHT sensor library`` verwendet, die Sie über den **Library Manager** installieren können.

.. code-block:: Arduino

    #include <Wire.h>
    #include <LiquidCrystal_I2C.h>
    #include <DHT.h>

    // Define the DHT11 sensor pin and type
    #define DHTPIN 4
    #define DHTTYPE DHT11

    // Create DHT object
    DHT dht(DHTPIN, DHTTYPE);

    // Create LCD object, set I2C address to 0x27, LCD size to 16x2
    LiquidCrystal_I2C lcd(0x27, 16, 2);

    void setup() {
        // Initialize serial communication
        Serial.begin(9600);
        Serial.println("DHT11 and Light Sensor Test!");

        // Initialize DHT sensor
        dht.begin();

        // Initialize LCD
        lcd.init();
        lcd.backlight(); // Turn on LCD backlight
    }

    void loop() {
        // Wait a few seconds between measurements
        delay(2000);

        // Reading temperature and humidity takes about 250 milliseconds
        // Sensor readings may also be up to 2 seconds 'old' (it's a very slow sensor)
        float humidity = dht.readHumidity();
        float temperature = dht.readTemperature(); // Read temperature as Celsius (default)

        // Read light level from the photoresistor
        int lightLevel = analogRead(A0);

        // Check if any reads failed and exit early (to try again)
        if (isnan(humidity) || isnan(temperature)) {
            Serial.println("Failed to read from DHT sensor!");
            return;
        }

        // Print the temperature and light level to the LCD
        lcd.clear();
        lcd.setCursor(0, 0); // Set cursor to column 0, line 0
        lcd.print("Temp: ");
        lcd.print(temperature);
        lcd.print(" C");
        
        lcd.setCursor(0, 1); // Set cursor to column 0, line 1
        lcd.print("Light: ");
        lcd.print(lightLevel);
    }




