# Buffer Functions

{% hint style="success" %}
We highly encourage everyone to use our[ new Python package](https://support.binho.io/python-libraries/binho-python-package) which is packed with features. This library is still supported, but is not recommended for new design.
{% endhint %}

The host adapter includes a 256-byte buffer which can be used to optimize multi-byte read and writes using any of the supported communication protocols.

### clearBuffer\(_index_\)

This function clears the entire buffer, setting all bytes to 0.

**Inputs:**

This function one parameter, index. There is only one buffer, so this will always be 0.

**Outputs:**

The host adapter will respond with '-OK' upon successful execution of the command.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.clearBuffer(0)
```

### addByteToBuffer\(_index_, _val_\)

This function adds a single byte to the buffer. Note that the buffer automatically keeps track of / auto-increments the index as data is loaded.

**Inputs:**

This function takes two parameters:

* `index`, the index of the buffer. There is only one buffer, so this will always be zero.
*  `val`, as 8-bit integer value.

**Outputs:**

The host adapter will respond with '-OK' upon successful execution of the command.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.addByteToBuffer(0, 12)
binhoDevice.addByteToBuffer(0, 136)
binhoDevice.addByteToBuffer(0, 0)
binhoDevice.addByteToBuffer(0, 255)
```

### readBuffer\(_index_, _numBytes_\)

This function reads out a given number of bytes from the buffer. Note that the bytes are **not** cleared out of the buffer after the read. They will remain in the buffer until they are overwritten or when the buffer is cleared.

**Inputs:**

This function takes two parameters:

* `index`, the index of the buffer. There is only one buffer, so this will always be zero.
* `numBytes`,  an 8-bit integer value of the number of bytes to read from the buffer.

**Outputs:**

The host adapter will respond with 'BUF0' followed by numBytes of bytes read from the buffer separated by spaces.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.addByteToBuffer(0, 12)
binhoDevice.addByteToBuffer(0, 136)
binhoDevice.addByteToBuffer(0, 0)
binhoDevice.addByteToBuffer(0, 255)

print(binhoDevice.readBuffer(0, 4))
#BUF0 12 136 0 255
```

### writeToBuffer\(_index_, _startIndex_, _values_\)

This function writes up to 32 bytes into the buffer in a single transaction, beginning at the startIndex index in the buffer. This provides for a faster way to fill the buffer when the data is predetermined, such as when programming memory devices.

**Inputs:**

This function takes three parameters:

* `index` -- the index of the buffer. There is only one buffer, so this will always be zero.
* `startIndex` -- the position in the buffer that the first byte should be stored. The index will be incremented for each of the following bytes.
* `values` -- an array of up to 32 bytes of data to be loaded into the buffer beginning at the startIndex position.

**Outputs:**

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

# Fill the entire buffer as quickly as possible
for y in range(8):
    data = []
    for x in range(32):
    	data += [x + (y*32)]

	binhoDevice.writeToBuffer(0, y*32, data)
```



