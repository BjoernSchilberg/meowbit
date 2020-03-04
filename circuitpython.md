# CircuitPython

- [CircuitPython](#circuitpython)
  - [Installieren von benötigten Biblotheken (CircuitPython Library Bundle)](#installieren-von-ben%c3%b6tigten-biblotheken-circuitpython-library-bundle)
  - [Ein Beispiel-Programm](#ein-beispiel-programm)
  - [Anschließen von Seeed Studio Grove Sensoren](#anschlie%c3%9fen-von-seeed-studio-grove-sensoren)
    - [Seeed Studio Grove - 4 Ziffern Anzeige (4-Digit Display) (TM1637 LED MODULE)](#seeed-studio-grove---4-ziffern-anzeige-4-digit-display-tm1637-led-module)
    - [Seeed Studio Grove - Temperatursensor](#seeed-studio-grove---temperatursensor)
  - [Optional: CircuitPython selber bauen](#optional-circuitpython-selber-bauen)

Der Meowbit kann mittels CircuitPython programmiert werden:
<https://circuitpython.org/board/meowbit_v121/>

## Installieren von benötigten Biblotheken (CircuitPython Library Bundle)

Jedes CircuitPython-Programm, benötigt eine Menge
Informationen, um zu funktionieren. Der Grund, warum CircuitPython so einfach
zu bedienen ist, liegt darin, dass die meisten dieser Informationen in anderen
Dateien gespeichert sind und im Hintergrund arbeiten. Diese Dateien werden
Bibliotheken genannt. Einige von ihnen sind in CircuitPython integriert. Andere
werden auf dem CIRCUITPY-Laufwerk in einem Ordner namens `lib` gespeichert.
Eine Eigenschaft von CircuitPyhton ist das Programm-Code getrennt von der
Firmware gespeichert wird. Das Speichern von Programm-Code getrennt von der
Firmware macht es einfacher, sowohl den Programm-Code, als auch die
Bibliotheken, von denen Programm-Code abhängt, zu aktualisieren.

Ein Nachteil dieses Ansatzes der getrennten Bibliotheken ist, dass sie nicht
eingebaut sind. Um diese zu verwenden, müssen diese vor der Verwendung auf das
CIRCUITPY-Laufwerk kopiert werden. Eine Sammlung aller verfügbaren
CircuitPython-Biblotheken zum Herunterladen wird unter
<https://circuitpython.org/libraries> bereit gestellt.

Das Bundle enthält auch optimierte Versionen der
Bibliotheken mit der Dateierweiterung `.mpy`. Diese Dateien nehmen weniger Platz
auf dem Laufwerk ein und haben einen kleineren Speicherbedarf, wenn diese geladen
werden.

Hinweis: Die Bundle-Version muss der Version von CircuitPython entsprechen,
welche ausgeführt wird  - 5.x-Bibliothek für die Ausführung einer beliebigen
Version von CircuitPython 5, 4.x für die Ausführung einer beliebigen Version
von CircuitPython 4, usw. Wenn Sie die Bibliotheken Version nicht mit der
CircuitPython Version übereinstimmt, , werden höchstwahrscheinlich Fehler
aufgrund von Änderungen der Bibliotheksschnittstellen auftreten, die bei
größeren Versionsänderungen möglich sind.

Nachdem Herunterladen des CircuitPython Library Bundles sollte ein `lib`-Ordner
auf Ihrem `CIRCUITPY`-Laufwerk erstellt werden.

Anschließend können die Biblotheken welche verwendet werden soll aus dem
heruntergeladenen zip-Archive herausgesucht werden und in den `lib`-Ordner auf
dem `CIRCUITPY`-Laufwerk kopiert werden.

Für das nachfolgende Beispiel-Programm werden folgenden Biblotheken benötigt.

```shell
lib/
├── adafruit_bus_device
├── adafruit_display_text
├── adafruit_mpu6050.mpy
├── adafruit_register
├── adafruit_st7735r.mpy
└── mpu6050.py
```

Weitere Details sind in der Doku der
[mpu6050-CircuitPython-Bibliothek](https://github.com/adafruit/Adafruit_CircuitPython_MPU6050)
zu finden.

## Ein Beispiel-Programm

```python
import board
import displayio
import busio
import terminalio
import digitalio
import time
from adafruit_display_text import label
from adafruit_st7735r import ST7735R
import adafruit_mpu6050

displayio.release_displays()

spi = board.INTERNAL_SPI
tft_cs = board.DISP_CS
tft_dc = board.DISP_DC

display_bus = displayio.FourWire(spi, command=tft_dc, chip_select=tft_cs, reset=board.DISP_RST)
display = ST7735R(display_bus, width=160, height=128, rotation=90, backlight_pin=board.DISP_BL)


i2c = busio.I2C(board.SCL, board.SDA)
mpu = adafruit_mpu6050.MPU6050(i2c)

while True:
    print("Acceleration: X:%.2f, Y: %.2f, Z: %.2f m/s^2"%(mpu.acceleration))
    print("Gyro X:%.2f, Y: %.2f, Z: %.2f degrees/s"%(mpu.gyro))
    print("Temperature: %.2f C"%mpu.temperature)
    print("")
    time.sleep(1)
```

## Anschließen von Seeed Studio Grove Sensoren

Benötigt wird ein [Seeed Studio Grove Shield for micro:bit
v2.0](https://www.seeedstudio.com/Grove-Shield-for-micro-bit-v2-0.html). Und ein paar Sensoren:

- <https://docs.google.com/spreadsheets/d/123yBkwL2mo8Zt2CCPW4_MX4TCSTSj8BVLRSmSZeEYzg/edit#gid=1056399402>

Grove Inventor Kit for micro:bit User Manual:

- <https://github.com/SeeedDocument/Grove_kit_for_microbit/blob/master/res/Guide-Grove%20kit%20for%20microbit.pdf>

### Seeed Studio Grove - 4 Ziffern Anzeige (4-Digit Display) (TM1637 LED MODULE)

- Chip: TM1637
- Interface: Digital
- Operating / Voltage: 5V

- <https://www.seeedstudio.com/Grove-4-Digit-Display-p-1651.html>
- <https://github.com/bablokb/circuitpython-tm1637>
- <https://learn.adafruit.com/circuitpython-essentials/circuitpython-digital-in-out>

```python
import board
import TM1637


if __name__ == "__main__":
  CLK = board.P1
  DIO = board.P15
  display = TM1637.TM1637(CLK, DIO)
  #display.hex(0xceef)
  #display.numbers(11,11)
  #display.temperature(11)
  display.show("Ahoy")
  ## time delay_ms doesnt exist, the following will not work (for now).
  ## details: https://blog.oddbit.com/post/2018-05-03-using-a-tm-led-module-with-cir/
  #display.scroll("Ahoy Pirate")
```

### Seeed Studio Grove - Temperatursensor

- IC Manufacture: NTC Thermistors
- Sensor / IC : NCP18WF104F03RC
- Operating Voltage : 3.3/5V
- Interface: Analog
- Measurable Range: -40 to 125°C
- Accurac / Sensitivty: +/- 1.5°C

- <https://www.seeedstudio.com/Grove-Temperature-Sensor.html>
- <https://github.com/SeeedDocument/Grove-Temperature_Sensor_V1.2/raw/master/res/NCP18WF104F03RC.pdf>

```python
import time
import board
import adafruit_thermistor

#pin = analogio.AnalogIn(board.P1)
pin = board.P1

# pylint: disable=no-member
resistor = 100000
resistance = 100000
nominal_temp = 25
b_coefficient = 4250

thermistor = adafruit_thermistor.Thermistor(pin, resistor, resistance, nominal_temp, b_coefficient)

# print the temperature in C and F to the serial console every second
while True:
    celsius = thermistor.temperature
    fahrenheit = (celsius * 9 / 5) + 32
    print('== Temperature ==\n{} *C\n{} *F\n'.format(celsius, fahrenheit))
    time.sleep(1)
```

## Optional: CircuitPython selber bauen

```shell
git clone https://github.com/adafruit/circuitpython.git
cd circuitpython
git submodule sync
git submodule update --init
git submodule update --recursive
make -C mpy-cross
cd ports/stm32f4/
make V=1 BOARD=meowbit_v121
```
