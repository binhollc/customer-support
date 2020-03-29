# UART Bridge with I2C

The example below provides a basic demonstration of how to use the _Binho Multi-Protocol USB Host Adapter_ as both a UART bridge and an I2C master to interact with the same DUT with both protocols.

```python
from binhoHostAdapter import binhoHostAdapter
from binhoHostAdapter import binhoUtilities

binhoTesterCommPort = 'COM22'

# create the binhoHostAdapter object
binhoTester = binhoHostAdapter.binhoHostAdapter(binhoTesterCommPort)

print("Connecting to binho host adapter tester...")
print

print(binhoTester.setLEDColor('YELLOW'))

print(binhoTester.setOperationMode(0, 'UART'))

print(binhoTester.setBaudRateUART(0, 115200))
print(binhoTester.setDataBitsUART(0, 8))
print(binhoTester.setParityUART(0, 'NONE'))
print(binhoTester.setStopBitsUART(0, 1))

print(binhoTester.beginBridgeUART(0))

binhoTester.writeBridgeUART("Testing...")
binhoTester.writeBridgeUART("more more")

print(binhoTester.stopBridgeUART("+++UART0"))
print(binhoTester.ping())

binhoTester.setOperationMode(0, 'I2C')
binhoTester.setPullUpStateI2C(0, "EN")
binhoTester.setClockI2C(0, 400000)

print(binhoTester.startI2C(0, 100))
print(binhoTester.writeByteI2C(0, 15))
print(binhoTester.writeByteI2C(0, 25))
print(binhoTester.endI2C(0))

binhoTester.setOperationMode(0, 'UART')
print(binhoTester.beginBridgeUART(0))

binhoTester.writeBridgeUART("Second Burst")
binhoTester.writeBridgeUART("Same as the first")
binhoTester.writeBridgeUART("A whole lot louder")
binhoTester.writeBridgeUART("And a whole lot wurst")

print(binhoTester.stopBridgeUART("+++UART0"))
print(binhoTester.ping())
```

