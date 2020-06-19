# SWI Functions

### beginSWI\(swiIndex, pin, pullup\)

The function starts the Atmel SWI host on the given IO pin. The SWI protocol can be used on any of the IO pins, however it is especially convenient to use it on IO0 and IO2 as the internal pull-up resistor can be used thus eliminating the need for an external pull-up resistor.

#### Inputs:

This function takes three parameters:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.
* `pin`, which can be any of the IO pins 0 through 4.
* `pullup`, which can be either `True` or `False`. A value of True will use the internal pullup resistor when `pin` is set to 0 or 2.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)
```

### sendTokenSWI\(swiIndex, token\)

This function is used to send either a WAKE, ONE, or ZERO token, as defined by the Atmel SWI protocol specification. Note that due to the timing constraints of the protocol, the delay between communication with the host adapter and the PC may prohibit the construction of full data packets by stringing together a series of ZERO and ONE tokens with this command. For actual data packet transmission, use the PACKET command.

#### Inputs:

This function takes two parameters:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.
* `token`, which can be `WAKE`, `ZERO`, or `ONE`.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)

binhoDevice.sendTokenSWI(0, 'WAKE')
```

### sendFlagSWI\(swiIndex, flag\)

This function transmits a flag as defined by the Atmel SWI protocol. Note that this command is simply using the TX command below to send the predefined byte value of each of the four flags.

#### Inputs:

This function takes two parameters:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.
* `flag`, which can be `COMMAND`, `TRANSMIT`, `IDLE`, or `SLEEP`.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)
binhoDevice.sendTokenSWI(0, 'WAKE')

binhoDevice.sendFlagSWI(0, 'IDLE')
```

### sendCommandFlagSWI\(swiIndex\)

This function transmits a `COMMAND` flag.

#### Inputs:

This function takes one parameter:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)
binhoDevice.sendTokenSWI(0, 'WAKE')

binhoDevice.sendCommandFlagSWI(0)
```

### sendTransmitFlagSWI\(swiIndex\)

This function transmits a `TRANSMIT` flag.

#### Inputs:

This function takes one parameter:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)
binhoDevice.sendTokenSWI(0, 'WAKE')

binhoDevice.sendTransmitFlagSWI(0)
```

### sendIdleFlagSWI\(swiIndex\)

This function transmits an `IDLE` flag.

#### Inputs:

This function takes one parameter:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)
binhoDevice.sendTokenSWI(0, 'WAKE')

binhoDevice.sendIdleFlagSWI(0)
```

### sendSleepFlagSWI\(swiIndex\)

This function transmits a `SLEEP` flag.

#### Inputs:

This function takes one parameter:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)
binhoDevice.sendTokenSWI(0, 'WAKE')

binhoDevice.sendSleepFlagSWI(0)
```

### transmitByteSWI\(swiIndex, data\)

This function transmits a byte of data as defined by the Atmel SWI protocol.

#### Inputs:

This function takes two parameters:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.
* `data`, which is the byte of data to be transmitted.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)
binhoDevice.sendTokenSWI(0, 'WAKE')

binhoDevice.transmitByteSWI(0, 0xAA)
```

### receiveBytesSWI\(swiIndex, count\)

This function receives n bytes of data as defined by the Atmel SWI protocol.

#### Inputs:

This function takes two parameters:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.
* `data`, which is the byte of data to be transmitted.

#### Outputs:

The host adapter will respond with 'SWI0' followed by 'RXD' followed by n bytes of received data.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)
binhoDevice.sendTokenSWI(0, 'WAKE')

print(binhoDevice.receiveBytesSWI(0, 5))
#-SWI0 RXD 0x0A 0x0B 0x0C 0x0D 0x0E
```

### setPacketOpCodeSWI\(swiIndex, opCode\)

This function is used to set the opcode of an SWI packet.

#### Inputs:

This function takes two parameters:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.
* `opCode`, which is the byte of data to be transmitted.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)
binhoDevice.sendTokenSWI(0, 'WAKE')

binhoDevice.setPacketOpCodeSWI(0, 0x02)
```

### setPacketParam1SWI\(swiIndex, value\)

This function is used to set the value of the Param1 byte of the SWI Packet.

#### Inputs:

This function takes two parameters:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.
* `value`, which is the byte of data to be transmitted in the Param1 field of the packet.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)
binhoDevice.sendTokenSWI(0, 'WAKE')

binhoDevice.setPacketOpCodeSWI(0, 0x02)
binhoDevice.setPacketParam1SWI(0, 0x00)
```

### setPacketParam2SWI\(swiIndex, value\)

This function is used to set the 16bit value of the Param2 field of the SWI Packet.

#### Inputs:

This function takes two parameters:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.
* `data`, which is the 16bit value to be transmitted in the Param2 field of the packet.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)
binhoDevice.sendTokenSWI(0, 'WAKE')

binhoDevice.setPacketOpCodeSWI(0, 0x02)
binhoDevice.setPacketParam1SWI(0, 0x00)
binhoDevice.setPacketParam2SWI(0, 0x0003)
```

