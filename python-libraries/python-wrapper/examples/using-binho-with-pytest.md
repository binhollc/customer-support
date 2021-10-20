# Using Binho with Pytest

{% hint style="success" %}
We highly encourage everyone to use our[ new Python package](https://support.binho.io/python-libraries/binho-python-package) which is packed with features. This library is still supported, but is not recommended for new design.
{% endhint %}

The _Binho Nova Multi-Protocol USB Host Adapter_ pairs elegantly with [pytest ](https://docs.pytest.org/en/latest/)to implement an incredibly robust and efficient hardware and firmware testing platform.

We'll leave discussion of the features of pytest for their own very-thorough documentation, and just hit on a few tips to make it fast and easy to get started using it with your host adatper.

In order for the serial connection with your Binho Nova to persist through the entire test plan, you'll want to use a fixture. This is as easy as creating a `conftest.py` file in the test directory. Additionally, allow it accept command line parameters for device IDs, as this makes it easy to use multiple adapters on the same host computer.

Here's an example of a conftest.py file which creates two fixtures, one for each Binho Nova used in the testing:

```python
from binhoHostAdapter import binhoHostAdapter
from binhoHostAdapter import binhoUtilities

import pytest


def pytest_addoption(parser):
    parser.addoption(
        "--dutID", action="store", default="0xc59bb495504e5336362e3120ff032d2c", help="dutID: the GUID of the device to perform the test on"
    )
    parser.addoption(
        "--fixtureID", action="store", default="0xa917ec725150464a39202020ff172123", help="fixtureID: the GUID of the test fixture device"
    )

@pytest.fixture(scope="session")
def dut(request):

	# enter the deviceID of target test device
	binho_test_deviceID = request.config.getoption('--dutID')

	utilities = binhoUtilities.binhoUtilities()

	binhoTesterCommPorts = utilities.getPortByDeviceID(binho_test_deviceID)
	binhoTesterCommPort = 0

	if len(binhoTesterCommPorts) == 0:
	    print("ERROR: No Binho Tester Device found!")
	    exit(1)
	elif len(binhoTesterCommPorts) > 1:
	    print("ERROR: More than one Binho Tester Device found!")
	    exit(1)
	else:
	    binhoTesterCommPort = binhoTesterCommPorts[0]
	    print("Found Binho Tester Device on " + binhoTesterCommPort)


	print("Opening " + binhoTesterCommPort + "...")
	print

	# create the binhoHostAdapter object
	testDevice = binhoHostAdapter.binhoHostAdapter(binhoTesterCommPort)

	print("Connecting to binho host adapter tester...")
	print

	def teardown():
		#print('--test device teardown')
		testDevice.close()

	request.addfinalizer(teardown)
	return testDevice

@pytest.fixture(scope="session")
def testFixture(request):

	# enter the deviceID of target test device
	binho_test_deviceID = request.config.getoption('--fixtureID')

	utilities = binhoUtilities.binhoUtilities()

	binhoTesterCommPorts = utilities.getPortByDeviceID(binho_test_deviceID)
	binhoTesterCommPort = 0

	if len(binhoTesterCommPorts) == 0:
	    print("ERROR: No Binho Tester Device found!")
	    exit(1)
	elif len(binhoTesterCommPorts) > 1:
	    print("ERROR: More than one Binho Tester Device found!")
	    exit(1)
	else:
	    binhoTesterCommPort = binhoTesterCommPorts[0]
	    print("Found Binho Tester Device on " + binhoTesterCommPort)


	print("Opening " + binhoTesterCommPort + "...")
	print

	# create the binhoHostAdapter object
	testDevice = binhoHostAdapter.binhoHostAdapter(binhoTesterCommPort)

	print("Connecting to binho host adapter tester...")
	print

	def teardown():
		#print('--test device teardown')
		testDevice.close()

	request.addfinalizer(teardown)
	return testDevice

```

In this fashion, the serial connections with the devices will be opened up at the beginning of the testing. Then, using these devices from within the test cases is simple, just pass them into each function that uses them as a parameter. Here's an example function to demonstrate this:

```python
def test_TestDevicesCommCheck(self, dut, testFixture):

        response = dut.ping()
        assert response == '-OK'

        response = testFixture.ping()
        assert response == '-OK'

        response = dut.setLEDColor('YELLOW')
        assert response == '-OK'

        response = testFixture.setLEDColor('YELLOW')
        assert response == '-OK'
```

Writing automated hardware and firmware tests has never been easier!
