# I2C Peripheral

The _Binho Nova Multi-Protocol USB Host Adapter_ can serve as an I2C bus peripheral device. Due to the nature of peripheral devices needing to promptly respond to the I2C controller on the bus, the _Binho Nova_ employs a novel approach to handling the communication on the device side to eliminate the need for communicating with the host PC during the I2C transaction. This approach entails emulating a peripheral device register bank which can be configured by the host PC. The host PC will also receive interrupts from Binho Nova when it's operating as an I2C peripheral device to inform it of communication events. This will be covered below in further detail.

The I2C peripheral device supports clock frequencies from 100kHz up to 3.4MHz, which covers all common operating modes (standard, full speed, fast, and high speed modes). The _Binho Nova Multi-Protocol USB Host Adapter_ also features support for clock-stretching and repeated starts. There are internal pull-up resistors which can be programmatically engaged or disabled as necessary even when operating as an I2C peripheral device.

The I2C pins are fixed - SDA is assigned to IO0 and SCL is assigned to IO2. IO1, IO3, and IO4 are available for use when the host adapter is configured for I2C peripheral mode. This is particularly useful to implement interrupt or alert signals.

![](../../../.gitbook/assets/20200619\_novaPinout.png)

The following table gives a brief overview of the available I2C Peripheral Commands.

{% hint style="warning" %}
The following use of Master/Slave terminology is considered obsolete. Controller/Peripheral is now used. These firmware commands will be deprecated in an upcoming firmware release.
{% endhint %}

