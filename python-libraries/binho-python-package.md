# Binho Python Package

Thanks to incredible feedback and support from our customers, we've released an entirely rebuilt python library for Binho Nova. While we'll continue to support our previous library, active development will only take place on this one.

Aside from taking a more _**pythonic**_ approach to implementing the library, we've also introduced a lot of structure and abstraction to make it even easier to use your Nova, including a command line interface and interactive shell. We've also baked numerous example scripts right into the library for easy reference.

## Getting Started

The easiest way to get started is to install this library using pip:

```text
pip install binho
```

Using Nova in your scripts is as simple as importing the library:

```python
import binho
```

Then initialize a connection to the binho device as such:

```python
# grab the first device found the system finds
binho = binhoHostAdapter()
```

When working on setups with multiple devices, you can specify the device to connect to in the following 3 ways:

1. grab the device with a specific index `binho = binhoHostAdapter(index=0)`
2. or get the device using the COM port `binho = binhoHostAdapter(port=targetComport)`
3. or get the device using the deviceID number `binho = binhoHostAdapter(deviceID = targetDeviceID)`

At this point it's possible to control the device as desired. Examples of common use cases are included in this library and are briefly discussed below. When you're done with the device, be sure to close out the connection with the following:

```text
binho.close()
```

That's all there is to it. The example scripts are introduced below, but it may also make sense to review the new Command line interface as well, as it may be possible to achieve your goals without writing any code at all.

## Example Scripts

