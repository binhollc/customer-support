# SPI Functions

{% hint style="success" %}
We highly encourage everyone to use our[ new Python package](https://support.binho.io/python-libraries/binho-python-package) which is packed with features. This library is still supported, but is not recommended for new design.
{% endhint %}

### setClockSPI(spiIndex, clock)

This function sets the clock frequency of the SPI bus. The default clock frequency is 2MHz.

#### Inputs:

This function takes two parameters:

* `spiIndex`, which is always 0 on _Binho Nova_ host adapter.
* `clock`, which is the desired frequency from 500000 Hz to 12000000 Hz in 1000 Hz steps.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SPI')
binhoDevice.setClockSPI(0, 4000000)
```

### getClockSPI(spiIndex)

This function gets the currently configured clock frequency of the SPI bus.

#### Inputs:

This function takes one parameter:

* `spiIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with 'SPI0' followed by 'CLK' followed by the configured clock frequency in Hertz.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SPI')
binhoDevice.setClockSPI(0, 4000000)

binhoDevice.getClockSPI(0)
```

### setOrderSPI(spiIndex, order)

This function sets the SPI bus bit order. The bit order can be configured to either LSB first or MSB first. The default bit order setting is MSB first.

#### Inputs:

This function takes two parameters:

* `spiIndex`, which is always 0 on _Binho Nova_ host adapter.
* `order`, which is the desired bit order, can be either `LSBFIRST` or `MSBFIRST`.

#### &#x20;Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SPI')
binhoDevice.setClockSPI(0, 4000000)
binhoDevice.setOrderSPI(0, 'MSBFIRST')
```

### getOrderSPI(spiIndex)

This function gets the currently configured SPI bus bit order.

#### Inputs:

This function takes one parameter:

* `spiIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with 'SPI0' followed by 'ORDER' followed by the currently configured bit order.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SPI')
binhoDevice.setClockSPI(0, 4000000)
binhoDevice.setOrderSPI(0, 'MSBFIRST')

binhoDevice.getOrderSPI(0)
```

### setModeSPI(spiIndex, mode)

This function sets the SPI bus mode of operation. SPI Modes 0, 1, 2, or 3 are determined by CPOL and CPHA. It's possible to set the mode directly, or to configure CPOL and CPHA independently to determine the mode setting. The default mode is 0.

#### Inputs:

This function takes two parameters:

* `spiIndex`, which is always 0 on _Binho Nova_ host adapter.
* `mode`, which is the desired mode: 0, 1, 2, or 3.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SPI')
binhoDevice.setClockSPI(0, 4000000)
binhoDevice.setOrderSPI(0, 'MSBFIRST')

binhoDevice.setModeSPI(0, 0)
```

### getModeSPI(spiIndex)

This function gets the currently configured SPI bus mode.

#### Inputs:

This function takes one parameter:

* `spiIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with 'SPI0' followed by 'MODE' followed by the currently configured mode of operation.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SPI')
binhoDevice.setClockSPI(0, 4000000)
binhoDevice.setOrderSPI(0, 'MSBFIRST')

binhoDevice.setModeSPI(0, 0)
binhoDevice.getModeSPI(0)
```

### getCpolSPI(spiIndex)

This function gets the current SPI bus Clock Polarity. Clock polarity indicates the idle condition of the clock signal. This is related to the SPI mode. It's possible to configure the CPOL setting directly by using this command, or indirectly by using the MODE command. The default value of CPOL is 0.

#### Inputs:

This function takes one parameter:

* `spiIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with 'SPI0' followed by 'CPOL' followed by the currently configured clock polarity value.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SPI')
binhoDevice.setClockSPI(0, 4000000)
binhoDevice.setOrderSPI(0, 'MSBFIRST')

binhoDevice.setModeSPI(0, 0)
binhoDevice.getCpolSPI(0)
```

### getCphaSPI(spiIndex)

This function gets the current SPI bus Clock Phase. Clock Phase indicates when the data is valid in relation to the clock edges. This is related to the SPI mode. It's possible to configure the CPHA setting directly by using this command, or indirectly by using the MODE command. The default value of CPHA is 0.

#### Inputs:

This function takes one parameter:

* `spiIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with 'SPI0' followed by 'CPHA' followed by the currently configured clock phase value.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SPI')
binhoDevice.setClockSPI(0, 4000000)
binhoDevice.setOrderSPI(0, 'MSBFIRST')

binhoDevice.setModeSPI(0, 0)
binhoDevice.getCphaSPI(0)
```

### setBitsPerTransferSPI(spiIndex, bits)

This function sets the number of bits per transmission. The SPI bus can be configured to either 8 or 16 bits per transfer. The default setting is 8 bits per transfer.

#### Inputs:

This function takes two parameters:

* `spiIndex`, which is always 0 on _Binho Nova_ host adapter.
* `bits`, which is the number of bits per transfer and can be either 8 or 16.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SPI')
binhoDevice.setClockSPI(0, 4000000)
binhoDevice.setOrderSPI(0, 'MSBFIRST')
binhoDevice.setModeSPI(0, 0)

binhoDevice.setBitsPerTransferSPI(0, 8)
```

### getBitsPerTransferSPI(spiIndex)

This function gets the number of bits per transmission.

#### Inputs:

This function takes one parameter:

* `spiIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with 'SPI0' followed by 'TXBITS' followed by the currently configured number of bits per transfer.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SPI')
binhoDevice.setClockSPI(0, 4000000)
binhoDevice.setOrderSPI(0, 'MSBFIRST')
binhoDevice.setModeSPI(0, 0)

binhoDevice.setBitsPerTransferSPI(0, 8)
binhoDevice.getBitsPerTransferSPI(0)
```

### beginSPI(spiIndex)

This function starts the SPI Controller. This command must be issued before sending/receiving any data on the SPI bus.

#### Inputs:

This function takes one parameter:

* `spiIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SPI')
binhoDevice.setClockSPI(0, 4000000)
binhoDevice.setOrderSPI(0, 'MSBFIRST')
binhoDevice.setModeSPI(0, 0)
binhoDevice.setBitsPerTransferSPI(0, 8)

#configure the CS pin
binhoDevice.setIOpinMode(0, 'DOUT')
binhoDevice.setIOpinValue(0, 'HIGH')

binhoDevice.beginSPI(0)
```

### transferSPI(spiIndex, data)

This function is used to send and receive data on the SPI bus.&#x20;

{% hint style="warning" %}
Before sending the **TXRX** command, be sure you've selected the target device on your SPI bus. The CS pin of the target device should be driven to the correct logical value using the [IO commands](https://support.binho.io/user-guide/ascii-interface/io-commands).
{% endhint %}

#### Inputs:

This function takes two parameters:

* `spiIndex`, which is always 0 on _Binho Nova_ host adapter.
* `bits`, which is the number of bits per transfer and can be either 8 or 16.

#### Outputs:

The host adapter will respond with 'SPI0' followed by 'RXD' followed by the byte of data received from the SPI peripheral device during the transfer.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SPI')
binhoDevice.setClockSPI(0, 4000000)
binhoDevice.setOrderSPI(0, 'MSBFIRST')
binhoDevice.setModeSPI(0, 0)
binhoDevice.setBitsPerTransferSPI(0, 8)

#configure the CS pin
binhoDevice.setIOpinMode(0, 'DOUT')
binhoDevice.setIOpinValue(0, 'HIGH')

binhoDevice.beginSPI(0)
binhoDevice.setIOpinValue(0, 'LOW')

binhoDevice.transferSPI(0, 0x9F)
binhoDevice.transferSPI(0, 0x00)
binhoDevice.transferSPI(0, 0x00)
binhoDevice.transferSPI(0, 0x00)
binhoDevice.transferSPI(0, 0x00)

binhoDevice.setIOpinValue(0, 'HIGH')
binhoDevice.endSPI(0)
```

### transferBufferSPI(spiIndex, numBytes)

This function is used to send and receive data on the SPI bus using the buffer.

#### Inputs:

This function takes two parameters:

* `spiIndex`, which is always 0 on _Binho Nova_ host adapter.
* `numBytes`, which is the number of bytes to transmit from the buffer. Note that this transfer starts from the byte at index 0 in BUF0.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SPI')
binhoDevice.setClockSPI(0, 4000000)
binhoDevice.setOrderSPI(0, 'MSBFIRST')
binhoDevice.setModeSPI(0, 0)
binhoDevice.setBitsPerTransferSPI(0, 8)

#load the buffer with the data
binhoDevice.clearBuffer(0)
binhoDevice.addByteToBuffer(0, 0x9F)
binhoDevice.addByteToBuffer(0, 0x00)
binhoDevice.addByteToBuffer(0, 0x00)
binhoDevice.addByteToBuffer(0, 0x00)
binhoDevice.addByteToBuffer(0, 0x00)

#configure the CS pin
binhoDevice.setIOpinMode(0, 'DOUT')
binhoDevice.setIOpinValue(0, 'HIGH')

binhoDevice.beginSPI(0)
binhoDevice.setIOpinValue(0, 'LOW')

binhoDevice.transferBufferSPI(0, 5)

binhoDevice.setIOpinValue(0, 'HIGH')
binhoDevice.endSPI(0)

#see what data was received during the transfer
print(binhoDevice.readBuffer(0, 5))
```

### endSPI(spiIndex)

This function stops the SPI Controller. The command ends the SPI session until a BEGIN command restarts the SPI controller.

#### Inputs:

This function takes one parameter:

* `spiIndex`, which is always 0 on _Binho Nova_ host adapter.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setOperationMode(0, 'SPI')
binhoDevice.setClockSPI(0, 4000000)
binhoDevice.setOrderSPI(0, 'MSBFIRST')
binhoDevice.setModeSPI(0, 0)
binhoDevice.setBitsPerTransferSPI(0, 8)

#configure the CS pin
binhoDevice.setIOpinMode(0, 'DOUT')
binhoDevice.setIOpinValue(0, 'HIGH')

binhoDevice.beginSPI(0)
binhoDevice.setIOpinValue(0, 'LOW')

binhoDevice.transferSPI(0, 0x9F)

binhoDevice.setIOpinValue(0, 'HIGH')
binhoDevice.endSPI(0)
```

### writeToReadFromSPI(spiIndex, writeFlag, readFlag, numBytes, data)

This function performs a SPI transfer up to 1024 bytes in a single transaction. This function minimizes the number of round-trips between the PC and Nova in order to maximize SPI throughput. This function is highly recommended for reading/writing SPI memory devices or in any other application where multibyte transactions are frequently used.

#### Inputs:

This function takes five parameters:

* `spiIndex`, which is always 0 on _Binho Nova_ host adapter.
* `writeFlag`, which should be 1 if the transaction includes writing data to the SPI bus, or 0 if the transaction is only reading data.
* `readFlag`, which should be 1 if the transaction includes reading data from the SPI bus, or 0 if the transaction is only writing data.
* `numBytes`, which indicates the number of bytes to be transferred on the SPI bus and must match the length of the _data _parameter.
* `data`, which is a string of hex characters without the leading '0x' and no spaces indicating the data to be written to the bus, and must match the length specified by the numBytes parameter.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command when the _readFlag _is 0. If _readFlag _is 1, the host adapter will respond with '-SPI0 RXD' followed by the data read from the SPI bus. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

# configure the SPI Bus
binhoDevice.setOperationMode(0, 'SPI')
binhoDevice.setClockSPI(0, 4000000)
binhoDevice.setOrderSPI(0, 'MSBFIRST')
binhoDevice.setModeSPI(0, 0)
binhoDevice.setBitsPerTransferSPI(0, 8)
hostAdapter.setIOpinValue(0, 'HIGH')

# Assert CS pin
hostAdapter.setIOpinValue(0, 'LOW')
# Read 1024 bytes
print(binhoDevice.writeToReadFromSPI(0, 0, 1, 1024, 0))
# DeAssert CS pin
hostAdapter.setIOpinValue(0, 'HIGH')

hostAdapter.setIOpinValue(0, 'LOW')
# Write 4 bytes and then read 4 bytes (8 total bytes)
print(binhoDevice.writeToReadFromSPI(0, 1, 1, 8, 'DEADBEEF00000000'))
hostAdapter.setIOpinValue(0, 'HIGH')
```
