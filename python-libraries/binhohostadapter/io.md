# IO Functions

### setIOpinMode\(_ioNumber_, _mode_\) 

This function sets the operation mode of a specific pin.

#### Inputs:

This function takes two parameters:

* `ioNumber`, which is the index of the pin to be configured, can be 0 - 4.
* `mode`, which is the desired operation mode for the pin. Valid modes include `DIN`\(Digital Input\), `DOUT` \(Digital Output\), `AIN` \(Analog Input\), `AOUT` \(Analog Output\), and `PWM`.

#### **Outputs:**

The host adapter will respond with '-OK' upon successful execution of the command.

#### **Example Usage:**

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setIOpinMode(0, 'AIN')
```

### getIOpinMode\(ioNumber\)

This function gets the operation mode of a specific pin.

#### Inputs:

This function takes one parameter: 

* `ioNumber`, which is the index of the pin for which the mode is being queried.

#### Outputs:

The host adapter will respond with IO\[n\] followed by 'MODE' followed by the current mode of the specified IO pin. See the details for the [setIOpinMode\(\)](https://support.binho.io/python-libraries/binhohostadapter/io#setiopinmode-ionumber-mode) command above for the possible values of `MODE`.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setIOpinMode(0, 'AIN')
binhoDevice.getIOpinMode(0)
```

### setIOpinPWMFreq\(ioNumber, freq\)

This function sets the PWM frequency of the provided IO pin. This function is only available on IO pins which support PWM output. Please see the [PWMFREQ](https://support.binho.io/user-guide/ascii-interface/io-commands#pwmfreq) ASCII command for additional info.

#### Inputs:

This function takes two parameters:

* `ioNumber`, which is the index of the pin for which the PWM frequency shall be applied.
* `freq`, which is the frequency in Hz. Acceptable range is from 750Hz to 80000Hz.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setIOpinMode(0, 'PWM')
binhoDevice.setIOpinPWMFreq(0, 55000)
```

### getIOpinPWMFreq\(ioNumber\)

This function gets the PWM frequency of the provided IO pin. This function is only available on IO pins which support PWM output. Please see the [PWMFREQ](https://support.binho.io/user-guide/ascii-interface/io-commands#pwmfreq) ASCII command for additional info.

#### Inputs:

This function takes one parameter:

* `ioNumber`, which is the index of the pin for which the PWM frequency is being queried.

#### Outputs:

The host adapter will respond with 'IOn' followed by 'PWMFREQ' followed up the configured frequency in Hz upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setIOpinMode(0, 'PWM')
binhoDevice.setIOpinPWMFreq(0, 55000)
binhoDevice.getIOpinPWMFreq(0)
```

### setIOpinInterrupt\(ioNumber, intMode\)

This function configures the given IO pin to be used as an interrupt source. Interrupts can be on rising edge, falling edge, or any edge. Additional information on which pins can be used as interrupts can be found on the [INT ](https://support.binho.io/user-guide/ascii-interface/io-commands#int)ASCII command documentation.

#### Inputs:

This function takes two parameters:

* `ioNumber`, which is the index of the pin which will be configured as an interrupt source.
* `intMode`, which can be either `RISE`, `FALL`, `CHANGE`, or `NONE`.

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setIOpinMode(3, 'DIN')
binhoDevice.setIOpinInterrupt(3, 'CHANGE')
```

### getIOpinInterrupt\(ioNumber\)

This function returns the currently configured interrupt settings for the provided IO pin.

#### Inputs:

This function takes one parameter:

* `ioNumber`, which is the index of the pin for which the interrupt configuration is being queried.

#### Outputs:

The host adapter will respond with 'IO\[n\]' followed by 'INT' followed by the current configured interrupt mode.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setIOpinMode(3, 'DIN')
binhoDevice.setIOpinInterrupt(3, 'CHANGE')
binhoDevice.getIOpinInterrupt(3)
```

### setIOpinValue\(ioNumber, value\)

This function sets the current value of the provided IO pin. Note that the meaning of value and the acceptable range and units depends on the currently configured MODE of the given IO pin. Please see the documentation of the [VALUE ](https://support.binho.io/user-guide/ascii-interface/io-commands#value)ASCII command for additional information.

#### Inputs:

This function takes two parameters:

* `ioNumber`, which is the index of the pin which will be set to the given value.
* `value`, which can be :
  * 0 or 1 when operating as a digital output, 
  * 0 to 1024 when operating as an analog output or PWM output
  * 0V to 3.3V when operating as analog output
  * 0% to 100% when operating as a PWM output

#### Outputs:

The host adapter will respond with '-OK' upon successful execution of the command.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setIOpinMode(0, 'DOUT')
binhoDevice.setIOpinValue(0, 1)
```

### getIOpinValue\(ioNumber\)

This function returns the current value of the provided IO pin. Note that the meaning of value and the acceptable range and units depends on the currently configured MODE of the given IO pin. Please see the documentation of the [VALUE ](https://support.binho.io/user-guide/ascii-interface/io-commands#value)ASCII command for additional information.

#### Inputs:

This function takes one parameter:

* `ioNumber`, which is the index of the pin for which the value is being queried.

#### Outputs:

The host adapter will respond with 'IO\[n\]' followed by 'VALUE' followed by the current value.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setIOpinMode(0, 'DIN')
binhoDevice.getIOpinValue(0)
```

### getIOpinInterruptFlag\(ioNumber\)

This function checks to see if an interrupt has been received from the host adapter. Note that this function does not involve any direct communication with the host adapter, rather it just checks to see if the python library has already received an interrupt from the device. More details about how interrupts are implemented can be found [here](https://support.binho.io/user-guide/using-the-device/receiving-interrupts).

#### Inputs:

This function takes one parameter:

* `ioNumber`, which is the index of the pin which is being checked for the interrupt flag.

#### Outputs:

This function returns either `True`or `False` . A value of True indicates that an interrupt event has occurred on the given IO pin.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setIOpinMode(3, 'DIN')
binhoDevice.setIOpinInterrupt(3, 'CHANGE')

binhoDevice.getIOpinInterruptFlag(3)
```

### clearIOpinInterruptFlag\(ioNumber\)

This function clears the interrupt flag. Note that this function does not involve any direct communication with the host adapter, rather it just clears the interrupt flag in the python library. More details about how interrupts are implemented can be found [here](https://support.binho.io/user-guide/using-the-device/receiving-interrupts).

#### Inputs:

This function takes one parameter:

* `ioNumber`, which is the index of the pin which is being checked for the interrupt flag.

#### Outputs:

This function does not return any value.

#### Example Usage:

```python
from binhoHostAdapter import binhoHostAdapter

# Change this to match your COMPort
default_commport = "COM22"

binhoDevice = binhoHostAdapter.binhoHostAdapter(default_commport)

binhoDevice.setIOpinMode(3, 'DIN')
binhoDevice.setIOpinInterrupt(3, 'CHANGE')

binhoDevice.getIOpinInterruptFlag(3)
binhoDevice.clearIOpinInterruptFlag(3)
```

