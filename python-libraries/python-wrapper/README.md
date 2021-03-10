# Python Wrapper \(legacy\)

Our initial python library was actually what would best be described as a 'wrapper'. It simply wrapped our ASCII commands into a set of equivalent Python functions. This served as well, but was a temporary solution to buy us time to produce a much more powerful library. We highly encourage everyone to use our new Python package which is packed with features:

{% page-ref page="../binho-python-package.md" %}

This page and the others in this section are left for historical purposes. This 'wrapper' library remains available and we'll continue to support customers using it, however all future improvements will be implemented only in our new library.

### Documentation

The packaged releases contains two libraries:

#### binhoHostAdapter

This library is essentially a wrapper for all of the commands presented in the [ASCII Command Set ](https://support.binho.io/user-guide/ascii-interface)documentation. The library is written in such a way to support multiple devices as well as [properly handling INTERRUPTS](https://support.binho.io/user-guide/using-the-device/receiving-interrupts) by making use of threads.

{% page-ref page="binhohostadapter/" %}

#### binhoUtilities

This library provides a handful of functions which aid in device management, as in identifying COM ports and _Binho_ devices attached to the host computer.

{% page-ref page="binhoutilities.md" %}

Here's where the releases live on PyPi:

{% embed url="https://pypi.org/project/binho-host-adapter/" %}

## Installation <a id="installation"></a>

The officially-supported Python library can easily be installed using pip:

```bash
pip install binho-host-adapter
```

This library is cross-platform and is intended for use with Python 3.x.

## Example Usage <a id="example-usage"></a>

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

## API Reference <a id="api-reference"></a>

The library contains two classes, each with a distinct purpose. Please see the following pages for the documentation on each of these classes.

Functions to connect to a host adapter and interact with it:

{% page-ref page="binhohostadapter/" %}

Utility functions for device discovery and management:

{% page-ref page="binhoutilities.md" %}