### setPacketDataSWI\(swiIndex, index, value\)

This function is used to set the bytes of the Data field one by one.

#### Inputs:

This function takes three parameters:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.
* `index`, which is the byte index to load the value into the Data field.
* `value`, which is the byte of data loaded into the given index of the Data field.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)
binhoDevice.sendTokenSWI(0, 'WAKE')

binhoDevice.setPacketOpCodeSWI(0, 0x02)
binhoDevice.setPacketParam1SWI(0, 0x00)
binhoDevice.setPacketParam2SWI(0, 0x0003)

binhoDevice.setPacketDataSWI(0, 0, 0x00)
binhoDevice.setPacketDataSWI(0, 1, 0x11)
binhoDevice.setPacketDataSWI(0, 2, 0x22)
binhoDevice.setPacketDataSWI(0, 3, 0x33)
binhoDevice.setPacketDataSWI(0, 4, 0x44)
binhoDevice.setPacketDataSWI(0, 5, 0x55)
binhoDevice.setPacketDataSWI(0, 6, 0x66)
binhoDevice.setPacketDataSWI(0, 7, 0x77)
```

### setPacketDataFromBufferSWI\(swiIndex, byteCount, bufferName\)

This function is used to set the Data field of the SWI packet to the contents from a buffer.

#### Inputs:

This function takes three parameters:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.
* `byteCount`, which is the number of bytes to load from the buffer.
* `bufferName`, which is the name of the buffer to use, typically BUF0.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)
binhoDevice.sendTokenSWI(0, 'WAKE')

binhoDevice.setPacketOpCodeSWI(0, 0x02)
binhoDevice.setPacketParam1SWI(0, 0x00)
binhoDevice.setPacketParam2SWI(0, 0x0003)

data = [0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88, 0x99, 0xAA, 0xBB, 0xCC, 0xDD, 0xEE, 0xFF, 0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88, 0x99, 0xAA, 0xBB, 0xCC, 0xDD, 0xEE, 0xFF]
binhoDevice.writeToBuffer(0, 0, data)
binhoDevice.setPacketDataFromBufferSWI(swiIndex, 32, 'BUF0')

binhoDevice.sendPacketSWI(0)
```

### sendPacketSWI\(swiIndex\)

This function sends the the constructed SWI packet.

#### Inputs:

This function takes one parameter:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)
binhoDevice.sendTokenSWI(0, 'WAKE')

binhoDevice.setPacketOpCodeSWI(0, 0x02)
binhoDevice.setPacketParam1SWI(0, 0x00)
binhoDevice.setPacketParam2SWI(0, 0x0003)

binhoDevice.setPacketDataSWI(0, 0, 0x00)
binhoDevice.setPacketDataSWI(0, 1, 0x11)
binhoDevice.setPacketDataSWI(0, 2, 0x22)
binhoDevice.setPacketDataSWI(0, 3, 0x33)
binhoDevice.setPacketDataSWI(0, 4, 0x44)
binhoDevice.setPacketDataSWI(0, 5, 0x55)
binhoDevice.setPacketDataSWI(0, 6, 0x66)
binhoDevice.setPacketDataSWI(0, 7, 0x77)

binhoDevice.sendPacketSWI(0)
```

### clearPacketSWI\(swiIndex\)

This function clears the OpCode, Param1, Param2, and Data fields of the SWI Packet.

#### Inputs:

This function takes one parameter:

* `swiIndex`, which is always 0 on the _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SWI')
binhoDevice.beginSWI(0, 0, True)
binhoDevice.sendTokenSWI(0, 'WAKE')

binhoDevice.setPacketOpCodeSWI(0, 0x02)
binhoDevice.setPacketParam1SWI(0, 0x00)
binhoDevice.setPacketParam2SWI(0, 0x0003)

binhoDevice.setPacketDataSWI(0, 0, 0x00)
binhoDevice.setPacketDataSWI(0, 1, 0x11)
binhoDevice.setPacketDataSWI(0, 2, 0x22)
binhoDevice.setPacketDataSWI(0, 3, 0x33)
binhoDevice.setPacketDataSWI(0, 4, 0x44)
binhoDevice.setPacketDataSWI(0, 5, 0x55)
binhoDevice.setPacketDataSWI(0, 6, 0x66)
binhoDevice.setPacketDataSWI(0, 7, 0x77)

binhoDevice.sendPacketSWI(0)

binhoDevice.clearPacketSWI(0)
```

