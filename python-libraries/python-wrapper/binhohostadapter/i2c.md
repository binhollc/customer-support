# I2C Functions

{% hint style="success" %}
We highly encourage everyone to use our[ new Python package](https://support.binho.io/python-libraries/binho-python-package) which is packed with features. This library is still supported, but is not recommended for new design.
{% endhint %}

### setClockI2C(i2cIndex, frequency)

This function sets the clock frequency of the I2C bus. The Binho _Nova Multi-Protocol USB Host Adapter_ supports I2C bus speeds from 100kHz to 4MHz. Per the I2C specification, the common clock frequencies are 100kHz, 400kHz, and 1MHz, although it's possible to run the bus at other frequencies.

**Inputs:**

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `frequency`, which is an integer value in Hertz in the range of 100000 to 4000000. Note that the frequency value is truncated to 1kHz resolution. For example, a frequency value of will 200800 will result in an I2C clock frequency of 200kHz.

**Outputs:**

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setClockI2C(0, 1000000)
```

### getClockI2C(i2cIndex)

This function gets the clock frequency of the I2C bus. The _Binho Nova_ supports I2C bus speeds from 100kHz to 4MHz. Per the I2C specification, the common clock frequencies are 100kHz, 400kHz, and 1MHz, although it's possible to run the bus at other frequencies.

**Inputs:**

This function takes one parameter:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.

**Outputs:**

The host adapter will respond with '-I2C0 CLK' followed by a space followed by the current I2C clock frequency in Hertz.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setClockI2C(0, 1000000)
print(binhoDevice.getClockI2C(0))
#-I2C0 CLK 1000000
```

### setPullUpStateI2C(i2cIndex, state)

This function is used to engage / disengage the 2.2kOhm internal pull-up resistors on the SDA and SCL signals.

**Inputs:**

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `state`, which is the desired state of the pullup resistors, either 'EN' for enable or 'DIS' for disable. See the example below for additional acceptable values for convenience.

**Outputs:**

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setClockI2C(0, 1000000)

# any of these calls are valid ways to engage the pull up resistors
binhoDevice.setPullUpStateI2C(0, 'EN')
binhoDevice.setPullUpStateI2C(0, 'ENABLE')
binhoDevice.setPullUpStateI2C(0, '1')
binhoDevice.setPullUpStateI2C(0, 'ON')

# any of these calls are valid ways to disengage the pull up resistors
binhoDevice.setPullUpStateI2C(0, 'DIS')
binhoDevice.setPullUpStateI2C(0, 'DISABLE')
binhoDevice.setPullUpStateI2C(0, '0')
binhoDevice.setPullUpStateI2C(0, 'OFF')
```

### getPullUpStateI2C(i2cIndex)

This function is used to get the status of the 2.2kOhm internal pull-up resistors on the SDA and SCL signals, as in whether they are currently engaged or disengaged.

**Inputs:**

This function takes one parameter:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.

**Outputs:**

The host adapter will respond with '-I2C0 PULL' followed by a space followed by the current state of the pull-up resistors, either 'ENABLED' or 'DISABLED'.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setClockI2C(0, 1000000)

binhoDevice.setPullUpStateI2C(0, 'EN')
print(binhoDevice.getPullUpStateI2C(0))
#-I2C0 PULL ENABLED

binhoDevice.setPullUpStateI2C(0, 'DIS')
print(binhoDevice.getPullUpStateI2C(0))
#-I2C0 PULL DISABLED
```

### scanAddrI2C(i2cIndex, address)

This function is used to scan a single address to determine if a slave device is present and responding.

#### Inputs:

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `address`, which is the address of the target peripheral device on the I2C bus.

#### Outputs:

The host adapter will respond with '-I2C0 SCAN' followed by a space followed by the address scanned, followed by the scan result, which will be either 'OK' or 'NG'.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setClockI2C(0, 1000000)
binhoDevice.setPullUpStateI2C(0, 'EN')

print(binhoDevice.scanAddrI2C(0, 0x42))
#-I2C0 SCAN 0x42 OK
```

### writeByteI2C(i2cIndex, data)

This function is used to write a single byte to a peripheral device on the I2C bus.

#### Inputs:

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `data`, which is the value to write to the target slave device.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setClockI2C(0, 1000000)
binhoDevice.setPullUpStateI2C(0, 'EN')

binhoDevice.startI2C(0, 0x42)
binhoDevice.writeByteI2C(0, 0xFA)
binhoDevice.endI2C(0)
```

### readByteI2C(i2cIndex, address)

This function is used to request a single byte from a peripheral device on the I2C bus.

#### Inputs:

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `address`, which is the address of the target peripheral device on the I2C bus.

#### Outputs:

The host adapter will respond with '-I2C0 RXD' followed by a space followed by the received byte. If no data is received or an error occurs during the read, the response will be 'NG'.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setClockI2C(0, 1000000)
binhoDevice.setPullUpStateI2C(0, 'EN')

print(binhoDevice.readByteI2C(0, 0x42))
#-I2C0 RXD 0xAB
```

### readBytesI2C(i2cIndex, address, numBytes)

This function is used to request data from a peripheral device on the I2C bus. The maximum number of bytes in a single request is 256.

#### Inputs:

This function takes three parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `address`, which is the address of the target peripheral device on the I2C bus.
* `numBytes`, which is the number of bytes to request from the peripheral device.

#### Outputs:

The host adapter will respond with '-I2C0 RXD' followed by a space followed by the received bytes. If no data is received or an error occurs during the read, the response will be 'NG'.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setClockI2C(0, 1000000)
binhoDevice.setPullUpStateI2C(0, 'EN')

print(binhoDevice.readBytesI2C(0, 0x42, 4))
#-I2C0 RXD 0xAB 0xAC 0xAD 0xAE
```

### writeFromBufferI2C(i2cIndex, numBytes)

This function is used to send data to a peripheral device on the I2C bus from the internal buffer.

#### Inputs:

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `numBytes`, which is the number of bytes to send from the internal buffer. The data from the buffer is sent beginning with index 0 in the buffer.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setClockI2C(0, 1000000)
binhoDevice.setPullUpStateI2C(0, 'EN')

binhoDevice.clearBuffer(0)
binhoDevice.addByteToBuffer(0, 12)
binhoDevice.addByteToBuffer(0, 136)
binhoDevice.addByteToBuffer(0, 0)
binhoDevice.addByteToBuffer(0, 255)

binhoDevice.writeFromBufferI2C(0, 4)
```

### readToBufferI2C(i2cIndex, address, numBytes)

This function is used to request data from a peripheral device on the I2C bus and receive it into BUF0. The maximum number of bytes in a single request is 256.

#### Inputs:

This function takes three parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `address`, which is the address of the target peripheral device on the I2C bus.
* `numBytes`, which is the number of bytes to request from the peripheral device.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setClockI2C(0, 1000000)
binhoDevice.setPullUpStateI2C(0, 'EN')

binhoDevice.readToBufferI2C(0, 0x42, 4)
print(binhoDevice.readBuffer(0, 4))
#BUF0 12 136 0 255
```

### startI2C(i2cIndex, address)

This function is used to start an I2C transmission to a target peripheral device on the I2C bus.

#### Inputs:

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `address`, which is the address of the target slave device on the I2C bus.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setClockI2C(0, 1000000)
binhoDevice.setPullUpStateI2C(0, 'EN')

binhoDevice.startI2C(0, 0x42)
binhoDevice.writeByteI2C(0, 0xFA)
binhoDevice.endI2C(0)
```

### endI2C(i2cIndex, repeat)

This function ends an I2C transmission. It can be used to send a stop bit or a repeated start bit.&#x20;

#### Inputs:

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `repeat`, which shall be set to True to send a repeated start bit, otherwise the transmission will be terminated with a stop bit.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setClockI2C(0, 1000000)
binhoDevice.setPullUpStateI2C(0, 'EN')

binhoDevice.startI2C(0, 0x42)
binhoDevice.writeByteI2C(0, 0xFA)
binhoDevice.endI2C(0)
```

### writeToReadFromI2C(i2cIndex, addr, stop, numRBytes, numWBytes, data)

This function performs a write of 0 to 1024 bytes followed by a read of 0 to 1024 bytes in a single transaction. This function minimizes the number of round-trips between the PC and Nova in order to maximize I2C throughput. This function is highly recommended for reading/writing I2C memory devices or in any other application where multibyte transactions are frequently used.&#x20;

#### Inputs:

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `addr`, which is the address of the target I2C peripheral device on the bus. Note the address must be formatted as 7bit address in hex, without the leading "0x".
* `stop`, which is used to indicate a stop bit should be sent at the end of the transaction. 0 = no stop bit sent, 1 = terminate transaction with stop bit.
* `numRBytes`, which is the number of bytes to read, from 0 to 1024.
* `numWBytes`, which is the number of bytes to write, from 0 to 1024.
* `data`, which is a string of hex characters that matches the length specified by numWBytes parameter.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command if numRBytes is 0. If numRBytes > 0, then the response will be 'I2C0 RXD' followed by the received data in Hex. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setClockI2C(0, 1000000)
binhoDevice.setPullUpStateI2C(0, 'EN')

#write 4 bytes, no read
binhoDevice.writeToReadFromI2C(0, 'A0', 1, 0, 4, 'DEADBEEF')

#write 2 bytes, read back 8 bytes
binhoDevice.writeToReadFromI2C(0, 'A0', 1, 8, 2, 'ABCD') 

```

### setSlaveAddressI2C(i2cIndex, address)

This function configures _Nova_ to behave as an I2C peripheral device assigned to the provided address.&#x20;

#### Inputs:

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `address`, which can be any valid 8-bit I2C peripheral device address.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setPullUpStateI2C(0, "EN")

binhoDevice.setSlaveAddressI2C(0, 0xA0)
```

### getSlaveAddressI2C(i2cIndex)

This function returns the address assigned to Nova while operating in I2C peripheral mode.&#x20;

#### Inputs:

This function takes one parameter:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with 'I2C0 SLAVE' followed by the configured device address upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setPullUpStateI2C(0, "EN")

binhoDevice.setSlaveAddressI2C(0, 0xA0)
print(binhoDevice.getSlaveAddressI2C(0))
```

### setSlaveModeI2C(i2cIndex, mode)

This function configures the behavior of the emulated I2C peripheral device. At this time, the Binho Nova I2C peripheral supports two modes of operation which allow it to behave like some of the most common I2C peripheral devices. Please see the description of the modes on the [I2C Slave Mode command documentation](https://support.binho.io/user-guide/ascii-interface/i2c-commands#slave-mode) page.&#x20;

#### Inputs:

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `mode`, which can be either _USEPTR_ or _STARTZERO_.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setPullUpStateI2C(0, "EN")

binhoDevice.setSlaveAddressI2C(0, 0xA0)
binhoDevice.setSlaveModeI2C(0, "USEPTR")
```

### getSlaveModeI2C(i2cIndex)

This function returns the configured mode of the emulated I2C peripheral device. At this time, the Binho Nova I2C peripheral supports two modes of operation which allow it to behave like some of the most common I2C peripheral devices. Please see the description of the modes on the [I2C Slave Mode command documentation](https://support.binho.io/user-guide/ascii-interface/i2c-commands#slave-mode) page.&#x20;

#### Inputs:

This function takes one parameter:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with 'I2C0 SLAVE MODE' followed by either _USEPTR_ or _STARTZERO _upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setPullUpStateI2C(0, "EN")

binhoDevice.setSlaveAddressI2C(0, 0xA0)
binhoDevice.setSlaveModeI2C(0, "USEPTR")
print(binhoDevice.getSlaveModeI2C(0))
```

### setSlaveRegisterCount(i2cIndex, registerCount)

This function configures the number of registers to emulate in the I2C peripheral device memory bank while operating in I2C peripheral mode.&#x20;

#### Inputs:

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `registerCount`, which can be any integer value from 1 to 256.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setPullUpStateI2C(0, "EN")

binhoDevice.setSlaveAddressI2C(0, 0xA0)
binhoDevice.setSlaveModeI2C(0, "USEPTR")

binhoDevice.setSlaveRegisterCount(0, 128)
```

### getSlaveRegisterCount(i2cIndex)

This function returns the number of registers being emulated in the I2C peripheral device memory bank while operating in I2C peripheral mode.&#x20;

#### Inputs:

This function takes one parameter:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with 'I2C0 SLAVE REGCNT' followed by the number of registers upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setPullUpStateI2C(0, "EN")

binhoDevice.setSlaveAddressI2C(0, 0xA0)
binhoDevice.setSlaveModeI2C(0, "USEPTR")

binhoDevice.setSlaveRegisterCount(0, 128)
print(binhoDevice.getSlaveRegisterCount(0))
```

### setSlaveRegister(i2cIndex, register, value)

This function sets the value stored in a given register in the emulated I2C peripheral device to the provided value while operating in I2C peripheral mode.&#x20;

#### Inputs:

This function takes three parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `register`, which can be any integer value from 0 to the number of registers configured in the device.
* `value`, which can be any integer value from 0 to 255

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setPullUpStateI2C(0, "EN")

binhoDevice.setSlaveAddressI2C(0, 0xA0)
binhoDevice.setSlaveModeI2C(0, "USEPTR")
binhoDevice.setSlaveRegisterCount(0, 128)

binhoDevice.setSlaveRegisterI2C(0, 0, 0xDE)
binhoDevice.setSlaveRegisterI2C(0, 1, 0xAD)
binhoDevice.setSlaveRegisterI2C(0, 2, 0xBE)
binhoDevice.setSlaveRegisterI2C(0, 3, 0xEF)
```

### getSlaveRegisterI2C(i2cIndex, register)

This function returns the value stored in a given register in the emulated I2C peripheral device while operating in I2C peripheral mode.&#x20;

#### Inputs:

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `register`, which can be any integer value from 0 to the number of registers configured in the device.

#### Outputs:

The host adapter will respond with 'I2C0 SLAVE REG' followed by the register number and the value stored in that register. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setPullUpStateI2C(0, "EN")

binhoDevice.setSlaveAddressI2C(0, 0xA0)
binhoDevice.setSlaveModeI2C(0, "USEPTR")
binhoDevice.setSlaveRegisterCount(0, 128)

binhoDevice.setSlaveRegisterI2C(0, 0, 0xDE)
binhoDevice.setSlaveRegisterI2C(0, 1, 0xAD)
binhoDevice.setSlaveRegisterI2C(0, 2, 0xBE)
binhoDevice.setSlaveRegisterI2C(0, 3, 0xEF)

print(binhoDevice.getSlaveRegisterI2C(0,2))
```

### setSlaveWriteMaskI2C(i2cIndex, register, mask)

This function sets the value of any register's writemask which is used to determine which bits in a register can be written to by an I2C controller on the bus.

#### Inputs:

This function takes three parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `register`, which can be any integer value from 0 to the number of registers configured in the device.
* `mask`, which can be any integer value from 0 to 255, where a 1 corresponds to granting write** **access to the corresponding bit in the register.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setPullUpStateI2C(0, "EN")

binhoDevice.setSlaveAddressI2C(0, 0xA0)
binhoDevice.setSlaveModeI2C(0, "USEPTR")
binhoDevice.setSlaveRegisterCount(0, 128)

binhoDevice.setSlaveRegisterI2C(0, 0, 0xDE)
binhoDevice.setSlaveRegisterI2C(0, 1, 0xAD)
binhoDevice.setSlaveRegisterI2C(0, 2, 0xBE)
binhoDevice.setSlaveRegisterI2C(0, 3, 0xEF)

# Only the lower byte of register 1 is writeable
binhoDevice.setSlaveWriteMaskI2C(0, 1, 0x0F)

# Make Registers 2 and 3 READ-ONLY
binhoDevice.setSlaveWriteMaskI2C(0, 2, 0x00)
binhoDevice.setSlaveWriteMaskI2C(0, 3, 0x00)
```

### getSlaveWriteMaskI2C(i2cIndex, register)

This function returns the value of any register's writemask which is used to determine which bits in a register can be written to by an I2C controller on the bus.

#### Inputs:

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `register`, which can be any integer value from 0 to the number of registers configured in the device.

#### Outputs:

The host adapter will respond with '-I2C0 SLAVE WRITEMASK' followed by the mask value upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setPullUpStateI2C(0, "EN")

binhoDevice.setSlaveAddressI2C(0, 0xA0)
binhoDevice.setSlaveModeI2C(0, "USEPTR")
binhoDevice.setSlaveRegisterCount(0, 128)

binhoDevice.setSlaveRegisterI2C(0, 0, 0xDE)
binhoDevice.setSlaveRegisterI2C(0, 1, 0xAD)
binhoDevice.setSlaveRegisterI2C(0, 2, 0xBE)
binhoDevice.setSlaveRegisterI2C(0, 3, 0xEF)

# Only the lower byte of register 1 is writeable
binhoDevice.setSlaveWriteMaskI2C(0, 1, 0x0F)
print(binhoDevice.getSlaveWriteMaskI2C(0, 1))
```

### setSlaveReadMaskI2C(i2cIndex, register, mask)

This function sets the value of any register's readmask which is used to determine which bits in a register can be read from by an I2C controller on the bus. This can be used to emulate _strobe _bits in a register.

#### Inputs:

This function takes three parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `register`, which can be any integer value from 0 to the number of registers configured in the device.
* `mask`, which can be any integer value from 0 to 255, where a 1 corresponds to granting read access to the corresponding bit in the register.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setPullUpStateI2C(0, "EN")

binhoDevice.setSlaveAddressI2C(0, 0xA0)
binhoDevice.setSlaveModeI2C(0, "USEPTR")
binhoDevice.setSlaveRegisterCount(0, 128)

binhoDevice.setSlaveRegisterI2C(0, 0, 0xDE)
binhoDevice.setSlaveRegisterI2C(0, 1, 0xAD)
binhoDevice.setSlaveRegisterI2C(0, 2, 0xBE)
binhoDevice.setSlaveRegisterI2C(0, 3, 0xEF)

# Only the lower byte of register 1 is readable
binhoDevice.setSlaveReadMaskI2C(0, 1, 0x0F)
```

### getSlaveReadMaskI2C(i2cIndex, register)

This function returns the value of any register's readmask which is used to determine which bits in a register can be read from by an I2C controller on the bus.

#### Inputs:

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `register`, which can be any integer value from 0 to the number of registers configured in the device.

#### Outputs:

The host adapter will respond with '-I2C0 SLAVE READMASK' followed by the mask value upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'I2C')
binhoDevice.setPullUpStateI2C(0, "EN")

binhoDevice.setSlaveAddressI2C(0, 0xA0)
binhoDevice.setSlaveModeI2C(0, "USEPTR")
binhoDevice.setSlaveRegisterCount(0, 128)

binhoDevice.setSlaveRegisterI2C(0, 0, 0xDE)
binhoDevice.setSlaveRegisterI2C(0, 1, 0xAD)
binhoDevice.setSlaveRegisterI2C(0, 2, 0xBE)
binhoDevice.setSlaveRegisterI2C(0, 3, 0xEF)

# Only the lower byte of register 1 is readable
binhoDevice.setSlaveReadMaskI2C(0, 1, 0x0F)
print(binhoDevice.getSlaveReadMaskI2C(0,1))
```