Take a look in the [/binho/examples](https://github.com/binhollc/binho-python-package/tree/main/binho/examples) folder of the codebase \(hosted on github\) to see example scripts which demonstrate how to use this library in your own scripts to automate Nova. These example scripts feature a lot of commentary and serve as a tutorial for using this library.

**Basic Examples:**

* [Hello World LED](https://github.com/binhollc/binho-python-package/blob/main/binho/examples/00_hello_world_led.py) This example shows the basics of establishing a connection with device and setting it's LED to BLUE. It's a boring example, but introduces the typical script structure and basics of working with this library.
* [Digital IO and PWM](https://github.com/binhollc/binho-python-package/blob/main/binho/examples/01_digitalio_and_pwm.py) This example shows how to use IO pins for digital input and output, as well as configuring hardware PWM.
* [Analog IO, DAC, and ADC](https://github.com/binhollc/binho-python-package/blob/main/binho/examples/02_analogio_dac_and_adc.py) This example shows how to use IO pins for analog input \(using the ADC\) and output \(using the DAC\).
* [SPI Communication](https://github.com/binhollc/binho-python-package/blob/main/binho/examples/03_spi_communication.py) This example shows how to configure the device for SPI communication and transfer data. This example can be run by connecting SDI and SDO signals to loopback the data.
* [I2C Communication](https://github.com/binhollc/binho-python-package/blob/main/binho/examples/04_i2c_communication.py) This example demonstrates how to configure the device for I2C communication and perform reads and writes. It also shows how to scan the bus, and perform read register and write register commands. This example is easily adaptable to work with any I2C device you have available.
* [1Wire Communication](https://github.com/binhollc/binho-python-package/blob/main/binho/examples/05_1wire_communication.py) This example demonstrates how to configure the device for \(Dallas / Maxim\) 1Wire communication by writing to and reading from a [DS24B33+ 1Wire EEPROM](https://www.mouse..com/ProductDetail/Maxim-Integrated/DS24B33%2b/?qs=%2F%2FkzJz%252Bz9F%2F59wgCoS63UQ%3D%3D). Note that the command set for most 1Wire EEPROMs is very similar so this example may work with other devices too.

**Advanced Examples:**

* [I2C EEPROM Demo](https://github.com/binhollc/binho-python-package/blob/main/binho/examples/10_i2c_eeprom_demo.py) - Reading and Writing common EEPROM devices has never been easier. Also shows how to read to / write from, and verify against .bin and .hex files.
* [SPI Flash Demo](https://github.com/binhollc/binho-python-package/blob/main/binho/examples/20_spi_flash_demo.py) - Reading and Writing common FLASH devices has never been easier using our SPI
* [SPI Flash SFDP](https://github.com/binhollc/binho-python-package/blob/main/binho/examples/21_spi_flash_sfdp.py) - Discover the parameters of a SPI Flash device without any prior knowledge by reading the JEDEC ID and SPI Flash Discoverable Parameters.
* _more coming soon_

## Command Line Interface

he installation of this library also includes the new command line interface which makes it possible to perform a lot of common functions without needing to write any code. The format of the commands is as follows:

```text
binho <<subcommand>> [arguments]
```

Each command has their own unique arguments, but all commands except 'info' support the following:

* `-h, --help`: prints the list of arguments
* `-v, --verbose`: display more details on the console during execution
* `-d, --device <deviceID>`: connect to the device with the provided deviceID number
* `-p, --port <commport>`: connect to the device on the provided COM port
* `-i, --index <i>`: connect to the device at index i

_Note that only one of `-d`, `-p`, or `-i` arguments can be supplied to any command._

**Device Management Subcommands**

* **`binho info`** This command can be used to find all Novas connected to the PC and get their associated information such as serial number, COM port, and firmware version. It will also indicate if a device is in DAPLink or Bootloader mode as well.
* **`binho dfu`** This command can be used to automatically update device firmware or just enter Bootloader mode.

**IO Subcommands**

* **`binho gpio`** This command can be used to take digital readings or drive IO pins as digital outputs. Note that the device does not retain it's state between consecutive runs of this command. As such, a subsequent run will overwrite any previous pin configurations.
* **`binho dac`** This command can be used to set the DAC output to a given voltage or raw 10bit value.
* **`binho adc`** This command can be used to take readings from IO pins using the analog inputs \(ADC\).
* **`binho pwm`** This command can be used to configure the IO pins to be used as PWM outputs.

**Protocol Subcommands**

* **`binho i2c`** This command can be used to perform I2C Scans, Reads, and Writes.
* **`binho eeprom`** This command can be used to read from and write to common I2C EEPROM devices.
* **`binho spi`** This command can be used to perform SPI transfers.
* **`binho spiflash`** \[Coming Soon!\] This command can be used to read from and write to common SPI Flash devices.
* **`binho 1wire`** This command can be used to communicate with 1Wire devices.

**DAPLink Subcommands**

Binho Nova can be used to program and debug microcontrollers by operating in DAPLink mode.

* **`binho daplink`** Use this command to switch Nova into and out of DAPLink mode.
* **`binho flasher`** While in DAPLink mode, this command can be used to program bin/hex files into microcontrollers.

**Misc. Subcommands**

* **`binho shell`** This command can be used to open up a connection to the device and begin an interactive shell.
* **`binho custom`** Adding custom commands is very easy! This command is just meant as a template which can be used to create your own commands to extend the command line functionality for any specific tasks. You can see the implementation [here](https://github.com/binhollc/binho-python-package/blob/main/binho/commands/binho_custom.py).

## Development

We welcome contributions to our library, and we've made it easy. Here's some brief guidance to get started developing with the library.

#### Installation

```text
python3 setup.py install
```

#### Building Docs

We're planning to use ReadTheDocs to host detailed library documentation in the near future. You can build the documentation with the following command.

```text
pip3 install .[dev]
docs\make.bat html
```

#### Roadmap

We're looking forward to adding support for various common UART use-cases. Additionally, we'll be adding in support for Atmel SWI if we find enough folks interested in that protocol. Our development roadmap is highly determined by customer requests and feedback, so please feel free to reach out if there's a particular feature you'd like to see added into this library.

## Additional Resources

The release can be viewed on PyPi here:

{% embed url="https://pypi.org/project/binho/" %}

And here's another direct link to the our public repo on Github:

{% embed url="https://github.com/binhollc/binho-python-package" %}

