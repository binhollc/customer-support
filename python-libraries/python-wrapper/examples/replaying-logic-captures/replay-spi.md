# Replay SPI

{% hint style="success" %}
We highly encourage everyone to use our[ new Python package](https://support.binho.io/python-libraries/binho-python-package) which is packed with features. This library is still supported, but is not recommended for new design.
{% endhint %}

The following script can be used to repeat SPI transactions captured and exported from Saleae Logic. After the capture is completed, export the SPI analyzer results as a csv file. You can follow [this guide](https://support.saleae.com/user-guide/using-logic/saving-loading-and-exporting-data#exporting-analyzer-results) to export the file. The exported data should be in `HEX`-- Note that the base/radix of the exported data will match the current setting for display in the software.

Simply modify the parameters in lines 9-17 in the script below and then everything is ready to play back the file from the _Binho Multi-Protocol USB Host Adapter_.

For easy testing, here's an example export file from Saleae Logic that works with this script:

{% file src="../../../../.gitbook/assets/spi_example.csv" %}

```python
from binhoHostAdapter import binhoHostAdapter
from binhoHostAdapter import binhoUtilities

from datetime import datetime, timedelta, time
import math
import csv


spiIndex = 0
spiClockFreq = 1000000
spiMode = 0
spiCSPinIONumber = 0
spiCSActiveLow = True

captureExportFile = 'C:\\Users\\Jonathan\\Desktop\\capture exports\\spi_example.csv'

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

print("Setting SPI bus parameters:")
print()
binho.setOperationMode(0, 'SPI')

print('Clock Frequency: ' + str(spiClockFreq))
binho.setClockSPI(spiIndex, spiClockFreq)

print('Mode: ' + str(spiMode))
binho.setModeSPI(spiIndex, spiMode)

print('CS Pin: IO' + str(spiCSPinIONumber))
binho.setIOpinMode(spiCSPinIONumber, 'DOUT')

if spiCSActiveLow:
	binho.setIOpinValue(spiCSPinIONumber, 'HIGH')
else:
	binho.setIOpinValue(spiCSPinIONumber, 'LOW')

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


with open(captureExportFile) as captureExport:
	capture_reader = csv.reader(captureExport, delimiter=',')
	rowCount = 0

	startTime = datetime.now()
	prevTimestamp = startTime
	prevRowTime = 0

	currentPacketID = 0
	payload = []
	
	for row in capture_reader:

		if rowCount == 0:

			# These are the column headers, just advance to the next row
			rowCount += 1
			print('PacketID#\tTimestamp\t\t\tLen(Bytes)')

		else:

			#print('A: ' + row[0] + '\tB: ' + row[1] + '\tC: ' + row[2] + '\tD:' + row[3])

			rowPacketID = row[1]

			#check if packetID is empty
			if rowPacketID == "":
				#this is a single write command with no data to follow it, skip it
				rowCount +=1

			elif int(rowPacketID) == currentPacketID:
				currRowTime = float(row[0])
				payload.append(int(row[2], 16))
				#print('Appening: ' + str(int(row[2], 16)) + ' row:' + str(rowCount))

				rowCount += 1

			else:
				# starts a new packet, so do the transaction we loaded

				deltaTimems = (currRowTime - prevRowTime) * 1000
				#deltaTimems = 0.125 * 1000
				#print('DeltaT: ' + str(deltaTimems))
				print('Waiting ' + str(timedelta(0,0,0,math.floor(deltaTimems))) + ' until next packet transmission', end='\r')

				while (datetime.now() - prevTimestamp) < timedelta(0,0,0,math.floor(deltaTimems)):
					#nothing, sleep does not have high enough resolution
					#print('WAITING ' + str(datetime.now() - prevTimestamp))
					pass

				print(str(currentPacketID) + '\t\t' + str(datetime.now()) + '\t' + '\t' + str(len(payload)))

				binho.clearBuffer(0)

				for dataByte in payload:
					binho.addByteToBuffer(0, dataByte)

				binho.beginSPI(spiIndex)

				if spiCSActiveLow:
					binho.setIOpinValue(spiCSPinIONumber, 'LOW')
				else:
					binho.setIOpinValue(spiCSPinIONumber, 'HIGH')

				binho.transferBufferSPI(spiIndex, len(payload))

				if spiCSActiveLow:
					binho.setIOpinValue(spiCSPinIONumber, 'HIGH')
				else:
					binho.setIOpinValue(spiCSPinIONumber, 'LOW')

				binho.endSPI(spiIndex)

				currentPacketID += 1
				payload = []
				prevTimestamp = datetime.now()
				prevRowTime = currRowTime

				currRowTime = float(row[0])
				payload.append(int(row[2], 16))
				rowCount += 1
				
print('Finished Replaying...')
print()
binho.setLEDColor('BLUE')
binho.close()
print('Goodbye!')
```
