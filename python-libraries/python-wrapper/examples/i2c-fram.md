# I2C FRAM Reading And Writing



{% hint style="success" %}
We highly encourage everyone to use our[ new Python package](https://support.binho.io/python-libraries/binho-python-package) which is packed with features. This library is still supported, but is not recommended for new design.
{% endhint %}

The example below demonstrates how to read from and write to an I2C FRAM. Beyond that, it's also an excellent example of how to port an open-source Arduino library into a python script that can utilize the Binho Multi-Protocol USB Host Adapter.

This example is uses a 256Kbit I2C FRAM \(MB85RC256V / Fujitsu\) breakout board from our friends over at Adafruit. You can purchase it [here](https://www.adafruit.com/product/1895).

```python
from binhoHostAdapter import binhoHostAdapter
from binhoHostAdapter import binhoUtilities

MB85RC_DEFAULT_ADDRESS = 0xA0
MB85RC_SLAVE_ID = 0xF8

class Adafruit_FRAM_I2C:

	# CONSTRUCTOR
	def __init__(self, addr):

		self.framInitialised = False
		self.i2c_addr = addr

	# PUBLIC FUNCTIONS

	# initializes I2C and configures the chip
	# call this function before doing anything else
	def begin(self, adapter):

		self.hostAdapter = adapter

		# Make sure we're actually connected
		manufacturerID = 0
		productID = 0
		deviceInfo = [manufacturerID, productID]

		self.getDeviceID(deviceInfo)

		manufacturerID = deviceInfo[0]
		productID = deviceInfo[1]

		deviceFound = True

		if manufacturerID != 0x00A:
			print("Unexpected Manufacturer ID: " + str(deviceInfo[0]))
			deviceFound = False

		if productID != 0x510:
			print("Unexpected Product ID: " + str(productID))
			deviceFound = False

		# Everything seems to be properly initialized and connected
		self._framInitialised = True;

		return deviceFound

	# Writes a byte at the specific FRAM address
	def write8(self, framAddr, value):
		
		self.hostAdapter.startI2C(0, self.i2c_addr)
		self.hostAdapter.writeByteI2C(0, framAddr >> 8)
		self.hostAdapter.writeByteI2C(0, framAddr & 0xFF)
		self.hostAdapter.writeByteI2C(0, value)
		self.hostAdapter.endI2C(0)

	# Reads an 8 bit value from the specified FRAM address
	def read8(self, framAddr):

		self.hostAdapter.startI2C(0, self.i2c_addr)
		self.hostAdapter.writeByteI2C(0, framAddr >> 8)
		self.hostAdapter.writeByteI2C(0, framAddr & 0xFF)
		self.hostAdapter.endI2C(0)

		response = self.hostAdapter.readByteI2C(0, self.i2c_addr)

		data = response.split()

		if len(data) == 3:
			return int(data[2])
		else:
			return 0

	# Reads the Manufacturer ID and the Product ID frm the IC
	def getDeviceID(self, deviceInfo):

		self.hostAdapter.startI2C(0, MB85RC_SLAVE_ID)
		self.hostAdapter.writeByteI2C(0, 0xA0)
		self.hostAdapter.endI2C(0, True)

		response = self.hostAdapter.readBytesI2C(0, MB85RC_SLAVE_ID, 3)

		data =response.split()

		if len(data) == 5:
			deviceInfo[0] = (int(data[2]) << 4) + (int(data[3]) >> 4)
			deviceInfo[1] = ((int(data[3]) & 0x0F) << 8) + int(data[4])


# Change this to match your COMPort
default_commport = "COM27"

print("I2C FRAM Example using Binho Host Adapter")
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
binhoDevice = binhoHostAdapter.binhoHostAdapter(COMPORT)
binhoDevice.setNumericalBase('10')

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setPullUpStateI2C(0, '1')
binhoDevice.setClockI2C(0, 1000000)

framMemory = Adafruit_FRAM_I2C(MB85RC_DEFAULT_ADDRESS)

if framMemory.begin(binhoDevice) == True:

	# found the I2C FRAM

	memData = [0 for x in range(32)]
	for i in range(32):
		memData[i] = framMemory.read8(i)

	print(memData)

	for i in range(32):
		framMemory.write8(i, i*2)

	for i in range(32):
		memData[i] = framMemory.read8(i)

	print(memData)

	binhoDevice.close()

else:
	# couldn't read the FRAM manufacturer and product ID bits
	print("I2C FRAM not identified... check your connections?")

```

For comparison's sake, you can see the Adafruit Arduino library that this example was ported from [here](https://github.com/adafruit/Adafruit_FRAM_I2C).

