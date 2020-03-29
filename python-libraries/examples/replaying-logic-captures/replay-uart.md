# Replay UART

The following script can be used to repeat UART streams captured and exported from Saleae Logic. After the capture is completed, export the Async Serial analyzer results as a csv file. You can follow [this guide](https://support.saleae.com/user-guide/using-logic/saving-loading-and-exporting-data#exporting-analyzer-results) to export the file. The exported data should be in `HEX`-- Note that the base/radix of the exported data will match the current setting for display in the software.

Simply modify the parameters in lines 9-18 in the script below and then everything is ready to play back the file from the _Binho Multi-Protocol USB Host Adapter_.

For easy testing, here's an example export file from Saleae Logic that works with this script:

{% file src="../../../.gitbook/assets/uart\_9600.csv" %}

```python
from binhoHostAdapter import binhoHostAdapter
from binhoHostAdapter import binhoUtilities

from datetime import datetime, timedelta, time
import math
import csv


uartIndex = 0
uartBaudRate = 9600
uartDataBits = 8
uartParity = 'NONE'
uartStopBits = 1
uartEscapeString = "+++UART0"

captureExportFile = 'C:\\Users\\Batman\\Desktop\\LogicExports\\uart_9600.csv'

binhoCommPort = 'COM27'


# ---- No Need to Change Anything Below Here ----

print("Opening " + binhoCommPort + "...")
print()

# create the binhoHostAdapter object
binho = binhoHostAdapter.binhoHostAdapter(binhoCommPort)

print("Connecting to binho host adapter...")
print()

print("Connected!")
print()
binho.setLEDColor('YELLOW')

print("Setting UART bus parameters:")
print()
binho.setOperationMode(0, 'UART')

print('BaudRate: ' + str(uartBaudRate))
binho.setBaudRateUART(uartIndex, uartBaudRate)

print('Databits: ' + str(uartDataBits))
binho.setDataBitsUART(uartIndex, uartDataBits)

print('Parity Bit: ' + str(uartParity))
binho.setParityUART(uartIndex, uartParity)

print('Stop Bits: ' + str(uartStopBits))
binho.setStopBitsUART(uartIndex, uartStopBits)

binho.setEscapeSequenceUART(uartIndex, uartEscapeString)

print()
print("Computing USB Transit time...")
t0 = datetime.now()
binho.ping()
binho.ping()
binho.ping()
binho.ping()
binho.ping()
t1 = datetime.now()

avgUSBTxTime = (t1 - t0)/5
print('Average USB Tx Time = ' + str(avgUSBTxTime) + 's')
print()


print("Beginning Replay...")
print()
binho.beginBridgeUART(uartIndex)

with open(captureExportFile) as captureExport:
	capture_reader = csv.reader(captureExport, delimiter=',')
	rowCount = 0

	startTime = datetime.now()
	prevTimestamp = startTime
	prevRowTime = 0
	
	for row in capture_reader:

		if rowCount == 0:

			# These are the column headers, just advance to the next row
			rowCount += 1
			print('Row#\tTimestamp\t\t\t\tData')

		else:

			currRowTime = float(row[0])
			payload = int(row[1], 16)

			deltaTimems = (currRowTime - prevRowTime) * 1000
			#deltaTimems = 0.125 * 1000
			#print('DeltaT: ' + str(deltaTimems))
			#print('computed delta: ' + str(timedelta(0,0,0,math.floor(deltaTimems))))

			while (datetime.now() - prevTimestamp) < timedelta(0,0,0,math.floor(deltaTimems)):
				#nothing, sleep does not have high enough resolution
				# print('WAITING ' + str(datetime.now() - prevTimestamp))
				pass

			binho.writeBridgeUART(chr(payload))
			prevTimestamp = datetime.now()
			prevRowTime = currRowTime

			print(str(rowCount) + '\t' + str(prevTimestamp) + '\t\t' + str(payload))

			rowCount += 1

binho.stopBridgeUART(uartEscapeString)
binho.ping()
print('Finished Replaying...')
print()
binho.setLEDColor('BLUE')
binho.close()
print('Goodbye!')
```

