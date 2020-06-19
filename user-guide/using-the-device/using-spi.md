# Using SPI

The _Binho Nova Multi-Protocol USB Host Adapter_ can serve as a SPI bus controller device which can be configured to meet the needs of nearly any SPI bus implementation. The SPI Clock can be set to a frequency between 500kHz and 12MHz, and all 4 SPI modes are supported. Transfers can be in 8-bit or 16-bit packets. The pin assignments for the SPI protocol are show in the graphic below, indicated in yellow:

![](../../.gitbook/assets/20200619_novapinout.png)

The following table gives a brief overview of the available SPI commands. 

| Commands | Description | Link |
| :--- | :--- | :--- |
| **CLK** | Gets/sets the SPI clock frequency. | [Details](https://support.binho.io/user-guide/ascii-interface/spi-commands#clk) |
| **ORDER** | Gets/sets the SPI bus bit order MSB-first/LSB-first. | [Details](https://support.binho.io/user-guide/ascii-interface/spi-commands#order) |
| **MODE** | Gets/sets the SPI mode. Modes 0, 1, 2, or 3 are determined by CPOL and CPHA | [Details](https://support.binho.io/user-guide/ascii-interface/spi-commands#mode) |
| **CPOL** | Gets/sets the SPI Clock Polarity. This is related to the SPI mode. | [Details](https://support.binho.io/user-guide/ascii-interface/spi-commands#cpol) |
| **CPHA** | Gets/sets the SPI Clock Phase. This is related to the SPI mode. | [Details](https://support.binho.io/user-guide/ascii-interface/spi-commands#cpha) |
| **TXBITS** | Gets/sets the number of bits per transmission. | [Details](https://support.binho.io/user-guide/ascii-interface/spi-commands#txbits) |
| **BEGIN** | Starts the SPI Controller. | [Details](https://support.binho.io/user-guide/ascii-interface/spi-commands#begin) |
| **TXRX** | Sends/Receives data on the SPI bus. | [Details](https://support.binho.io/user-guide/ascii-interface/spi-commands#txrx) |
| **END** | Stops the SPI Controller. | [Details](https://support.binho.io/user-guide/ascii-interface/spi-commands#end) |

Feel free to jump ahead to the ASCII Command Set reference to learn the specifics of each command, or continue below to see an example of how to use these commands to achieve communication on the SPI bus.

{% page-ref page="../ascii-interface/spi-commands.md" %}

### Setting Up the SPI Bus

The first step in using the _Binho Nova Multi-Protocol USB Host adapter_ as a SPI Controller is to put the adapter into SPI mode using the [Device Operating Mode](https://support.binho.io/user-guide/using-the-device/device-settings#operating-mode) command.

```text
+MODE 0 SPI
-OK
```

Now the host adapter is in SPI communication mode. The next step is to configure the SPI bus. Here are the default settings:

* SPI Clock Frequency = 2MHz
* SPI Mode = 0 \(CPOL = 0, CPHA = 0\)
* 8 Bit Packet Size \(TXBITS\)
* MSB-First bit ordering.

Let's change the bus configuration to a 5MHz clock:

```text
SPI0 CLK 5000000
-OK
```

And let's setup the IO0 pin to use as our chip-select signal. For this device, the chip-select signal is active low, so IO0 should be a digital output that is idle high.

```text
IO0 MODE DOUT
-OK

IO0 VALUE HIGH
-OK
```

The bus is now all setup and ready to go.

### Sending and Receiving Data

Begin by putting the SPI signals \(SCK, SDI, SDO\) in their initial state simply by using the `BEGIN` command.

```text
SPI0 BEGIN
-OK
```

In order to begin transferring data over the SPI bus, one must first assert the chip-enable/chip-select signal of the peripheral device. 

```text
IO0 VALUE LOW
-OK
```

Now the chip-select signal is asserted and our target SPI peripheral device is ready to communicate.

Let's do a 5 byte transfer, which consists of sending a 1-byte command and then clocking out a 4-byte response.

```text
SPI0 TXRX 0x9F
-SPI0 RXD 0x00

SPI0 TXRX 0x00
-SPI0 RXD 0x00

SPI0 TXRX 0x00
-SPI0 RXD 0x00

SPI0 TXRX 0x00
-SPI0 RXD 0x00

SPI0 TXRX 0x00
-SPI0 RXD 0x00

IO0 VALUE HIGH
-OK

SPI0 END
-OK
```

Well that sure was tedious... and something seems strange. Looks like the peripheral device didn't return any data because we were too slow. It's fairly common for devices to have timing requirements regarding the duration of a transfer. Thankfully, for situations like this, we can use the buffer! Let's load up the buffer with our data:

```text
BUF0 CLEAR
-OK

BUF0 ADD 0x9F
-OK
```

Now let's perform the SPI transaction using BUF0 and send 5 bytes from BUF0, using IO0 as the ChipSelect pin \(active-low\):

```text
SPI0 BEGIN
-OK

IO0 VALUE LOW
-OK

SPI0 TXRX BUF0 5
-OK

IO0 VALUE HIGH
-OK

SPI0 END
-OK
```

Since we are using the buffer, the received data is also placed into the buffer. Let's read it out and see if the device responded as expected:

```text
BUF0 READ 5
-BUF0 0x00 0x04 0x7F 0x03 0x02
```

Awesome! The expected response was received. That's pretty much all it takes to use the _Binho Nova Multi-Protocol USB Host Adapter_ to speak with SPI devices. Simply clear out the buffer and repeat the process to start another transfer.

Note that this example didn't cover using IO1, but it's not hard to imagine how this could be used as an interrupt pin, or to control the chip-select pin of a second peripheral device on the SPI bus.

Feel free to take a look in the examples section to see how to use the python library to use this in an automated way:

{% page-ref page="../../python-libraries/examples/" %}

Otherwise, stick around here and move to the next page to see a similar tutorial on an I2C bus.

