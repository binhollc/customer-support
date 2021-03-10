# 1-WIRE Functions

{% hint style="success" %}
We highly encourage everyone to use our[ new Python package](https://support.binho.io/python-libraries/binho-python-package) which is packed with features. This library is still supported, but is not recommended for new design.
{% endhint %}

### begin1WIRE\(oneWireIndex, pin, pullup\)

This function starts the 1-WIRE host on the given IO pin.  The 1-Wire protocol can be used on any of the IO pins, however it is especially convenient to use it on IO0 and IO2 as the internal pull-up resistor can be used thus eliminating the need for an external pull-up resistor.

#### Inputs:

This function takes three parameters:

* `oneWireIndex`, which is always 0 on the _Binho Nova_ host adapter.
* `pin`, which can be any of the IO pins 0 through 4.
* `pullup`, which can be either `True` or `False`. A value of True will use the internal pullup resistor when `pin` is set to 0 or 2.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
binhoDevice.setOperationMode(0, '1WIRE')
binhoDevice.begin1WIRE(0, 0, True)
```

### reset1WIRE\(oneWireIndex\)

This function instructions the 1-Wire host to send the reset command. Sending the reset command is the first step of selecting a target device to communicate with.

#### Inputs:

This function takes one parameter:

* `oneWireIndexIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '-OK' if the device on the 1-Wire bus asserted a presence pulse as a result of the result on the 1-WIRE bus. If the command fails or no device asserts a pulse after the reset, the function will return '-NG'.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
binhoDevice.setOperationMode(0, '1WIRE')
binhoDevice.begin1WIRE(0, 0, True)

binhoDevice.reset1WIRE(0)
```

### writeByte1WIRE\(oneWireIndex, data, powered = False\)

This function sends one byte of data on the 1-WIRE bus.

#### Inputs:

This function takes two parameters:

* `oneWireIndexIndex`, which is always 0 on _Binho Nova_ host adapter.
* `powered`, \[optional\], which will continue to leave the bus powered upon completion of the write if set to `True`.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
binhoDevice.setOperationMode(0, '1WIRE')
binhoDevice.begin1WIRE(0, 0, True)
binhoDevice.reset1WIRE(0)
binhoDevice.resetSearch1WIRE(0)
binhoDevice.search1WIRE(0, True)
binhoDevice.reset1WIRE(0)
binhoDevice.select1WIRE(0)

binhoDevice.writeByte1WIRE(0, 0xAB)
```

### readByte1WIRE\(oneWireIndex\)

This function reads one byte of data from the 1-WIRE bus.

#### Inputs:

This function takes one parameter:

* `oneWireIndexIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '1WIRE0' followed by 'READ' followed by one byte of data read from the bus.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
binhoDevice.setOperationMode(0, '1WIRE')
binhoDevice.begin1WIRE(0, 0, True)
binhoDevice.reset1WIRE(0)
binhoDevice.resetSearch1WIRE(0)
binhoDevice.search1WIRE(0, True)
binhoDevice.reset1WIRE(0)
binhoDevice.select1WIRE(0)

binhoDevice.readByte1WIRE(0)
```

### select1WIRE\(oneWireIndex\)

This function selects the device whose address is currently in the internal address buffer to be the target for communication. The address in the internal address buffer is set by discovering the device via the search process.

#### Inputs:

This function takes one parameter:

* `oneWireIndexIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
binhoDevice.setOperationMode(0, '1WIRE')
binhoDevice.begin1WIRE(0, 0, True)
binhoDevice.reset1WIRE(0)
binhoDevice.resetSearch1WIRE(0)
binhoDevice.search1WIRE(0, True)

binhoDevice.reset1WIRE(0)
binhoDevice.select1WIRE(0)
```

### skip1WIRE\(oneWireIndex\)

This function allows one to skip the search process and enables communication with the device right away. This can only be used when there is only one 1-Wire device on the bus.

#### Inputs:

This function takes one parameter:

* `oneWireIndexIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
binhoDevice.setOperationMode(0, '1WIRE')
binhoDevice.begin1WIRE(0, 0, True)
binhoDevice.reset1WIRE(0)

binhoDevice.skip1WIRE(0)
```

### depower1WIRE\(oneWireIndex\)

This function is used to power down the 1-WIRE bus.

#### Inputs:

This function takes one parameter:

* `oneWireIndexIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
binhoDevice.setOperationMode(0, '1WIRE')
binhoDevice.begin1WIRE(0, 0, True)
binhoDevice.reset1WIRE(0)

binhoDevice.depower1WIRE(0)
```

### getAddress1WIRE\(oneWireIndex\)

This function returns the address that was found by performing a search.

#### Inputs:

This function takes one parameter:

* `oneWireIndexIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '1WIRE0' followed by 'ADDR' followed by the 8-byte long address of the device discovered by the search.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
binhoDevice.setOperationMode(0, '1WIRE')
binhoDevice.begin1WIRE(0, 0, True)
binhoDevice.reset1WIRE(0)
binhoDevice.resetSearch1WIRE(0)
binhoDevice.search1WIRE(0, True)

binhoDevice.getAddress1WIRE(0)
```

### search1WIRE\(oneWireIndex, normalSearch = True\)

This function begins the process of searching for devices on the 1-WIRE bus. The address of the found device will be stored in internal address buffer.

#### Inputs:

This function takes up to two parameters:

* `oneWireIndexIndex`, which is always 0 on _Binho Nova_ host adapter.
* `normalSearch`, \[optional\], which can be `True` or `False`. In the case of False, an Alarm / Conditional search will be performed, which only returns devices in some sort of alarm state. See this [Application Note](https://www.maximintegrated.com/en/design/technical-documents/app-notes/1/187.html) for additional information on this topic.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
binhoDevice.setOperationMode(0, '1WIRE')
binhoDevice.begin1WIRE(0, 0, True)
binhoDevice.reset1WIRE(0)
binhoDevice.resetSearch1WIRE(0)
binhoDevice.search1WIRE(0, True)
```

### resetSearch1WIRE\(oneWireIndex\)

This function resets the search so that a new search can be started from the beginning.

#### Inputs:

This function takes one parameter:

* `oneWireIndexIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
binhoDevice.setOperationMode(0, '1WIRE')
binhoDevice.begin1WIRE(0, 0, True)
binhoDevice.reset1WIRE(0)
binhoDevice.resetSearch1WIRE(0)
```

### targetSearch1WIRE\(oneWireIndex, target\)

This function searches the 1-WIRE bus for devices belonging to the same family code as the target.

#### Inputs:

This function takes two parameters:

* `oneWireIndexIndex`, which is always 0 on _Binho Nova_ host adapter.
* `target`, which is a 1-byte long device family code for which to search.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
binhoDevice.setOperationMode(0, '1WIRE')
binhoDevice.begin1WIRE(0, 0, True)
binhoDevice.reset1WIRE(0)
binhoDevice.resetSearch1WIRE(0)
binhoDevice.targetSearch1WIRE(0, 0xAA)
```