| Command             | Description                                                             | Details                                                                                     |
| ------------------- | ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **PULL**            | Gets/sets the current state of the on-board pull-up resistors.          | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#pull)            |
| **SLAVE**           | Gets/sets the peripheral device Address                                 | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#slave)           |
| **SLAVE MODE**      | Gets/sets the mode of operation of peripheral device                    | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#slave-mode)      |
| **SLAVE REGCNT**    | Gets/sets the number of registers in the device register bank           | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#slave-regcnt)    |
| **SLAVE REG**       | Gets/sets the value contained in a register in the device register bank | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#slave-reg)       |
| **SLAVE READMASK**  | Gets/sets the read permissions for the bits in a given register         | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#slave-readmask)  |
| **SLAVE WRITEMASK** | Gets/sets the write permissions for the bits in a given register        | [Details](https://support.binho.io/user-guide/ascii-interface/i2c-commands#slave-writemask) |

Feel free to jump ahead to the ASCII Command Set reference to learn the specifics of each command, or continue below to see examples of how to use these commands to implement I2C peripheral device behaviors using a _Binho Nova_.

{% content-ref url="../../ascii-interface/i2c-commands.md" %}
[i2c-commands.md](../../ascii-interface/i2c-commands.md)
{% endcontent-ref %}

{% hint style="success" %}
I2C peripheral functionality was introduced with firmware version 0.2.0. Please ensure your _Binho Nova_ has the latest device firmware in order to use the features on this page. You can learn how to update your device firmware [here](https://support.binho.io/software/how-to/how-to-update-firmware).
{% endhint %}

### Initializing the I2C Peripheral Device

The first step in using the _Binho Nova Multi-Protocol USB Host Adapter_ as an I2C peripheral is to put the adapter into I2C mode using the [Device Operating Mode](https://support.binho.io/user-guide/using-the-device/device-settings#operating-mode) command.

```
+MODE 0 I2C
-OK
```

Now the host adapter is in I2C communication mode. The next step is to instruct the Binho Nova to behave as an I2C peripheral device. This is achieved by assigning it a peripheral device address to respond to as shown below, where `0xA0` is assigned as the peripheral address.

```
I2C0 SLAVE 0xA0
-OK
```

At this point, the _Binho Nova_ will begin responding whenever an I2C controller device sends it's address out on the bus.

{% hint style="info" %}
#### A Quick Note About Pull Up Resistors

Typically, the I2C controller on the bus is responsible for supplying pullup resistors on the SCL and SDA signals, however during system development, it's sometimes easier to use the pullup resistors integrated in the _Binho Nova_. As such, it's still possible to enable the integrated resistors even when the _Nova_ is behaving as an I2C peripheral.
{% endhint %}

### Configuring I2C Peripheral Device Behavior

As mentioned above, the _Binho Nova_ emulates a virtual register bank in the device firmware so that it does not need to communicate with the host computer during I2C transactions. This allows the I2C bus communication to take place at normal speeds without delays while the host adapter waits for data or instructions on how to respond from a host PC. This is important so that the behavior of the system being tested does not change between interacting with real I2C devices compared to a _Binho Nova_ operating as a peripheral device.

The _Binho Nova_ I2C peripheral device has two modes of operation which allow it to behave like some of the most common I2C peripheral devices:

#### 1) USEPTR - Use Pointer Register

In this mode of operation, a "pointer register" is used to keep track of the current register index in the device memory bank. This pointer will auto-increment each time a register is read from or written to. This allows successive reads and writes. This is a common approach for advanced I2C devices with rich configuration settings and multiple data parameters that are interesting to be sampled/read at the same time. In this mode, the I2C controller can set the value in the pointer register by performing a 1-byte Write of the desired register index before performing an I2C read operation. This is typically referred to as a "read register" operation. Note that this is the default mode upon I2C peripheral initialization.

#### 2) STARTZERO - Start At Zero

In this mode of operation, all I2C read and write transactions will always begin from the 0th register in the memory bank. This is very common for simple devices which just have a few registers.

The following code snippet demonstrates how to configure and query the I2C peripheral mode using the `MODE` command:

```
I2C0 SLAVE MODE ?
-I2C0 SLAVE MODE USEPTR

I2C0 SLAVE MODE STARTZERO
-OK

I2C0 SLAVE MODE ?
-I2C0 SLAVE MODE STARTZERO

I2C0 SLAVE MODE USEPTR
-OK

I2C0 SLAVE MODE ?
-I2C0 SLAVE MODE USEPTR
```

### Configuring I2C Peripheral Device Registers

By default, the _Binho Nova_ I2C peripheral device emulates a memory bank of 256 x 8-bit registers, and all bits of all registers are granted read and write access. The default value of all registers is 0xFF, and the Pointer Register is set to index 0.

The number of registers in the bank can be changed from 1 to 256 using the `REGCNT` command as show below:

```
I2C0 SLAVE REGCNT ?
-I2C0 SLAVE REGCNT 0x100

I2C0 SLAVE REGCNT 8
-OK

I2C0 SLAVE REGCNT ?
-I2C0 SLAVE REGCNT 8
```

The value of each register can be read or written to using the `REG` command. The snippet below demonstrates how to read the current value of register 0x00 and then write the value of 0xCD to register 0x0B:

```
I2C0 SLAVE REG 0x00 ?
-I2C0 SLAVE REG 0x00 0xFF

I2C0 SLAVE REG 0x0B 0xCD
-OK

I2C0 SLAVE REG 0x0B ?
-I2C0 SLAVE REG 0x0B 0xCD
```

Note that this command can also be used to read and write the Pointer register as demonstrated below:

```
I2C0 SLAVE REG PTR ?
-I2C0 SLAVE REG PTR 0x00

I2C0 SLAVE REG PTR 0x05
-OK

I2C0 SLAVE REG PTR ?
-I2C0 SLAVE REG PRT 0x05
```

### Configuring I2C Peripheral Device Register Access

It's very common that I2C peripheral devices will have some data which is read-only. Additionally some devices implement bits which are write-only, and always read as 0. These are typically referred to as _Strobe_ bits. The _Binho Nova_ allows Read and Write access controls to be applied at the individual bit level in order to achieve these behaviors.

{% hint style="info" %}
The Host PC will always have the ability to read and write all registers. The access controls for the registers are only applicable to communication over the I2C bus. For example, a register that is configured to be Read-Only will still be write-able by the host PC without needing to change the access settings.
{% endhint %}

Each register in the peripheral device memory bank has a corresponding `READMASK` and `WRITEMASK`, which are used to configure the bit-level access.&#x20;

For example, a register with a `READMASK` of 0x3F means that the bits 5 through 0 are readable, and bits 6 and 7 would always be read as 0. The following snippet demonstrates using the `READMASK` command to configure the read access for the bits of register 0:

```
I2C0 SLAVE READMASK 0x00 0x3F
-OK

I2C0 SLAVE READMASK 0x00 ?
-I2C0 SLAVE READMASK 0x00 0x3F
```

Now for a second example, a register with a `WRITEMASK` of 0xF0 means that the bits 7 through 4 are write-able, and bits 3 through 0 cannot be written to. Any values written to bits 3 through 0 will be discarded. The following snippet demonstrates using the `WRITEMASK` command to configure the write access for the bits of register 0:

```
I2C0 SLAVE WRITEMASK 0x00 0xF0
-OK

I2C0 SLAVE WRITEMASK 0x00 ?
-I2C0 SLAVE WRITEMASK 0x00 0xF0
```

By default, `READMASK` and `WRITEMASK` for all registers are 0xFF, meaning that all bits in the registers are both read-able and write-able.
