# Binho + CircuitPython

The _Binho Nova_ host adapter works seamlessly with [CircuitPython](https://circuitpython.org/). You can leverage all of the open-source device drivers and example code right from your PC. In the video below, Shannon Morse walks through the process of setting this up from scratch, starting with Python installation, and shows how simple it is to use with Nova.

{% embed url="https://www.youtube.com/watch?v=aTzU04I9W1M" %}

The same instructions presented in the video can be found below for easy reference.

### **Setup**

### Prerequisites

This guide presumes that you already have Python 3.x and pip installed on your computer.

You can verify these requirements by entering the following command:

```bash
C:\Binho\adafruit>pip --version
pip 19.3.1 from c:\program files (x86)\python38-32\lib\site-packages\pip (python 3.8)
```

### Step 1: Setup Binho Nova Host Adapter Hardware

The _Binho Nova Multi-Protocol USB Host Adapter_ utilizes the standardized USB Communications Device Class driver in order to achieve maximum compatibility with as many systems as possible. As such, there's no driver to download and install for most operating systems. 

Certain operating systems like Mac and Ubuntu may require additional permissions to start using _Binho Nova_. In addition, Windows 7 does not have the standard USB CDC driver included as default. 

Please check the following guide to setup permissions on Mac/Ubuntu and Windows 7 driver setup:

{% page-ref page="../user-guide/using-the-device/software-installation.md" %}

### Step 2: Install the Binho Host Adapter Libraries

```bash
pip install binho-host-adapter
```

Verify Nova can communicate with binhoHostAdapter python library:

```bash
C:\Binho\adafruit>python
Python 3.8.0 (tags/v3.8.0:fa919fd, Oct 14 2019, 19:21:23) [MSC v.1916 32 bit (Intel)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> from binhoHostAdapter import binhoUtilities
>>> devices = binhoUtilities.binhoUtilities().listAvailableDevices()
>>> print(devices)
['COM8']
```

### Step 3: Install Adafruit Blinka

```bash
pip install adafruit-blinka
```

### Step 4: Set BLINKA\_NOVA environment variable

In order for Adafruit Blinka libraries to use _Binho Nova_, set the BLINKA\_NOVA environment variable with the following command:

Windows Command line:

```bash
set BLINKA_NOVA=1
```

Windows Powershell:

```bash
$Env:BLINKA_NOVA = "1"
```

Mac/Ubuntu:

```bash
export BLINKA_NOVA=1
```

Verify Binho Nova’s environment variable is set and Adafruit Blinka libraries can recognize and communicate with the adapter:

```bash
C:\Binho\adafruit>set BLINKA_NOVA=1

C:\Binho\adafruit>python
Python 3.8.0 (tags/v3.8.0:fa919fd, Oct 14 2019, 19:21:23) [MSC v.1916 32 bit (Intel)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import board
>>> dir(board)
['I2C', 'IO0', 'IO1', 'IO2', 'IO3', 'IO4', 'MISO', 'MOSI', 'RX', 'SCK',
'SCL', 'SCLK', 'SDA', 'SPI', 'SS0', 'SS1', 'TX', '__builtins__',
'__cached__', '__doc__', '__file__', '__loader__', '__name__',
'__package__', '__spec__', 'ap_board', 'board_id', 'detector', 'pin',
'sys']
>>>
```

## Examples

For the examples shown below, it may make sense to review the Connecting the Hardware guide which includes the pinout of the _Binho Nova_ connector for easy reference.

{% page-ref page="../user-guide/using-the-device/connecting-the-hardware.md" %}

### Bosch BME280 Temperature, Barometic Pressure, and Humidity Sensor

Install circuitpython bme280 python library:

```bash
pip install adafruit-circuitpython-bme280
```

#### SPI Bus Example:

![Pin Connections: IO0 to CS, I02 to SDO, IO3 to SCK, IO4 to SDI, 3V3 to VIN, GND to GND](https://lh5.googleusercontent.com/mfH5PjfhvpUEHfeYQTEVpf5hHrmzLhFE8Wp1z1tehSITd6RCWhmwWruzPs2gehfbcATuAvthX3lKjonOMBq8-m6pIXpj7A24rOeyFttEeI5uju-BV4i7rEc0BAz4FUuD7wB6NZtH)

This example uses Adafruit’s **digitalio** package to create a **DigitalInOut** object for Chip Select Pin and **busio** package to create a **SPI** object.

```python
import time
import board
import digitalio
import busio
import adafruit_bme280

# Create library object using our Bus SPI port
spi = busio.SPI(board.SCK, board.MOSI, board.MISO)
bme_cs = digitalio.DigitalInOut(board.IO0)
bme280 = adafruit_bme280.Adafruit_BME280_SPI(spi, bme_cs)
 
# change this to match the location's pressure (hPa) at sea level
bme280.sea_level_pressure = 1013.25
 
while True:
   print("\nTemperature: %0.1f C" % bme280.temperature)
   print("Humidity: %0.1f %%" % bme280.humidity)
   print("Pressure: %0.1f hPa" % bme280.pressure)
   print("Altitude = %0.2f meters" % bme280.altitude)
   time.sleep(2)
```

#### I2C Bus Example:

![Pin Connections: IO0 to SDI, IO2 to SCK, 3V3 to VIN, GND to GND](https://lh6.googleusercontent.com/4HlSRGApfGKvcUuhNCL_UxqLf6T_5JqAlRyXxUkJ1yYahM3TYn_5tC5a8piux1LIkvJoSNwkNqkBXbLj_NilWrljazSdTTkTCIq2-6nNZG_Dv2K3qLQjvFfr55OVTddSrJhmZX29)

This example uses Adafruit’s **busio** package to create an **I2C** object.

```python
import time
import board
import busio
import adafruit_bme280

# Create library object using our Bus I2C port
i2c = busio.I2C(board.SCL, board.SDA)
bme280 = adafruit_bme280.Adafruit_BME280_I2C(i2c)

# change this to match the location's pressure (hPa) at sea level
bme280.sea_level_pressure = 1013.25

while True:
   print("\nTemperature: %0.1f C" % bme280.temperature)
   print("Humidity: %0.1f %%" % bme280.humidity)
   print("Pressure: %0.1f hPa" % bme280.pressure)
   print("Altitude = %0.2f meters" % bme280.altitude)
   time.sleep(2)
```

Running the example code:

![](https://lh4.googleusercontent.com/W1T9UsdRcsSfsBrJT83ableWiv1L6ixeuJ-Q0KDPNnDt6anJUMvwNsv60NsmTo8Eyc5ZtrH4dVnVMezwm1yZ10yxLjcWSEOPWNB0FzO1_TwjSeI87mA_U8jrm35Bb0WdJtfPkux1)

### Blinking and Pulsing LED

![IO0 to LED Anode\(+\), LED Cathode\(-\) to Resistor, Resistor to GND ](https://lh4.googleusercontent.com/ngqrUuAKwqy3MgCtfkf4WEvyBm5OF6dV8RnUsUc1eOLM-Yq1GCMIpDg6QVAj68WJOAQz1FeNUMsQAglHGzEVigkSK7-LgA00PcqyS0KYemzjpAIbh7zfN9SSJ1epyXRGGR3tZh6W)

#### GPIO Example:

This example uses Adafruit’s **digitalio** package to create a **DigitalInOut** object.

```python
import time
import board
import digitalio

led = digitalio.DigitalInOut(board.IO0)
led.direction = digitalio.Direction.OUTPUT

while True:
    led.value = True
    time.sleep(0.5)
    led.value = False
    time.sleep(0.5)
```

#### PWM Example:

This example uses Adafruit’s **pulseio** package to create a **PWMOut** object.

```python
import time
import board
import pulseio

led = pulseio.PWMOut(board.IO0, frequency=5000, duty_cycle=0)

while True:
    for i in range(100):
        # PWM LED up and down
        if i < 50:
            # Up
            led.duty_cycle = int(i * 2 * 65535 / 100)
        Else:
            # Down
            led.duty_cycle = 65535 - int((i - 50) * 2 * 65535 / 100)        
        time.sleep(0.01)
```

Running the example code:

![](https://lh4.googleusercontent.com/Eqj3rye1GWxHzACyud0Tx1dMelwsaEvrBbp_HBGTPKHOzBh8VfyL2-8KoDTkgt8rQ6Rd1dnK4DAYiP4mZRoP3ZGUujylCjwjGHTp-Yj3b2eM-hVqQcHbGOKFZzVjx3l1-1QqL395)

### UART Bridge

The following UART example uses an [FTDI USB Cable](https://www.adafruit.com/product/70).

This example uses Adafruit’s **busio** package to create a **UART** object. It will read 3 characters from the FTDI cable which CoolTerm is connected to.  The script then sends ‘hello world’ to the FTDI cable which will display in CoolTerm.

```python
import board
import busio

uart = busio.UART(board.IO4, board.IO3, 115200, 8, None, 1, 1000)
data = uart.read(2)
# convert bytearray to string
data_string = ''.join([chr(b) for b in data])
print(data_string, end="")
uart.write('hello world')
uart.deinit()
```

Running the example code:

![](https://lh6.googleusercontent.com/5hjrF_ijMa8hqQFvlbNCvReqTUpKx2AaxpW8Q7esXbOZS-o3KtOvHb1SLQAKdrWdFRAKF8WZhqgHcNCCnCFbr45TPj8sKUj6ke7dfk_fcr23s8Ggb3-U1nWOzDw9XN5qvV-4vFuK)

