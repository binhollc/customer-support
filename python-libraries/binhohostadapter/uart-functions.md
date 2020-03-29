# UART Functions

{% hint style="warning" %}
The UART functionality is different than the other supported protocols, as it operates as a bridge, rather than transmitting commands only when functions are called. More information about this can be found in the [User Guide](https://support.binho.io/user-guide/using-the-device/using-uart).
{% endhint %}

### setBaudRateUART\(uartIndex, baud\)

This function sets the baudrate of the UART bridge. The default baudrate is 9600bps.

#### Inputs:

This function takes two parameters:

* `uartIndex`, which is always 0 on _Binho Nova_ host adapter.
* `baud`, which can be set from 300 to 1000000bps.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'UART')
binhoDevice.setBaudRateUART(0, 115200)
```

### getBaudRateUART\(uartIndex\)

This function gets the baudrate of the UART bridge.

#### Inputs:

This function takes one parameter:

* `uartIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with 'UART0' followed by 'BAUD' followed by the configured baudrate in bps.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'UART')
binhoDevice.setBaudRateUART(0, 115200)
binhoDevice.getBaudRateUART(0)
```

### setDataBitsUART\(uartIndex, databits\)

This function sets the number of databits for the UART connection. The default databits setting is 8 bits.

#### Inputs:

This function takes two parameters:

* `uartIndex`, which is always 0 on _Binho Nova_ host adapter.
* `databits`, which can be set from 5 to 8 bits.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'UART')
binhoDevice.setBaudRateUART(0, 115200)
binhoDevice.setDataBitsUART(0, 8)
```

### getDataBitsUART\(uartIndex\)

This function gets the number of databits for the UART connection. 

#### Inputs:

This function takes one parameter:

* `uartIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with 'UART0' followed by 'DATABITS' followed by the configured number of bits.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'UART')
binhoDevice.setBaudRateUART(0, 115200)
binhoDevice.setDataBitsUART(0, 8)
binhoDevice.getDataBitsUART(0)
```

### setParityUART\(uartIndex, parity\)

This function sets the Parity bit configuration for the UART connection. The default setting is None.

#### Inputs:

This function takes two parameters:

* `uartIndex`, which is always 0 on _Binho Nova_ host adapter.
* `parity`, which can be set to `NONE`, `ODD`, or `EVEN`.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'UART')
binhoDevice.setBaudRateUART(0, 115200)
binhoDevice.setDataBitsUART(0, 8)
binhoDevice.setParityUART(0, 'NONE')
```

### getParityUART\(uartIndex\)

This function gets the Parity bit configuration for the UART connection.

#### Inputs:

This function takes one parameter:

* `uartIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with 'UART0' followed by 'PARITY' followed by the currently configured parity setting.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'UART')
binhoDevice.setBaudRateUART(0, 115200)
binhoDevice.setDataBitsUART(0, 8)
binhoDevice.setParityUART(0, 'NONE')

binhoDevice.getParityUART(0)
```

### setStopBitsUART\(uartIndex, stopbits\)

This function sets the Stop bit configuration for the UART connection. The default number of stop bits is 1.

#### Inputs:

This function takes two parameters:

* `uartIndex`, which is always 0 on _Binho Nova_ host adapter.
* `stopbits`, which can be set to either `1` or `2`.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'UART')
binhoDevice.setBaudRateUART(0, 115200)
binhoDevice.setDataBitsUART(0, 8)
binhoDevice.setParityUART(0, 'NONE')

binhoDevice.setStopBitsUART(0, 1)
```

### getStopBitsUART\(uartIndex\)

This function sets the Stop bit configuration for the UART connection.

#### Inputs:

This function takes one parameter:

* `uartIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with 'UART0' followed by 'STOPBITS' followed by the current number of stop bits.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'UART')
binhoDevice.setBaudRateUART(0, 115200)
binhoDevice.setDataBitsUART(0, 8)
binhoDevice.setParityUART(0, 'NONE')
binhoDevice.setStopBitsUART(0, 1)

binhoDevice.getStopBitsUART(0)
```

