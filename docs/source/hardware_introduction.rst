.. note::

    Hallo und willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Tauchen Sie tiefer ein in die Welt von Raspberry Pi, Arduino und ESP32 mit anderen Enthusiasten.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Anleitungen aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Spezialrabatte**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Sind Sie bereit, mit uns zu erkunden und zu erschaffen? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

Hardware-Einführungen
==============================

**Eigenschaften**

* **Batterie**: 3,7V 14500 Li-Ionen-Batterie, 500mAh
* **Ausgang**: 5V/1,5A, 3,3V/1A (einstellbar über Jumper. 0V, 3,3V und 5V Konfiguration)
* **Ausgang**: USB (Typ-A) 5V
* **Ladeeingang**: USB Typ-C, 5V
* **Ladestrom**: 500mA
* **Abschaltstrom**: < 0.5mA
* **Schutzspannung bei Überentladung der Lithium-Batterie**: 2.4V
* **Schutzspannung bei Überladung der Lithium-Batterie**: 4.28V
* Integrierte Ladeanzeige (CHG)
* Integrierte Stromanzeige (PWR)
* Ein-/Ausschalter verfügbar
* **Abmessungen**: 52mm x 32mm x 24mm (L x B x H)

**Pinbelegung**

.. image:: img/power_bank_pinout.png
    :width: 500
    :align: center

**Einsetzen in das Steckbrett**

Das Modul wird direkt an einem Ende des Steckbretts installiert. Die vier 2-poligen Header auf der Rückseite des Moduls werden für die Stromversorgung des Steckbretts verwendet. Es wird empfohlen, die negative Seite in die entsprechenden Löcher der blauen/schwarzen Linie des Steckbretts einzustecken.

.. image:: img/plugin_breadboard2.png
    :width: 400
    :align: center

.. image:: img/plugin_breadboard.png
    :width: 400
    :align: center

**Netzschalter**

Schalten Sie den Schalter in die **ON**-Position, um den Bordschalter einzuschalten. Die grüne **PWR**-Leuchte leuchtet auf, und zu diesem Zeitpunkt gibt der USB-Typ-A-Anschluss 5V aus. Die Stromquellen der beiden Wege auf dem Steckbrett werden über Jumper-Kappen ausgewählt.

.. image:: img/power_switch.png
    :width: 500
    :align: center

**3V3/5V Pin Header-Ausgang**

Die J2- und J3-Header auf der Platine steuern das Umschalten und die Spannungsauswahl der Strompfade auf jeder Seite mittels Jumper-Kappen. Das Platzieren der Jumper-Kappe auf dem mittleren 2Pin (OFF) trennt den Ausgang, was daran zu erkennen ist, dass die LEDs auf beiden Wegen erlöschen. Das Verschieben der Jumper-Kappe auf die 3V3- oder 5V-Abschnitte steuert die Ausgabe von 3,3V oder 5V.

.. image:: img/select_power.png
    :width: 500
    :align: center

**Stromwege**

Der USB-Typ-C-Eingang wird teilweise zum Laden und teilweise zur direkten Ausgabe an USB-Typ-A, den 5V-Pin-Ausgang und den Eingang des 3,3V-Linearschaltreglers verwendet.

**Laden**

Wenn eine 5V-Stromquelle an den USB-Typ-C-Anschluss angeschlossen wird, kann sie die Batterie laden, und die rote **CHG**-Leuchte leuchtet auf, um das Laden anzuzeigen. Sie erlischt, wenn der Ladevorgang abgeschlossen ist.

.. image:: img/power_charge.png
    :width: 500
    :align: center

**Batterieschutz**

* **Überentladungsschutz**: Wenn die Batteriespannung unter 2,4V fällt, wird der Batterieschutz aktiviert, und die Batterie entlädt sich nicht mehr. Durch Anschließen des Ladegeräts und Laden über 3,0V wird der Überentladungsschutz deaktiviert.
* **Überladeschutz**: Wenn die Gesamts Batteriespannung 4,28V erreicht, stoppt der Ladevorgang. Das Absinken der Spannung auf 4,08V deaktiviert den Überladeschutz.
* **Überstromschutz**: Der Überstromschutz liegt bei etwa 3,75A.
* **Kurzschlussschutz**: Der Kurzschlussschutz liegt bei etwa 32A.
