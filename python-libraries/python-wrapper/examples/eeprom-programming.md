# I2C EEPROM Reading And Writing

The example below demonstrates how to read from and write to an I2C EEPROM. This particular demo uses the [SparkX Qwiic 512Kbit EEPROM](https://www.sparkfun.com/products/14764) from Sparkfun.

```python
from binhoHostAdapter import binhoHostAdapter

from time import sleep

class Sparkfun_QwiicEEPROM:

    def __init__(self, addr, pageWriteMs):
        self.eeprom_address = addr;
        self.eeprom_pageWriteTime_ms = pageWriteMs

    def begin(self, adapter):
        self.hostAdapter = adapter

    def writePage(self, memAddress, pageData):
        memAddressHigh = (memAddress >> 8)
        memAddressLow = memAddress & 0xFF

        fullAddress = [memAddressHigh, memAddressLow]

        ## load the address into the buffer
        self.hostAdapter.writeToBuffer(0, 0, fullAddress)

        ## load the data into the buffer
        self.hostAdapter.writeToBuffer(0, 2, pageData)

        ## I2C Start Transmission
        self.hostAdapter.startI2C(0, self.eeprom_address)

        ## I2C Write Buffer
        self.hostAdapter.writeFromBufferI2C(0, 34)

        ## I2C End Transmission
        self.hostAdapter.endI2C(0)

        ## Clear the Buffer
        self.hostAdapter.clearBuffer(0)

    def readPage(self, memAddress):
        memAddressHigh = (memAddress >> 8)
        memAddressLow = memAddress & 0xFF

        ## Clear the Buffer
        self.hostAdapter.clearBuffer(0)

        ## I2C Start Transmission
        print(self.hostAdapter.startI2C(0, self.eeprom_address))

        print(self.hostAdapter.writeByteI2C(0, memAddressHigh))
        print(self.hostAdapter.writeByteI2C(0, memAddressLow))

        ## I2C End Transmission
        print(self.hostAdapter.endI2C(0))

        ## Request the data
        self.hostAdapter.readToBufferI2C(0, self.eeprom_address, 32)

        print(self.hostAdapter.readBuffer(0, 32))

    def pageWriteDelay(self):
        return self.eeprom_pageWriteTime_ms / 1000.0

# Change this to match your COMPort
COMPORT = "COM27"

print("EEPROM Example using Binho Host Adapter")
print("v1.0 -- Jonathan Georgino <jonathan@binho.io>")
print()

# create the binhoHostAdapter object
binho = binhoHostAdapter.binhoHostAdapter(COMPORT)
eeprom = Sparkfun_QwiicEEPROM(0xA0, 1)

print("Connecting to host adapter...")
print(binho.getDeviceID())
print()

print("Configuring I2C settings...")
binho.setOperationMode(0, "I2C")
binho.setPullUpStateI2C(0, "EN")
binho.setClockI2C(0, 400000)
print("Ready!")
print()


eeprom.begin(binho)

currentMemoryAddress = 0x0000

print("Starting to write to the EEPROM ...")

for i in range(4):

    print("Writing Page #" + str(i))

    pageData = [0xAA+i, 0x10, 0x20, 0x30, 0x40, 0x50, 0x60, 0x70, 0x80, 0x90, 0xA0, 0xB0, 0xC0, 0xD0, 0xE0, 0xF0]
    pageData += [0xAB+i, 0x11, 0x21, 0x31, 0x41, 0x51, 0x61, 0x71, 0x81, 0x91, 0xA1, 0xB1, 0xC1, 0xD1, 0xE1, 0xF1]

    print("PageData=")
    print(pageData)

    eeprom.writePage(currentMemoryAddress, pageData)

    currentMemoryAddress += 32

    sleep(eeprom.pageWriteDelay())

    print()
    print()

currentMemoryAddress = 0

print("Starting to read to the EEPROM ...")

for i in range(4):

    print("Reading Page #" + str(i))

    eeprom.readPage(currentMemoryAddress)

    currentMemoryAddress += 32

    print()
    print()

binho.close()
```