### setEscapeSequenceUART\(uartIndex, escape\)

This function sets the escape sequence that can be used to break out of the UART bridge mode. The default escape sequence is `+++UART0`. 

#### Inputs:

This function takes two parameters:

* `uartIndex`, which is always 0 on _Binho Nova_ host adapter.
* `escape`, which must be a string of 6 to 16 characters.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'UART')
binhoDevice.setBaudRateUART(0, 115200)
binhoDevice.setDataBitsUART(0, 8)
binhoDevice.setParityUART(0, 'NONE')
binhoDevice.setStopBitsUART(0, 1)

binhoDevice.setEscapeSequenceUART(0, '++ESCAPE++')
```

### getEscapeSequenceUART\(uartIndex\)

This function gets the currently configured escape sequence which can be used to close the UART bridge once it's been opened.

#### Inputs:

This function takes one parameter:

* `uartIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with 'UART0' followed by 'ESC' followed by the currently configured escape sequence.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'UART')
binhoDevice.setBaudRateUART(0, 115200)
binhoDevice.setDataBitsUART(0, 8)
binhoDevice.setParityUART(0, 'NONE')
binhoDevice.setStopBitsUART(0, 1)
binhoDevice.setEscapeSequenceUART(0, '++ESCAPE++')

binhoDevice.getEscapeSequenceUART(0)
```

### beginBridgeUART\(uartIndex\)

This function opens up the UART bridge. Note that once the bridge is open, only reading and writing UART commands can be used until the UART bridge is closed.

#### Inputs:

This function takes one parameter:

* `uartIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'UART')
binhoDevice.setBaudRateUART(0, 115200)
binhoDevice.setDataBitsUART(0, 8)
binhoDevice.setParityUART(0, 'NONE')
binhoDevice.setStopBitsUART(0, 1)

binhoDevice.beginBridgeUART(0)

```

### stopBridgeUART\(sequence\)

This function closes an opened UART bridge. 

#### Inputs:

This function takes one parameter:

* `sequence`, which is the 6 to 16 character long escape sequence.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'UART')
binhoDevice.setBaudRateUART(0, 115200)
binhoDevice.setDataBitsUART(0, 8)
binhoDevice.setParityUART(0, 'NONE')
binhoDevice.setStopBitsUART(0, 1)

binhoDevice.beginBridgeUART(0)

binhoDevice.stopBridgeUART('+++UART0')
```

### writeBridgeUART\(data\)

This function is used to transmit data over an open UART bridge. Note that this command does not directly communicate with the Binho Nova host adapter. Instead, it's writing the data to a transmit buffer within the python library. The python library will then transmit the data placed in the buffer.

#### Inputs:

This function takes one parameter:

* `data`, which is the data to be written to the UART bus.

#### Outputs:

This function does not return any data.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'UART')
binhoDevice.setBaudRateUART(0, 115200)
binhoDevice.setDataBitsUART(0, 8)
binhoDevice.setParityUART(0, 'NONE')
binhoDevice.setStopBitsUART(0, 1)

binhoDevice.beginBridgeUART(0)

binhoDevice.writeBridgeUART('Testing...')
binhoDevice.writeBridgeUART('more testing...')

binhoDevice.stopBridgeUART('+++UART0')
```

### readBridgeUART\(\)

This function is used to read data over an open UART bridge. Note that this command does not directly communicate with the Binho Nova host adapter. Instead, it's reading from the received data buffer within the python library.

#### Inputs:

This function takes no parameters.

#### Outputs:

This function returns any data that has been received over the UART bridge.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'UART')
binhoDevice.setBaudRateUART(0, 115200)
binhoDevice.setDataBitsUART(0, 8)
binhoDevice.setParityUART(0, 'NONE')
binhoDevice.setStopBitsUART(0, 1)

binhoDevice.beginBridgeUART(0)

binhoDevice.writeBridgeUART('Testing...')
binhoDevice.writeBridgeUART('more testing...')

print(binhoDevice.readBridgeUART())

binhoDevice.stopBridgeUART('+++UART0')
```

