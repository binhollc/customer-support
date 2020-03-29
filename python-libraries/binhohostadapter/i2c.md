# I2C Functions

### setClockI2C\(i2cIndex, frequency\)

This function sets the clock frequency of the I2C bus. This binho Multi-Protocol USB Host Adapter supports I2C bus speeds from 100kHz to 4MHz. Per the I2C specification, the common clock frequencies are 100kHz, 400kHz, and 1MHz, although it's possible to run the bus at other frequencies.

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

### getClockI2C\(i2cIndex\)

This function gets the clock frequency of the I2C bus. This binho Multi-Protocol USB Host Adapter supports I2C bus speeds from 100kHz to 4MHz. Per the I2C specification, the common clock frequencies are 100kHz, 400kHz, and 1MHz, although it's possible to run the bus at other frequencies.

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

### setPullUpStateI2C\(i2cIndex, state\)

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

### getPullUpStateI2C\(i2cIndex\)

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

### scanAddrI2C\(i2cIndex, address\)

This function is used to scan a single address to determine if a slave device is present and responding.

#### Inputs:

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `address`, which is the address of the target slave device on the I2C bus.

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

### writeByteI2C\(i2cIndex, data\)

This function is used to write a single byte to a slave device on the I2C bus.

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

### readByteI2C\(i2cIndex, address\)

This function is used to request a single byte from a slave device on the I2C bus.

#### Inputs:

This function takes two parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `address`, which is the address of the target slave device on the I2C bus.

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

### readBytesI2C\(i2cIndex, address, numBytes\)

This function is used to request data from a slave device on the I2C bus. The maximum number of bytes in a single request is 256.

#### Inputs:

This function takes three parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `address`, which is the address of the target slave device on the I2C bus.
* `numBytes`, which is the number of bytes to request from the slave device.

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

### writeFromBufferI2C\(i2cIndex, numBytes\)

This function is used to send data to a slave device on the I2C bus from the internal buffer.

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

### readToBufferI2C\(i2cIndex, address, numBytes\)

This function is used to request data from a slave device on the I2C bus and receive it into BUF0. The maximum number of bytes in a single request is 256.

#### Inputs:

This function takes three parameters:

* `i2cIndex`, which is always 0 on _Binho Nova_ host adapter.
* `address`, which is the address of the target slave device on the I2C bus.
* `numBytes`, which is the number of bytes to request from the slave device.

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

### startI2C\(i2cIndex, address\)

This function is used to start an I2C transmission to a target slave device on the I2C bus.

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

### endI2C\(i2cIndex, repeat\)

This function ends an I2C transmission. It can be used to send a stop bit or a repeated start bit. 

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

