# QuickStart with Python

We've released python libraries to make it lighting fast to start automating test and development tasks with a _Binho Nova Multi-Protocol USB Host Adapter._ The packaged releases contains two libraries:

#### binhoHostAdapter

This library is essentially a wrapper for all of the commands presented in the [ASCII Command Set ](https://support.binho.io/user-guide/ascii-interface)documentation. The library is written in such a way to support multiple devices as well as [properly handling INTERRUPTS](https://support.binho.io/user-guide/using-the-device/receiving-interrupts) by making use of threads.

#### binhoUtilities

This library provides a handful of functions which aid in device management, as in identifying COM ports and _Binho_ devices attached to the host computer.

### Step \#1: Download and Install the Binho Host Adapter Libraries

The officially-supported Python library can easily be installed using pip:

```bash
pip install binho-host-adapter
```

This library is cross-platform and is intended for use with Python 3.x. Source code can be found [here](https://bitbucket.org/binho-llc/usb-host-adapter-python-libraries/src/master/).

![](../.gitbook/assets/pip-install-binhohostadapter.gif)

### Step \#2: Find Connected Devices

Let's use the [binhoUtilities ](../python-libraries/binhoutilities.md)class to find devices attached to this computer. Start by creating a new python script, call it `binhoDemo.py`, and enter the following code:

{% code title="binhoDemo.py" %}
```python
from binhoHostAdapter import binhoUtilities
from binhoHostAdapter import binhoHostAdapter

utilities = binhoUtilities.binhoUtilities()
devices = utilities.listAvailableDevices()

print('Binho Host Adapters Found On The Following Ports:')
print(devices)
```
{% endcode %}

Make sure your device is plugged in and then run the script. A list of ports where Binho devices have been found will be printed to the screen.

![](../.gitbook/assets/python-findconnecteddevices.gif)

### Step \#3: Connect to a Device

Now that you know how to use the `binhoUtilities` class to discover devices, let's extend the script to connect with the first device it discovers. We'll leave a comment where we'll implement our desired functionality later in Step \#4, and then just immediately close the connection and exit.

{% code title="binhoDemo.py" %}
```python
from binhoHostAdapter import binhoUtilities
from binhoHostAdapter import binhoHostAdapter

utilities = binhoUtilities.binhoUtilities()
devices = utilities.listAvailableDevices()

print('Binho Host Adapters Found On The Following Ports:')
print(devices)
print()

# Make sure at least one binho device was found before proceeding
if len(devices) < 1:
    print('No Binho Host Adapter found...Quitting script')
    exit(1)

# Target the port of the first device in the list
targetPort = devices[0]
print('Connecting to host adapter on ' + targetPort)
print()

# Connect to the binho device on the target port
binho = binhoHostAdapter.binhoHostAdapter(targetPort)
print('Connected!')
print()

#
# Here's where we'll implement our desired functionality, see Step #4
#

# Close the connection to the device before exiting
binho.close()
print('Connection closed!')
print()

# Exit gracefully
exit(0)
```
{% endcode %}

![](../.gitbook/assets/python-connect-to-device.gif)

### Step \#4: Interact

Now that we have a basic script which handles device discovery, connection, and disconnection, all that's left to do is implement our desired functionality. The simple example below shows how to generate a PWM signal on IO0.

{% code title="binhoDemo.py" %}
```python
from binhoHostAdapter import binhoUtilities
from binhoHostAdapter import binhoHostAdapter

utilities = binhoUtilities.binhoUtilities()
devices = utilities.listAvailableDevices()

print('Binho Host Adapters Found On The Following Ports:')
print(devices)
print()

# Make sure at least one binho device was found before proceeding
if len(devices) < 1:
    print('No Binho Host Adapter found...Quitting script')
    exit(1)

# Target the port of the first device in the list
targetPort = devices[0]
print('Connecting to host adapter on ' + targetPort)
print()

# Connect to the binho device on the target port
binho = binhoHostAdapter.binhoHostAdapter(targetPort)
print('Connected!')
print()

# Set the LED color to GREEN
binho.setLEDColor('GREEN')
print('Set LED to Green')
print()

# Set the operation mode to IO
binho.setOperationMode(0, 'IO')

# Set IO0 to PWM mode
binho.setIOpinMode(0, 'PWM')

# Set IO0 PWM Frequency to 75kHz
binho.setIOpinPWMFreq(0, 75000)

# Set IO0 PWM duty cycle to 512/1024
binho.setIOpinValue(0, 512)
print('IO0 set to PWM with 50% duty cycle / 75kHz frequency')
print()

# Close the connection to the device before exiting
binho.close()
print('Connection closed!')
print()

# Exit gracefully
exit(0)
```
{% endcode %}

![](../.gitbook/assets/python-io-pwm.gif)

### Going Further

We've just covered the most basic case of using the Python libraries to automate your _Binho Nova_. The full documentation of the python libraries can be found here:

{% page-ref page="../python-libraries/" %}

And example Python scripts which demonstrate all of the various protocols and features of the _Binho Nova_ can be found here:

{% page-ref page="../python-libraries/examples/" %}

And of course, the libraries are open-source and the repository can be found here:

{% embed url="https://bitbucket.org/binho-llc/usb-host-adapter-python-libraries/src/master/" %}



