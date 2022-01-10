# binhoUtilities

{% hint style="success" %}
We highly encourage everyone to use our[ new Python package](https://support.binho.io/python-libraries/binho-python-package) which is packed with features. This library is still supported, but is not recommended for new design.
{% endhint %}

The _binhoUtilities_ class contains several functions which help with managing serial ports, discovering devices, and connecting to specific devices. This class can be included in your code as show below.

```python
from binhoHostAdapter import binhoUtilities
```

One of the key benefits of using these helper functions is that care has been taken to ensure they work well across the various operating systems, particularly in regards to how serial ports are named and accessed.

### listAvailablePorts()

This function returns an array of currently available serial ports. The serial ports returned may or may not be a binho Multi-Protocol USB Host Adapter or some other device. See _listAvailableDevices_ function below to get a list of serial ports related only to binho Multi-Protocol Host Adapters. Note that any serial ports which are currently opened will not be shown in the list.

**Inputs:**

This function takes no parameters.

**Outputs:**

This function returns an array of available serial ports as strings.

**Example Usage:**

```python
from binhoHostAdapter import binhoUtilities

utilities = binhoUtilities.binhoUtilities()

availableCommPorts = utilities.listAvailablePorts()

print(availableCommPorts)
# Console Output (on Win10 PC):
# ['COM22', 'COM23']
```

### listAvailableDevices()

This function returns an array of currently available serial ports that are associated with a binho Multi-Protocol USB Host Adapter. See _listAvailablePorts_ function above to get a list of all available serial ports. Note that any serial ports which are currently opened will not be shown in the list.

**Inputs:**

This function takes no parameters.

**Outputs:**

This function returns an array of available serial ports as strings.

**Example Usage:**

```python
from binhoHostAdapter import binhoUtilities

utilities = binhoUtilities.binhoUtilities()

availableDevices = utilities.listAvailableDevices()

print(availableDevices)
# Console Output (on Win10 PC):
# ['COM22']
```

### getPortByDeviceID(deviceID)

This function returns an array which will either be either of length 0 (empty) or of length one, where the element will be a string containing the name of the serial port of the binho Multi-Protocol USB Host Adapter with the desired _deviceID_. This function is very useful when multiple host adapters are connected to the same computer.

**Inputs:**

This function takes 1 parameter, _deviceID_, as a string. It can either include or omit the "_0x_" prefix on the _deviceID_.

**Outputs:**

This function returns an array of serial ports as strings. Note that since the _deviceID_s are globally unique, the returned array will have either zero or one element.

**Example Usage:**

```python
from binhoHostAdapter import binhoUtilities

utilities = binhoUtilities.binhoUtilities()

targetDeviceCommPort = utilities.getPortByDeviceID('0xc59bb495504e5336362e3120ff032d2c')

print(targetDeviceCommPort)
# Console Output (on Win10 PC):
# ['COM22']

targetDeviceCommPort = utilities.getPortByDeviceID('c59bb495504e5336362e3120ff032d2c')

print(targetDeviceCommPort)
# Console Output (on Win10 PC):
# ['COM22']
```
