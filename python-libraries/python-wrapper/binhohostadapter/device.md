# Device Functions

{% hint style="success" %}
We highly encourage everyone to use our[ new Python package](https://support.binho.io/python-libraries/binho-python-package) which is packed with features. This library is still supported, but is not recommended for new design.
{% endhint %}

### echo()

This function toggles whether or not the device echos back all the commands it receives. This really has no practical use in automation, but was included in the python library for the sake of completion. Turning the echo on is best for manual use in a terminal application which doesn't support local echo. Aside from this use case, it is not recommended to turn on echo. The default state of the device is with echo turned off.&#x20;

**Inputs:**

This function takes no parameters.

**Outputs:**

The host adapter will respond with `-OK` upon successful execution of the command.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

# Toggle Echo On
binhoDevice.echo()

# Toggle Echo Off again
binhoDevice.echo()
```

### ping()

This function provides for a simple way to solicit a response from the device. This can be useful when verifying the connection status with the device.

**Inputs**:

This function takes no parameters.

**Outputs:**

The host adapter will respond with `-OK` upon successful execution of the command.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.ping()
```

### setOperationMode(_index_, _mode_)

This function sets the mode of operation of the given adapter interface. This determines which protocol is being used, and therefore the pin functions. Any IO pins which are not assigned a function for the set protocol will continue to be available for concurrent use. The default operation mode on power up is _IO._

**Inputs:**

This function takes 2 parameters, _index_, as an integer, and _mode_, as a string.&#x20;

Note that for _Binho Multi-Protocol USB Host Adapter_, the only valid _index _is 0. This aspect of the protocol is design to support future products which may have more than one adapter interface.

The following list of values are valid values for _mode _parameter:

* 'I2C'
* 'SPI'
* 'UART'
* '1-WIRE'
* 'SWI'
* 'IO'

{% hint style="info" %}
The list of possible device modes will grow as new protocols added to the firmware. Note that it will be important to ensure your device has the latest version of firmware loaded on to it.
{% endhint %}

**Outputs:**

The host adapter will respond with `-OK` upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with `-NG` indicating the command did not execute successfully.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

# Change from IO mode to I2C mode
binhoDevice.setOperationMode(0, 'I2C')

# Now change from I2C mode to SPI mode
binhoDevice.setOperationMode(0, 'SPI')

# Let's go back to IO mode
binhoDevice.setOperationMode(0, 'IO')
```

### getOperationMode(_index_)

This function gets the host adapter operation mode. This determines which protocol is being used, and therefore the pin functions. Any IO pins which are not assigned a function for the set protocol will continue to be available for concurrent use. The default operation mode on power up is _IO._

**Inputs:**

This function takes 1 parameters, _index_, as an integer. Note that for Binho Multi-Protocol USB Host Adapter, the only valid _index _is 0. This aspect of the protocol is design to support future products which may have more than one adapter interface.

**Outputs:**

The host adapter will respond with one of the following responses:

* `-MODE [index] IO` when the host adapter is in IO mode (default).
* `-MODE [index] I2C` when the host adapter is in I2C mode.
* `-MODE [index] SPI` when the host adapter is in SPI mode.
* `-MODE [index] 1WIRE` when the host adapter is in 1-WIRE mode.
* `-MODE [index] SWI` when the host adapter is in SWI mode.
* `-MODE [index] UART` when the host adapter is in UART mode.

{% hint style="info" %}
The list of possible device modes will grow as new protocols added to the firmware. Note that it will be important to ensure your device has the latest version of firmware loaded on to it.
{% endhint %}

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

print(binhoDevice.getOperationMode(0))
#-MODE IO

# Change from IO mode to I2C mode
binhoDevice.setOperationMode(0, 'I2C')
print(binhoDevice.getOperationMode(0))
#-MODE I2C

# Now change from I2C mode to SPI mode
binhoDevice.setOperationMode(0, 'SPI')
print(binhoDevice.getOperationMode(0))
#-MODE SPI
```

### setNumericalBase(_base_)

This function sets the host adapter display base. This determines which radix is being used when data is sent back as ASCII characters. The supported numerical bases are binary, decimal, and hexadecimal. The default numerical base on power up is _hex._

**Inputs:**

This function takes 1 parameter, _base_, as a string. The following list of values are valid for this parameter:

* 'BIN' or '2' sets the numerical base to base-2 (binary).
* 'DEC' or '10' sets the numerical base to base-10 (decimal).
* 'HEX' or '16' sets the numerical base to base-16 (hexadecimal).

**Outputs:**

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

# Set the numerical base to each of the three supported bases
binhoDevice.setNumericalBase('BIN')
binhoDevice.setNumericalBase('DEC')
binhoDevice.setNumericalBase('HEX')

# These functions do the same as the three above
binhoDevice.setNumericalBase('2')
binhoDevice.setNumericalBase('10')
binhoDevice.setNumericalBase('16')
```

### getNumericalBase()

This function gets the host adapter display base. This determines which radix is being used when data is sent back as ASCII characters. The supported numerical bases are binary, decimal, and hexadecimal. The default numerical base on power up is _hex._

**Inputs:**

This function takes no parameters.

**Outputs:**

The host adapter will respond with one of the following responses:

* '-BASE BIN' when the host adapter is set to base-2 (binary).
* '-BASE DEC' when the host adapter is set to base-10 (decimal).
* '-BASE HEX' when the host adapter is set to base-16 (hexadecimal, default).

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

print(binhoDevice.getNumericalBase())
#-BASE HEX

# Change from hex to binary
binhoDevice.setNumericalBase('BIN')
print(binhoDevice.getNumericalBase())
#-BASE BIN

# Now change to decimal / base-10
binhoDevice.setNumericalBase('DEC')
print(binhoDevice.getNumericalBase())
#-BASE DEC
```

### setLEDRGB(_r_, _g_, _b_)

This function sets the color of the internal RGB LED by setting the 8-bit red, green, and blue values.

**Inputs:**

This function takes three parameters, each as integer values:

* _r_ - red value expressed as 0 - 255
* _g_ - green value expressed as 0 - 255
* _b_ - blue value expressed as 0 - 255

Note that any value passed in less than 0 will be rounded up to 0. Conversely, any value passed in greater than 255 will be rounded down to 255.

**Outputs:**

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setLEDRGB(128,64,128)
```

### setLEDColor(_color_)

This function sets the color of the internal RGB LED by passing the name of a common color.

**Inputs:**

This function takes a single input parameter, _color_, as a string. The following list of values are valid inputs for this parameter:

* 'WHITE'
* 'RED'
* 'LIME'
* 'GREEN'
* 'BLUE'
* 'YELLOW'
* 'CYAN'
* 'AQUA'
* 'MAGENTA'
* 'FUCHSIA'
* 'PURPLE'

**Outputs:**

The host adapter will respond with '-OK' upon successful execution of the command. In case of an invalid parameter, the host adapter will respond with '-NG' indicating the command did not execute successfully.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setLEDColor('WHITE')
binhoDevice.setLEDColor('RED')
binhoDevice.setLEDColor('LIME')
binhoDevice.setLEDColor('GREEN')
binhoDevice.setLEDColor('BLUE')
binhoDevice.setLEDColor('YELLOW')
binhoDevice.setLEDColor('CYAN')
binhoDevice.setLEDColor('AQUA')
binhoDevice.setLEDColor('MAGENTA')
binhoDevice.setLEDColor('FUCHSIA')
binhoDevice.setLEDColor('PURPLE')
```

### getFirmwareVer()

This function returns the firmware version information of the host adapter.

**Inputs:**

This function takes no parameters.

**Outputs:**

The host adapter will respond with '-FWVER' followed by a space followed by the version.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
print(binhoDevice.getFirmwareVer())
#-FWVER 0.3
```

### getHardwareVer()

This function returns the hardware version information of the host adapter.

**Inputs:**

This function takes no parameters.

**Outputs:**

The host adapter will respond with '-HWVER' followed by a space followed by the version.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
print(binhoDevice.getHardwareVer())
#-HWVER 1.0
```

### resetToBtldr()

This function performs a reset but will remain in the bootloader upon power-up. This will cause the USB connection to drop and re-enumerate. The host adapter will remain in the bootloader until a new firmware image is loaded, or until it is power cycled by unplugging it from USB and plugging it back in. Note that while the device is in the bootloader, it will act as a USB mass storage device, and will not respond to any of protocol commands.

**Inputs:**

This function takes no parameters.

**Outputs:**

The host adapter will respond with '-BTLDR OK' immediately before resetting.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
print(binhoDevice.resetToBtldr())
#-BTLDR OK
```

### reset()

This function performs a reset. This will cause the USB connection to drop and re-enumerate. This also restores all device settings to their defaults.

**Inputs:**

This function takes no parameters.

**Outputs:**

The host adapter will respond with '-RESET OK' immediately before resetting.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
print(binhoDevice.reset())
#-RESET OK
```

### getDeviceID()

This function returns the globally-unique device identifier. This deviceID can be particularly useful to reference the device when using multiple host adapters on the same computer.&#x20;

**Inputs:**

This function takes no parameters.

**Outputs:**

The host adapter will respond with '-ID 0x' followed by the 16-byte device ID. The ID will be displayed in hex regardless of the current Numerical Base setting.

**Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)
print(binhoDevice.getDeviceID())
#-ID 0xc59bb495504e5336362e3120ff041d2c
```
