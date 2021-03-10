# I2C Peripheral Mode

This example demonstrates how to use _Binho Nova_ as an I2C Peripheral device, with a device address of 0xA0 and operating in USEPTR mode of emulation.

```python
import os
import subprocess
import shutil
import psutil
import sys

from binhoHostAdapter import binhoHostAdapter
from binhoHostAdapter import binhoUtilities

from time import sleep

# enter the deviceID of target test device
binhoID = '0x1c4780b050515950362e3120ff141c2a'

utilities = binhoUtilities.binhoUtilities()

binhoComPorts = utilities.getPortByDeviceID(binhoID)
binhoComPort = 0

if len(binhoComPorts) == 0:
    print("ERROR: No Binho Slave Device found!")
    exit(1)
elif len(binhoComPorts) > 1:
    print("ERROR: More than one Binho Slave Device found!")
    exit(1)
else:
    binhoComPort = binhoComPorts[0]
    print("Found Binho Slave Device on " + binhoComPort)


print("Opening " + binhoComPort + "...")
print()

# create the binhoHostAdapter object
binhoNova = binhoHostAdapter.binhoHostAdapter(binhoComPort)

print("Configuring Binho Nova I2C Slave with address of 0xA0")

binhoNova.setLEDColor('RED')
binhoNova.setOperationMode(0, 'I2C')
binhoNova.setPullUpStateI2C(0, "EN")

# assign the address of 0xA0
binhoNova.setSlaveAddressI2C(0, 0xA0)

# set the mode to USEPTR mode
binhoNova.setSlaveModeI2C(0, "USEPTR")

# load some initial values into registers 0x06 - 0x11
binhoNova.setSlaveRegisterI2C(0,0x06,0x00)
binhoNova.setSlaveRegisterI2C(0,0x07,0x06)
binhoNova.setSlaveRegisterI2C(0,0x08,0x05)
binhoNova.setSlaveRegisterI2C(0,0x09,0x00)
binhoNova.setSlaveRegisterI2C(0,0x0A,0x04)
binhoNova.setSlaveRegisterI2C(0,0x0B,0x00)
binhoNova.setSlaveRegisterI2C(0,0x0C,0x00)
binhoNova.setSlaveRegisterI2C(0,0x0D,0x00)
binhoNova.setSlaveRegisterI2C(0,0x0E,0x00)
binhoNova.setSlaveRegisterI2C(0,0x0F,0x00)
binhoNova.setSlaveRegisterI2C(0,0x10,0x00)
binhoNova.setSlaveRegisterI2C(0,0x11,0xFF)

# make all the registers readonly by setting WriteMasks to 0x00
binhoNova.setSlaveWriteMaskI2C(0, 0x06, 0x00)
binhoNova.setSlaveWriteMaskI2C(0, 0x07, 0x00)
binhoNova.setSlaveWriteMaskI2C(0, 0x08, 0x00)
binhoNova.setSlaveWriteMaskI2C(0, 0x09, 0x00)
binhoNova.setSlaveWriteMaskI2C(0, 0x0A, 0x00)
binhoNova.setSlaveWriteMaskI2C(0, 0x0B, 0x00)
binhoNova.setSlaveWriteMaskI2C(0, 0x0C, 0x00)
binhoNova.setSlaveWriteMaskI2C(0, 0x0D, 0x00)
binhoNova.setSlaveWriteMaskI2C(0, 0x0E, 0x00)
binhoNova.setSlaveWriteMaskI2C(0, 0x0F, 0x00)
binhoNova.setSlaveWriteMaskI2C(0, 0x10, 0x00)
binhoNova.setSlaveWriteMaskI2C(0, 0x11, 0x00)

# set the Pointer register to 0x00
binhoNova.setSlaveWriteMaskI2C(0, 'PTR', 0x00)
print(binhoNova.getSlaveWriteMaskI2C(0, 'PTR'))

# read back the configuration and print it out
for i in range(12):
    print(binhoNova.getSlaveRegisterI2C(0,6+i))
    print(binhoNova.getSlaveWriteMaskI2C(0,6+i))
    print()

print()
print("It's Over!")
binhoNova.close()
```

