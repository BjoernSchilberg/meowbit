# CircuitPython

- [CircuitPython](#circuitpython)
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
