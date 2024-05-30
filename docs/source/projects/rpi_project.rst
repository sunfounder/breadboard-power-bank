.. note::

    Hallo und willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Tauchen Sie tiefer ein in die Welt von Raspberry Pi, Arduino und ESP32 mit anderen Enthusiasten.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Anleitungen aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Spezialrabatte**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Sind Sie bereit, mit uns zu erkunden und zu erschaffen? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

Automatisches Eingangssystem mit Raspberry Pi
====================================================================

In diesem Projekt verwenden wir einen PIR-Sensor, um die Bewegung von Fußgängern zu erkennen und mit Servos, LEDs und einem Summer die Funktion einer automatischen Sensortür eines Convenience-Shops zu simulieren. Wenn ein Fußgänger im Erfassungsbereich des PIR-Sensors erscheint, wird das Anzeigelicht aktiviert, die Tür öffnet sich, und der Summer spielt den Öffnungsgong.

.. image:: img/rpi_project.jpg
    :width: 600
    :align: center

**Benötigte Komponenten**

Für dieses Projekt benötigen wir folgende Komponenten:

* Raspberry Pi
* GPIO-Erweiterungsplatine
* Widerstand
* LED
* PIR-Bewegungssensormodul
* Servo
* Summer
* Transistor
* |link_breadvolt|
* 800 Löcher Breadboard
* Jumperkabel

**Schaltplan**

.. image:: img/rpi_welcome.png
    :width: 600
    :align: center

**Code**

.. code-block:: Python

    #!/usr/bin/env python3

    from gpiozero import LED, MotionSensor, Servo, TonalBuzzer
    import time

    # GPIO pin setup for LED, motion sensor (PIR), and buzzer
    ledPin = LED(6)
    pirPin = MotionSensor(21)
    buzPin = TonalBuzzer(27)

    # Servo motor pulse width correction factor and calculation
    myCorrection = 0.45
    maxPW = (2.0 + myCorrection) / 1000  # Maximum pulse width
    minPW = (1.0 - myCorrection) / 1000  # Minimum pulse width

    # Initialize servo with custom pulse widths
    servoPin = Servo(25, min_pulse_width=minPW, max_pulse_width=maxPW)

    # Musical tune for buzzer, with notes and durations
    tune = [('C#4', 0.2), ('D4', 0.2), (None, 0.2),
            ('Eb4', 0.2), ('E4', 0.2), (None, 0.6),
            ('F#4', 0.2), ('G4', 0.2), (None, 0.6),
            ('Eb4', 0.2), ('E4', 0.2), (None, 0.2),
            ('F#4', 0.2), ('G4', 0.2), (None, 0.2),
            ('C4', 0.2), ('B4', 0.2), (None, 0.2),
            ('F#4', 0.2), ('G4', 0.2), (None, 0.2),
            ('B4', 0.2), ('Bb4', 0.5), (None, 0.6),
            ('A4', 0.2), ('G4', 0.2), ('E4', 0.2),
            ('D4', 0.2), ('E4', 0.2)]

    def setAngle(angle):
        """
        Move the servo to a specified angle.
        :param angle: Angle in degrees (0-180).
        """
        value = float(angle / 180)  # Convert angle to servo value
        servoPin.value = value      # Set servo position
        time.sleep(0.001)           # Short delay for servo movement

    def doorbell():
        """
        Play a musical tune using the buzzer.
        """
        for note, duration in tune:
            buzPin.play(note)       # Play the note
            time.sleep(float(duration))  # Duration of the note
        buzPin.stop()               # Stop buzzer after playing the tune

    def closedoor():
        # Turn off LED and move servo to close door
        ledPin.off()
        for i in range(180, -1, -1):
            setAngle(i)             # Move servo from 180 to 0 degrees
            time.sleep(0.001)       # Short delay for smooth movement
        time.sleep(1)               # Wait after closing door

    def opendoor():
        # Turn on LED, open door (move servo), play tune, close door
        ledPin.on()
        for i in range(0, 181):
            setAngle(i)             # Move servo from 0 to 180 degrees
            time.sleep(0.001)       # Short delay for smooth movement
        time.sleep(1)               # Wait before playing the tune
        doorbell()                  # Play the doorbell tune
        closedoor()                 # Close the door after the tune

    def loop():
        # Main loop to check for motion and operate door
        while True:
            if pirPin.motion_detected:
                opendoor()               # Open door if motion detected
            time.sleep(0.1)              # Short delay in loop

    try:
        loop()
    except KeyboardInterrupt:
        # Clean up GPIO on user interrupt (e.g., Ctrl+C)
        buzPin.stop()
        ledPin.off()

Nachdem der Code ausgeführt wurde, wird die Tür automatisch öffnen (durch den Servo simuliert), das Anzeigelicht einschalten und die Türklingelmusik spielen, wenn der PIR-Sensor jemanden vorbeigehen erkennt. Nachdem die Türklingelmusik gespielt hat, schließt das System automatisch die Tür und schaltet das Anzeigelicht aus, um auf das nächste Vorbeigehen zu warten.
