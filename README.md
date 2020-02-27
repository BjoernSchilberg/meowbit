# Meowbit

- [Meowbit](#meowbit)
  - [Spezifikation](#spezifikation)
  - [Pinbelegung](#pinbelegung)
  - [Entwicklungsumgebungen](#entwicklungsumgebungen)
    - [Microsoft MakeCode Arcade](#microsoft-makecode-arcade)
    - [MicroPython](#micropython)
    - [CircuitPython](#circuitpython)

Infos und Beispiele zum Meowbit von <https://kittenbot.cn>.

Das
[Meowbit](https://www.kittenbot.cc/collections/frontpage/products/meowbit-codable-console-for-microsoft-makecode-arcade)
ist eine kleine Spielekonsole des chinesischen Herstellers Kittenbot. Du
kannst es [auf Amazon kaufen](https://amzn.to/2R8b7Ja). Auch Adafruit
[vertreibt
es](https://blog.adafruit.com/2019/08/01/new-product-kittenbot-meowbit-codable-console-for-makecode-arcade/)
seit August 2019. ![Meowbit, bbc:microbit, Calliope
Mini](images/00-meowbit_microbit_calliope.jpg)

## Spezifikation

Das Meowbit nutzt als Rechenkern einen `STM32F401RET6`, das ist ein
32-Bit-ARM-Cortex-M4-Prozessor
([Datenblatt](http://www.farnell.com/datasheets/1848998.pdf)). Darüber hinaus
ist eine beeindruckende Menge an Hardware eingebaut:

- LED für Lade- / Arbeitsanzeige
- Lichtsensor
- Schiebeschalter zum Ein-/Ausschalten
- zwei programmierbare LED
- Reset-Taste
- DFU-Modus-Taste (auch zum Aufrufen des Menüs durch die Makecode-Firmware)
- 160 x 128 TFT Farbbildschirm (ST7735)
- Temperaturfühler
- vier programmierbare Richtungstasten
- programmierbarer Summer
- zwei programmierbare Tasten A und B
- 40-Pin-Goldkontaktleiste, kompatibel zum micro:bit
- USB-Port zum Laden und Programmieren
- SD-Kartenslot (zum Speichern von Programmen und nachträglichen Erweitern um ein Bluetooth- oder WLAN-Modul)
- Klinkenbuchse zum Verbinden mehrerer Geräte (JacDac)
- 6-Achsen-Gyroskop und Beschleunigungsmesser (InvenSense MPU-6050)
- 3,7 V Lithium-Batterie-Schnittstelle

Standardmäßig sind 2 MByte des SPI-Flash-Speichers mit einer Unicode-Zeichentabelle
belegt.

## Pinbelegung

Hier ist die Pinbelegung der 40-poligen Steckerleiste (Stand: 2020-01-18.
[Original](https://meowbit-doc.kittenbot.cn/#/more/intro)):

![Pinbelegung](images/1Sbv9O.png)

## Entwicklungsumgebungen

Das Meowbit kann auf drei Arten programmiert werden:

### Microsoft MakeCode Arcade

<https://arcade.makecode.com/> von Microsoft. Das Meowbit ist eine der
offiziell unterstützten Spielekonsolen.

![Microsoft Arcade](images/00-arcade.png)

### MicroPython

Programmieren mit [MicroPython](micropython.md).

### CircuitPython

Programmieren mit [CircuitPython](circuitpython.md).