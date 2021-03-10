# 1-Wire Communication

The python script below demonstrates how to communicate with a [DS2413](https://www.maximintegrated.com/en/products/interface/controllers-expanders/DS2413.html), a dual-channel addressable switch IC. You can get one on an easy-to-use [breakout board](https://www.adafruit.com/product/1551) from our friends at Adafruit.

```python
from binhoHostAdapter import binhoHostAdapter

from time import sleep

# change this to your COM port
binhoTesterCommPort = 'COM22'

print("Opening " + binhoTesterCommPort + "...")
print()

for i in range(3):
	
	# create the binhoHostAdapter object
	binhoTester = binhoHostAdapter.binhoHostAdapter(binhoTesterCommPort)

	print("Connecting to binho host adapter tester...")
	print()

	oneWireIndex = 0

	binhoTester.setLEDColor('CYAN')

	binhoTester.setOperationMode(0, '1WIRE')

	print(binhoTester.begin1WIRE(oneWireIndex, 0, True))
	print(binhoTester.reset1WIRE(oneWireIndex))

	print("Looking for a DS2413 on the bus")
	print(binhoTester.resetSearch1WIRE(oneWireIndex))
	print(binhoTester.search1WIRE(oneWireIndex))
	print(binhoTester.getAddress1WIRE(oneWireIndex))

	def write2413(state):

		print('state orig: ' + str(state))

		state |= 0xfc

		print('state now: ' + str(state))
		print('state inv: ' + str(~state + 256))

		print('write2413 func begin')
		print(binhoTester.reset1WIRE(oneWireIndex))
		print(binhoTester.select1WIRE(oneWireIndex))
		print(binhoTester.writeByte1WIRE(oneWireIndex, 90))
		print(binhoTester.writeByte1WIRE(oneWireIndex, state))
		print(binhoTester.writeByte1WIRE(oneWireIndex, ~state +256))
		print(binhoTester.readByte1WIRE(oneWireIndex))
		print(binhoTester.readByte1WIRE(oneWireIndex))
		print(binhoTester.reset1WIRE(oneWireIndex))


	write2413(3)
	print('Sleeping for a hot sec ... ')
	sleep(1.0)
	print('doing it again...')
	write2413(0)
	print('Finished!')

	binhoTester.close()
```

