.. note::

    Hello, welcome to the SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Community on Facebook! Dive deeper into Raspberry Pi, Arduino, and ESP32 with fellow enthusiasts.

    **Why Join?**

    - **Expert Support**: Solve post-sale issues and technical challenges with help from our community and team.
    - **Learn & Share**: Exchange tips and tutorials to enhance your skills.
    - **Exclusive Previews**: Get early access to new product announcements and sneak peeks.
    - **Special Discounts**: Enjoy exclusive discounts on our newest products.
    - **Festive Promotions and Giveaways**: Take part in giveaways and holiday promotions.

    👉 Ready to explore and create with us? Click [|link_sf_facebook|] and join today!

Automatic Entrance System with Raspberry Pi
====================================================================

In this project, we will use PIR to sense the movement of pedestrians, and use servos, LED, buzzer to simulate the work of the sensor door of the convenience store. When the pedestrian appears within the sensing range of the PIR, the indicator light will be on, the door will be opened, and the buzzer will play the opening bell.

.. image:: img/rpi_project.jpg
    :width: 600
    :align: center

**Required Components**

In this project, we need the following components.

* Raspberry Pi
* GPIO Extension Board
* Resistor
* LED
* PIR Motion Sensor Module
* Servo
* Buzzer
* Transistor
* |link_breadvolt|
* 800 Holes Breadboard
* Jumper Wires

**Wiring Diagram**

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

After the code runs, if the PIR sensor detects someone passing by, the door will automatically open (simulated by the servo), turn on the indicator and play the doorbell music. After the doorbell music plays, the system will automatically close the door and turn off the indicator light, waiting for the next time someone passes by.