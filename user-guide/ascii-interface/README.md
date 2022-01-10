# ASCII Command Set

The ASCII Command Set provides an easy means to open up a serial port connection and interact with the device via a human-readable protocol. This is ideal for tasks such as debugging device drivers / performance issues, as well as device evaluation and proof of concept development.&#x20;

{% hint style="info" %}
This section of the User Guide is presented in the format of a "Language Reference" to list all of the commands and their parameters. A "Programmer's Guide" which demonstrates how the commands can be used to achieve certain functionality can be found [here](https://support.binho.io/user-guide/using-the-device/device-settings).
{% endhint %}

The ASCII Commands are broken down into several categories.

### Device

The _device_ category includes functions which get device information, control the LED, and set the mode of operation among other things.

{% content-ref url="device-commands.md" %}
[device-commands.md](device-commands.md)
{% endcontent-ref %}

### Buffer

The _buffer_ category includes the functions necessary to read/write the internal buffer. Using the buffer is especially useful when sending predetermined multi-byte transfers, such as when reading and writing FRAM, EEPROM, and FLASH memories.

{% content-ref url="buffer-commands.md" %}
[buffer-commands.md](buffer-commands.md)
{% endcontent-ref %}

### IO

The _IO_ category includes the functions necessary to control the pins when not being used for digital communications. Each of the available IO pins can be assigned a variety of functions such as digital input, digital output, analog input, analog output, PWM output, and other specialized functions.

{% content-ref url="io-commands.md" %}
[io-commands.md](io-commands.md)
{% endcontent-ref %}

### SPI

{% content-ref url="spi-commands.md" %}
[spi-commands.md](spi-commands.md)
{% endcontent-ref %}

### I2C

{% content-ref url="i2c-commands.md" %}
[i2c-commands.md](i2c-commands.md)
{% endcontent-ref %}

### 1-WIRE

{% content-ref url="1-wire-commands.md" %}
[1-wire-commands.md](1-wire-commands.md)
{% endcontent-ref %}

### SWI

{% content-ref url="swi-commands.md" %}
[swi-commands.md](swi-commands.md)
{% endcontent-ref %}

### UART

{% content-ref url="uart-commands.md" %}
[uart-commands.md](uart-commands.md)
{% endcontent-ref %}
