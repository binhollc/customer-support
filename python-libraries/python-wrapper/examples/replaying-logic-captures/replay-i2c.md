# Replay I2C

{% hint style="success" %}
We highly encourage everyone to use our[ new Python package](https://support.binho.io/python-libraries/binho-python-package) which is packed with features. This library is still supported, but is not recommended for new design.
{% endhint %}

The following script can be used to repeat I2C transactions captured and exported from Saleae Logic. After the capture is completed, export the I2C analyzer results as a csv file. You can follow [this guide](https://support.saleae.com/user-guide/using-logic/saving-loading-and-exporting-data#exporting-analyzer-results) to export the file. The exported data should be in `HEX`-- Note that the base/radix of the exported data will match the current setting for display in the software.

Simply modify the parameters in lines 9-15 in the script below and then everything is ready to play back the file from the _Binho Nova Multi-Protocol USB Host Adapter_.

For easy testing, here's an example export file from Saleae Logic that works with this script:

{% file src="../../../../.gitbook/assets/i2c\_eeprom.csv" %}

```python
from binhoHostAdapter import binhoHostAdapter
from binhoHostAdapter import binhoUtilities

from datetime import datetime, timedelta, time
import math
import csv


i2cIndex = 0
i2cClockFreq = 400000
i2cPullEn = 'EN'

captureExportFile = 'C:\\Users\\Batman\\Desktop\\LogicExports\\i2c_eeprom.csv'

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

print("Setting I2C bus parameters:")
print()
binho.setOperationMode(0, 'I2C')

print('Clock Frequency: ' + str(i2cClockFreq))
binho.setClockI2C(i2cIndex, i2cClockFreq)

print('PullUps: ' + str(i2cPullEn))
binho.setPullUpStateI2C(i2cIndex, i2cPullEn)

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
	deviceAddress = 0
	payload = []
	isReadPacket = True
	
	for row in capture_reader:

		if rowCount == 0:

			# These are the column headers, just advance to the next row
			rowCount += 1
			print('PacketID#\tTimestamp\t\t\tAddress\tType\tLen(Bytes)')

		else:
			rowPacketID = row[1]

			#check if packetID is empty
			if rowPacketID == "":
				#this is a single write command with no data to follow it, skip it
				rowCount +=1

			elif int(row[1]) == currentPacketID:
				currRowTime = float(row[0])
				deviceAddress = int(row[2],16)
				payload.append(int(row[3], 16))

				if row[4] == 'Write':
					isReadPacket = False
				else:
					isReadPacket = True

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

				if isReadPacket:

					print(str(currentPacketID) + '\t\t' + str(datetime.now()) + '\t' +  format(deviceAddress, '#02x') + '\tREAD\t' + str(len(payload)))

					binho.readBytesI2C(i2cIndex, deviceAddress, len(payload))

				else:

					print(str(currentPacketID) + '\t\t' + str(datetime.now()) + '\t' + format(deviceAddress, '#02x') + '\tWRITE\t' + str(len(payload)))
					binho.startI2C(i2cIndex, deviceAddress)

					for dataByte in payload:
						binho.writeByteI2C(i2cIndex, dataByte)

					binho.endI2C(i2cIndex)

				payload = []
				prevTimestamp = datetime.now()
				prevRowTime = currRowTime

				currentPacketID = int(row[1])
				currRowTime = float(row[0])

				deviceAddress = int(row[2],16)
				payload.append(int(row[3], 16))

				if row[4] == 'Write':
					isReadPacket = False
				else:
					isReadPacket = True

				rowCount += 1

print('Finished Replaying...')
print()
binho.setLEDColor('BLUE')
binho.close()
print('Goodbye!')
```

