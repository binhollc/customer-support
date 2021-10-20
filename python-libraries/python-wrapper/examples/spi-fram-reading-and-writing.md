# SPI FRAM Reading And Writing

{% hint style="success" %}
We highly encourage everyone to use our[ new Python package](https://support.binho.io/python-libraries/binho-python-package) which is packed with features. This library is still supported, but is not recommended for new design.
{% endhint %}

This example is a very brief demonstration of using SPI to read from and write to a SPI FRAM device. This particular examples uses the [64Kbit FRAM Breakout Board](https://www.adafruit.com/product/1897) from Adafruit.

```python
from binhoHostAdapter import binhoHostAdapter
from binhoHostAdapter import binhoUtilities

# Change this to match your COMPort
default_commport = "COM11"

print("SPI FRAM Example using Binho Host Adapter")
print("v1.0 -- Jonathan Georgino <jonathan@binho.io>")
print

utilities = binhoUtilities.binhoUtilities()
devices = utilities.listAvailableDevices()

if len(devices) == 1:
	COMPORT = devices[0]
	print("Found 1 attached adapter @ " + devices[0])
	print
else:
	COMPORT = default_commport
	print("Found more than 1 attached adapter, using default port " + COMPORT)
	print

print("Opening " + COMPORT + "...")
print

# create the binhoHostAdapter object
hostAdapter = binhoHostAdapter.binhoHostAdapter(COMPORT)

print(hostAdapter.setOperationMode(0, 'SPI'))
print(hostAdapter.setClockSPI(0, 1000000))
print(hostAdapter.setModeSPI(0, 0))
print(hostAdapter.setIOpinMode(0, 'DOUT'))
print(hostAdapter.setIOpinValue(0, 'HIGH'))

print(hostAdapter.beginSPI(0))
print(hostAdapter.setIOpinValue(0, 'LOW'))

print(hostAdapter.transferSPI(0, 0x9f))
print(hostAdapter.transferSPI(0, 0))
print(hostAdapter.transferSPI(0, 0))
print(hostAdapter.transferSPI(0, 0))
print(hostAdapter.transferSPI(0, 0))

print(hostAdapter.setIOpinValue(0, 'HIGH'))
print(hostAdapter.endSPI(0))


print(hostAdapter.clearBuffer(0))
print(hostAdapter.addByteToBuffer(0, 0x9f))

print(hostAdapter.beginSPI(0))
print(hostAdapter.setIOpinValue(0, 'LOW'))

print(hostAdapter.transferBufferSPI(0, 5))

print(hostAdapter.setIOpinValue(0, 'HIGH'))
print(hostAdapter.endSPI(0))

print(hostAdapter.readBuffer(0, 5))
```
