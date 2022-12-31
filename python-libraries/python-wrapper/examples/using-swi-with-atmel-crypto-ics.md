# SWI with Atmel Crypto ICs

{% hint style="success" %}
We highly encourage everyone to use our[ new Python package](https://support.binho.io/python-libraries/binho-python-package) which is packed with features. This library is still supported, but is not recommended for new design.
{% endhint %}

The following python script demonstrates how to use the SWI host Nova functionality of the Binho Multi-Protocol USB Host Adapter with the Atmel (now Microchip) [ATSHA204A ](https://www.microchip.com/wwwproducts/en/ATsha204a)CryptoAuthentication IC. Communication with other devices from this family will be strikingly similar.

```python
from binhoHostAdapter import binhoHostAdapter
from binhoHostAdapter import binhoUtilities

# change this to your COM port
binhoTesterCommPort = 'COM22'

# create the binhoHostAdapter object
binhoTester = binhoHostAdapter.binhoHostAdapter(binhoTesterCommPort)

print("Connecting to binho host adapter tester...")
print()

swiIndex = 0

binhoTester.setLEDColor('CYAN')

binhoTester.setOperationMode(0, 'SWI')

binhoTester.beginSWI(swiIndex, 0, True)

# Begin Wake/ID Example
binhoTester.sendTokenSWI(swiIndex, "WAKE")

print(binhoTester.receiveBytesSWI(swiIndex, 4))

# Begin Serial Number Example
binhoTester.setPacketOpCodeSWI(swiIndex, 0x02)
binhoTester.setPacketParam1SWI(swiIndex, 0x00)
binhoTester.setPacketParam2SWI(swiIndex, 0x0000)

binhoTester.sendPacketSWI(swiIndex)

print(binhoTester.receiveBytesSWI(swiIndex, 7))

binhoTester.setPacketOpCodeSWI(swiIndex, 0x02)
binhoTester.setPacketParam1SWI(swiIndex, 0x00)
binhoTester.setPacketParam2SWI(swiIndex, 0x0002)

binhoTester.sendPacketSWI(swiIndex)

print(binhoTester.receiveBytesSWI(swiIndex, 7))

binhoTester.setPacketOpCodeSWI(swiIndex, 0x02)
binhoTester.setPacketParam1SWI(swiIndex, 0x00)
binhoTester.setPacketParam2SWI(swiIndex, 0x0003)

binhoTester.sendPacketSWI(swiIndex)

print(binhoTester.receiveBytesSWI(swiIndex, 7))

# Begin MAC Challenge Example

print('clearing buffer')
print(binhoTester.clearPacketSWI(0))

data = [0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88, 0x99, 0xAA, 0xBB, 0xCC, 0xDD, 0xEE, 0xFF, 0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88, 0x99, 0xAA, 0xBB, 0xCC, 0xDD, 0xEE, 0xFF]

print(binhoTester.writeToBuffer(0, 0, data))

print(binhoTester.setPacketOpCodeSWI(swiIndex, 0x08))
print(binhoTester.setPacketParam1SWI(swiIndex, 0x00))
print(binhoTester.setPacketParam2SWI(swiIndex, 0x0000))

print(binhoTester.setPacketDataFromBufferSWI(swiIndex, 32, "BUF0"))

print(binhoTester.sendPacketSWI(swiIndex))

#these first two will return NG because not enough time passed for the execution of the above command
binhoTester.receiveBytesSWI(swiIndex, 0x23)
binhoTester.receiveBytesSWI(swiIndex, 0x23)

# this one will actually return the response we are looking for
print(binhoTester.receiveBytesSWI(swiIndex, 0x23))

print(binhoTester.setPacketOpCodeSWI(swiIndex, 0x08))
print(binhoTester.setPacketParam1SWI(swiIndex, 0x00))
print(binhoTester.setPacketParam2SWI(swiIndex, 0x0000))

binhoTester.setPacketDataSWI(swiIndex, 0, 0x00)
binhoTester.setPacketDataSWI(swiIndex, 1, 0x11)
binhoTester.setPacketDataSWI(swiIndex, 2, 0x22)
binhoTester.setPacketDataSWI(swiIndex, 3, 0x33)
binhoTester.setPacketDataSWI(swiIndex, 4, 0x44)
binhoTester.setPacketDataSWI(swiIndex, 5, 0x55)
binhoTester.setPacketDataSWI(swiIndex, 6, 0x66)
binhoTester.setPacketDataSWI(swiIndex, 7, 0x77)
binhoTester.setPacketDataSWI(swiIndex, 8, 0x88)
binhoTester.setPacketDataSWI(swiIndex, 9, 0x99)
binhoTester.setPacketDataSWI(swiIndex, 10, 0xAA)
binhoTester.setPacketDataSWI(swiIndex, 11, 0xBB)
binhoTester.setPacketDataSWI(swiIndex, 12, 0xCC)
binhoTester.setPacketDataSWI(swiIndex, 13, 0xDD)
binhoTester.setPacketDataSWI(swiIndex, 14, 0xEE)
binhoTester.setPacketDataSWI(swiIndex, 15, 0xFF)
binhoTester.setPacketDataSWI(swiIndex, 16, 0x00)
binhoTester.setPacketDataSWI(swiIndex, 17, 0x11)
binhoTester.setPacketDataSWI(swiIndex, 18, 0x22)
binhoTester.setPacketDataSWI(swiIndex, 19, 0x33)
binhoTester.setPacketDataSWI(swiIndex, 20, 0x44)
binhoTester.setPacketDataSWI(swiIndex, 21, 0x55)
binhoTester.setPacketDataSWI(swiIndex, 22, 0x66)
binhoTester.setPacketDataSWI(swiIndex, 23, 0x77)
binhoTester.setPacketDataSWI(swiIndex, 24, 0x88)
binhoTester.setPacketDataSWI(swiIndex, 25, 0x99)
binhoTester.setPacketDataSWI(swiIndex, 26, 0xAA)
binhoTester.setPacketDataSWI(swiIndex, 27, 0xBB)
binhoTester.setPacketDataSWI(swiIndex, 28, 0xCC)
binhoTester.setPacketDataSWI(swiIndex, 29, 0xDD)
binhoTester.setPacketDataSWI(swiIndex, 30, 0xEE)
binhoTester.setPacketDataSWI(swiIndex, 31, 0xFF)

print(binhoTester.sendPacketSWI(swiIndex))

#these first two will return NG because not enough time passed for the execution of the above command
binhoTester.receiveBytesSWI(swiIndex, 0x23)
binhoTester.receiveBytesSWI(swiIndex, 0x23)

# this one will actually return the response we are looking for
print(binhoTester.receiveBytesSWI(swiIndex, 0x23))

binhoTester.close()
```

A special shout out to our friends at [Sparkfun ](https://www.sparkfun.com/)who produced a similar example above for their ATSHA204 Arduino Library many, many moons ago. This demo is built to match their example.
