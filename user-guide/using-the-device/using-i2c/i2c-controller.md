# I2C Controller

The _Binho Nova Multi-Protocol USB Host Adapter_ can serve as an I2C bus controller device which can be configured to meet the needs of nearly any I2C bus implementation. The I2C controller supports clock frequencies from 100kHz up to 3.4MHz, which covers all common operating modes \(standard, full speed, fast, and high speed modes\). The _Binho Nova Multi-Protocol USB Host Adapter_ also features support for clock-stretching and repeated starts. There are internal pull-up resistors which can be programmatically engaged or disabled as necessary.

The I2C pins are fixed - SDA is assigned to IO0 and SCL is assigned to IO2. IO1, IO3, and IO4 are available for use when the host adapter is configured for I2C controller mode.

![](../../../.gitbook/assets/20200619_novapinout.png)

The following table gives a brief overview of the available I2C controller commands.

| Command | Description | Details |
| :--- | :--- | :--- |
| **CLK** | Gets/sets the I2C clock frequency. | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#clk) |
| **ADDR** | Gets/sets the display format of I2C addresses.  | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#addr) |
| **PULL** | Gets/sets the current state of the on-board pull-up resistors. | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#pull) |
| **SCAN** | Scans for devices on the I2C bus. | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#scan) |
| **WRITE** | Writes n bytes to a given I2C device. | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#write) |
| **REQ** | Requests n bytes from a given I2C device. | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#req) |
| **START** | Starts an I2C transmission to a given device. | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#start) |
| **END** | Ends an I2C transmission. It can be used to send a stop bit or a repeated start bit. | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#end) |
| **WHR** | Writes 0 to 1024 bytes followed by reading 0 to 1024 bytes in one transaction. | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#whr) |

Feel free to jump ahead to the ASCII Command Set reference to learn the specifics of each command, or continue below to see an example of how to use these commands to achieve communication on the I2C bus.

{% page-ref page="../../ascii-interface/i2c-commands.md" %}

### Setting Up the I2C Bus

The first step in using the _Binho Nova Multi-Protocol USB Host Adapter_ as an I2C controller is to put the adapter into I2C mode using the [Device Operating Mode](https://support.binho.io/user-guide/using-the-device/device-settings#operating-mode) command.

```text
+MODE 0 I2C
-OK
```

Now the host adapter is in I2C communication mode. The next step is to configure the I2C bus. Here are the default settings:

* I2C clock frequency = 400kHz
* I2C pull-up resistors disabled.
* I2C addresses displayed as 8-bit addresses.

Let's change the address display setting to 7-bit so that it matches the manufacturer's datasheet for the device on the bus.

{% hint style="info" %}
The ADDR setting only modifies how the address is entered/displayed in the serial console. It does not have any effect on the device addressing during I2C communication.
{% endhint %}

```text
I2C0 ADDR 7BIT
-OK
```

And let's also engage the internal pull-up resistors to eliminate the need for wiring up external resistors.

```text
I2C0 PULL EN
-OK
```

### Scanning for I2C Devices

Before even attempting communication with the peripheral device, let's make sure everything checks out. The address of the target device is 0x50.

```text
I2C0 SCAN 0x50
-I2C0 SCAN 0x50 OK
```

Great! The _Binho Nova Multi-Protocol USB Host Adapter_ found the target device on the I2C bus as indicated by the `OK` response when scanning it's address. If the addresses of the devices on the I2C bus are unknown, it's possible to scan the entire bus for devices simply by omitting the address from the scan command.

```text
I2C0 SCAN
-I2C0 SCAN 0x01 NG
-I2C0 SCAN 0x02 NG
-I2C0 SCAN 0x03 NG
[...omitting scan results from 0x04 to 0x4E...]
-I2C0 SCAN 0x4F NG
-I2C0 SCAN 0x50 OK
-I2C0 SCAN 0x51 NG
[...omitting scan results from 0x52 to 0x7C...]
-I2C0 SCAN 0x7D NG
-I2C0 SCAN 0x7E NG
-I2C0 SCAN 0x7F NG
-I2C0 SCAN OK 1 DEVICES
```

Now it's time to start communicating with the devices on the bus!

### Sending and Receiving Data

Let's begin to define an I2C transaction that will be carried out by the host adapter. Every I2C transaction must begin with a start bit followed by the address of the target device. When performing an I2C Write operation, the `START` command must be issued manually:

```text
I2C0 START 0x50
-OK
```

Now let's queue up a couple of bytes of data to write:

```text
I2C0 WRITE 0xAA
-OK

I2C0 WRITE 0xBB
-OK
```

```text
I2C0 END
-OK
```

Unlike SPI controller mode, the I2C transactions are only sent out on the bus after sending the `END` command. This is great because it means there's no rush to enter data in the serial console when manually interacting with I2C devices.

Requesting data from the I2C peripheral device is also quite simple. When requesting data from the peripheral device, there's no need to manually issue the `START` command, it's handled automatically:

```text
I2C0 REQ 0x50 2
-I2C0 RXD 0x42 0x36
```

Sometimes reading data requires writing a value to a particular location and then sending a repeated start bit to read out the desired value. Here's an example of such a transaction:

```text
I2C0 ADDR 8BIT
-OK

I2C0 START 0xF8
-OK

I2C0 WRITE 0xA0
-OK

I2C0 END R
-OK

I2C0 REQ 0xF8 3
-I2C0 RXD 0x00 0xA5 0x10
```

It's also possible to write and request data on the I2C bus using the buffer. This can be helpful when writing/requesting more than just a few bytes at a time. Let's use BUF0 to help send 8 bytes on the I2C bus:

```text
BUF0 CLEAR
-OK

BUF0 WRITE 0 0xA0 0xA1 0xA2 0xA3 0xA4 0xA5 0xA6 0xA7
-OK

I2C0 START 0xA0
-OK

I2CO WRITE BUF0 8
-OK

I2C0 END
-OK
```

The buffer can also be used to receive requested data from the I2C bus as follows:

```text
BUF0 CLEAR
-OK

I2C0 REQ 0xA0 BUF0 4
-OK

BUF0 READ 4
-BUF0 0x0C 0x0E 0x10 0x12
```

That's all there is to I2C communication with the _Binho Nova Multi-Protocol USB Host Adapter_. You can take some time to learn all the details about the I2C commands or skip ahead and look at some of the examples that use I2C in an automated fashion:

{% page-ref page="../../../python-libraries/python-wrapper/examples/" %}

{% hint style="success" %}
For use cases where a lot of data is being read/written and speed is important, the WHR command should be utilized to minimize the amount of communication between the Nova and the host PC. You can learn all the details about this [here](https://support.binho.io/user-guide/ascii-interface/i2c-commands#whr).
{% endhint %}

Next up we'll be doing a walk-through using I2C Peripheral.

