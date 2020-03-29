# ASCII Command Set

The ASCII Command Set provides an easy means to open up a serial port connection and interact with the device via a human-readable protocol. This is ideal for tasks such as debugging device drivers / performance issues, as well as device evaluation and proof of concept development. 

{% hint style="info" %}
This section of the User Guide is presented in the format of a "Language Reference" to list all of the commands and their parameters. A "Programmer's Guide" which demonstrates how the commands can be used to achieve certain functionality can be found [here](https://support.binho.io/user-guide/using-the-device/device-settings).
{% endhint %}

The ASCII Commands are broken down into several categories.

### Device

The _device_ category includes functions which get device information, control the LED, and set the mode of operation among other things.

{% page-ref page="device-commands.md" %}

### Buffer

The _buffer_ category includes the functions necessary to read/write the internal buffer. Using the buffer is especially useful when sending predetermined multi-byte transfers, such as when reading and writing FRAM, EEPROM, and FLASH memories.

{% page-ref page="buffer-commands.md" %}

### IO

The _IO_ category includes the functions necessary to control the pins when not being used for digital communications. Each of the available IO pins can be assigned a variety of functions such as digital input, digital output, analog input, analog output, PWM output, and other specialized functions.

{% page-ref page="io-commands.md" %}

### SPI

{% page-ref page="spi-commands.md" %}

### I2C

{% page-ref page="i2c-commands.md" %}

### 1-WIRE

{% page-ref page="1-wire-commands.md" %}

### SWI

{% page-ref page="swi-commands.md" %}

### UART

{% page-ref page="uart-commands.md" %}

