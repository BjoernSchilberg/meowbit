# CircuitPython

- [CircuitPython](#circuitpython)
  - [Optional: CircuitPython selber bauen](#optional-circuitpython-selber-bauen)

Der Meowbit kann mittels CircuitPython programmiert werden:
<https://circuitpython.org/board/meowbit_v121/>

Ein Beispiel-Programm

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
