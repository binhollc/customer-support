# Python Libraries

We've released python libraries to make it lighting fast to start automating test and development tasks with a _Binho Multi-Protocol USB Host Adapter._ The packaged releases contains two libraries:

#### binhoHostAdapter

This library is essentially a wrapper for all of the commands presented in the [ASCII Command Set ](https://support.binho.io/user-guide/ascii-interface)documentation. The library is written in such a way to support multiple devices as well as [properly handling INTERRUPTS](https://support.binho.io/user-guide/using-the-device/receiving-interrupts) by making use of threads.

{% page-ref page="binhohostadapter/" %}

#### binhoUtilities

This library provides a handful of functions which aid in device management, as in identifying COM ports and _Binho_ devices attached to the host computer.

{% page-ref page="binhoutilities.md" %}

Here's where the releases live:

{% embed url="https://pypi.org/project/binho-host-adapter/" %}

And here's the source code repository:

{% embed url="https://bitbucket.org/binho-llc/usb-host-adapter-python-libraries/src/master/" %}

## Installation

The officially-supported Python library can easily be installed using pip:

```bash
pip install binho-host-adapter
```

This library is cross-platform and is intended for use with Python 3.x. 

## Example Usage

```python
from binhoHostAdapter import binhoHostAdapter
from binhoHostAdapter import binhoUtilities

print("Demo Script with Binho Host Adapter Python Libraries")
print

utilities = binhoUtilities.binhoUtilities()
devices = utilities.listAvailableDevices()

if len(devices) == 0:
	print("No Devices Found!")
	exit()

elif len(devices) == 1:
	COMPORT = devices[0]
	print("Found 1 attached adapter @ " + devices[0])
	print
else:
	COMPORT = devices[0]
	print("Found more than 1 attached adapter, using first device found on " + COMPORT)
	print

print("Opening " + COMPORT + "...")
print

# create the binhoHostAdapter object
binho = binhoHostAdapter.binhoHostAdapter(COMPORT)

print("Connecting to host adapter...")
print(binho.getDeviceID())
print
```

There are also many examples of the python libraries in action in the examples section of the support portal:

{% page-ref page="examples/" %}

## API Reference

The library contains two classes, each with a distinct purpose. Please see the following pages for the documentation on each of these classes:

Functions to connect to a host adapter and interact with it

{% page-ref page="binhohostadapter/" %}

Utility functions for device discovery and management:

{% page-ref page="binhoutilities.md" %}

