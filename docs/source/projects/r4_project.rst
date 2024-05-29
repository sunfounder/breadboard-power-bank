.. note::

    こんにちは、SunFounderのRaspberry Pi & Arduino & ESP32愛好家コミュニティへようこそ！Facebook上でRaspberry Pi、Arduino、ESP32についてもっと深く掘り下げ、他の愛好家と交流しましょう。

    **参加する理由は？**

    - **エキスパートサポート**：コミュニティやチームの助けを借りて、販売後の問題や技術的な課題を解決します。
    - **学び＆共有**：ヒントやチュートリアルを交換してスキルを向上させましょう。
    - **独占的なプレビュー**：新製品の発表や先行プレビューに早期アクセスしましょう。
    - **特別割引**：最新製品の独占割引をお楽しみください。
    - **祭りのプロモーションとギフト**：ギフトや祝日のプロモーションに参加しましょう。

    👉 私たちと一緒に探索し、創造する準備はできていますか？[|link_sf_facebook|]をクリックして今すぐ参加しましょう！

環境モニタリングシステム（Arduino使用）
====================================================================

このプロジェクトでは、Arduino Uno R4 Minima、DHT11温湿度センサー、フォトレジスター、およびI2C LCD1602ディスプレイを使用して、シンプルで効果的なモニタリングシステムを作成する方法を示します。このシステムは、温度と光の強さを読み取り、LCD画面にこれらの値を表示するため、組み込みシステムにおけるセンサー統合とI2C通信について学ぶのに実用的なプロジェクトです。

.. image:: img/r4_project.jpg
    :width: 600
    :align: center

**必要なコンポーネント**

このプロジェクトには、以下のコンポーネントが必要です。

* Arduino Uno R4 Minima
* I2C LCD1602
* DHT11 温湿度センサー
* フォトレジスター
* 10k レジスター
* |link_breadvolt|
* 400ホールズ・ブレッドボード
* ジャンパーワイヤー

**配線図**

**コード**

.. note::

    ここでは ``LiquidCrystal I2C`` と ``DHT sensor library`` を使用しています。これらは **ライブラリマネージャー** からインストールできます。

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




