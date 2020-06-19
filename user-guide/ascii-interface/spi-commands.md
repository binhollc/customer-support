# SPI Commands

### CLK

Gets/sets the SPI clock frequency. The SPI clock frequency can be configured from 500kHz to 12MHz in 1kHz steps. The default clock frequency is 2MHz. 

Set the SPI clock frequency: `SPI0 CLK [frequency]`

Get the current SPI clock frequency: `SPI0 CLK ?`

**Parameters:**

The `frequency` parameter can be set to any frequency from 500000 Hz to 12000000 Hz in 1000 Hz steps.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the updated SPI clock frequency. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
SPI0 CLK ?
-SPI0 CLK 2000000

SPI0 CLK 5000000
-OK
```

### ORDER

Gets/sets the SPI data bit order. The SPI bus bit order can be configured to either LSB first or MSB first. The default bit order setting is MSB first.

Set the SPI bit order: `SPI0 ORDER [bitorder]`

Get the current SPI bit order: `SPI0 ORDER ?`

**Parameters:**

The `bitorder` parameter shall be set as follows:

* to configure the SPI bus for LSB first, set it to `LSB` or `LSBFIRST`
* to configure the SPI bus for MSB first, set it to `MSB` or `MSBFIRST`

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the bit order of the SPI bus. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
SPI0 ORDER ?
-SPI0 ORDER MSBFIRST

SPI0 ORDER LSBFIRST
-OK
```

### MODE

Gets/sets the SPI mode. Modes 0, 1, 2, or 3 are determined by CPOL and CPHA. It's possible to set the mode directly, or to configure CPOL and CPHA independently to determine the mode setting. The default mode is 0.

Set the SPI bus mode: `SPI0 MODE [spiMode]`

Get the current SPI bus mode setting: `SPI0 MODE ?`

**Parameters:**

The `spiMode` parameter shall be set to 0, 1, 2, or 3 based on the desired clock polarity and phase settings:

| **SPI Mode** | **CPOL** | **CPHA** |
| :---: | :---: | :---: |
| 0 | 0 | 0 |
| 1 | 0 | 1 |
| 2 | 1 | 0 |
| 3 | 1 | 1 |

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the mode of the SPI bus. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
SPI0 MODE ?
-SPI0 MODE 0

SPI0 MODE 3
-OK

SPI0 CPOL ?
-SPI0 CPOL 1

SPI0 CPHA ?
-SPI0 CPHA 1
```

### CPOL

Gets/set the SPI Clock Polarity. Clock polarity indicates the idle condition of the clock signal. This is related to the SPI mode. It's possible to configure the CPOL setting directly by using this command, or indirectly by using the MODE command. The default value of CPOL is 0.

Set the SPI Clock Polarity setting: `SPI0 CPOL [polarity]`

Get the current SPI Clock Polarity setting: `SPI0 CPOL ?`

**Parameters:**

The `polarity` parameter shall be set as follows:

* to configure the SPI clock polarity for idle low, use `0`.
* to configure the SPI clock polarity for idle high, use `1`.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the clock polarity of the SPI bus. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
SPI0 CPOL ?
-SPI0 CPOL 0

SPI0 CPOL 1
-OK
```

### CPHA

Gets/sets the SPI Clock Phase. Clock Phase indicates when the data is valid in relation to the clock edges. This is related to the SPI mode. It's possible to configure the CPHA setting directly by using this command, or indirectly by using the MODE command. The default value of CPHA is 0.

Set the SPI Clock Phase setting: `SPI0 CPHA [phase]`

Get the current SPI Clock Phase setting: `SPI0 CPHA ?`

**Parameters:**

The `phase` parameter shall be set as follows:

* to configure the SPI clock phase for data changing on the trailing edge, use `0`.
* to configure the SPI clock phase for data changing on the leading edge, use `1`.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the clock phase of the SPI bus. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
SPI0 CPHA ?
-SPI0 CPHA 0

SPI0 CPHA 1
-OK
```

### TXBITS

Gets/sets the number of bits per transmission. The SPI bus can be configured to either 8 or 16 bits per transfer. The default setting is 8 bits per transfer.

Set the SPI bits per transmission setting: `SPI0 TXBITS [bitcount]`

Get the current SPI bits per transmission setting: `SPI0 TXBITS ?`

**Parameters:**

The `bitcount` parameter shall be set as follows:

* to configure the SPI bus transmission for 8-bit transmissions, use `8`.
* to configure the SPI bus transmission for 16-bit transmissions, use `16`

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the number of bits per transmission for the SPI bus. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
SPI0 TXBITS ?
-SPI0 TXBITS 8

SPI0 TXBITS 16
-OK
```

### BEGIN

Starts the SPI Controller. This command must be issued before sending/receiving any data on the SPI bus.

Start the SPI transmission: `SPI0 BEGIN`

**Parameters:**

This command has no parameters.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in starting the SPI controller. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
SPI0 BEGIN
-OK
```

### TXRX

Sends/Receives data on the SPI bus. This transfer can be done either directly, or by using a buffer.

To transfer data using the buffer: `SPI0 TXRX BUF[n] [count]`

To transfer data directly: `SPI0 TXTX [data]`

{% hint style="warning" %}
Before sending the **TXRX** command, be sure you've selected the target peripheral device on your SPI bus. The CS pin of the target device should be driven to the correct logical value using the [IO commands](https://support.binho.io/user-guide/ascii-interface/io-commands).
{% endhint %}

**Parameters:**

When using the buffer method, `count` is the number of bytes to transfer from 1 to 256.

When using direct transfer, `[data]` is the 8-bit or 16-bit payload to transfer. The size of data depends on the `TXBITS` setting.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in transferring the data on the SPI bus. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
IO0 MODE DOUT
-OK

SPI0 BEGIN
-OK

IO0 VALUE LOW
-OK

SPI0 TXRX 0xAB
-SPI0 RXD 0xFF

IO0 VALUE HIGH
-OK

SPI0 END
-OK

BUF0 WRITE 0 0xAA 0xBB 0xCC
-OK

SPI0 BEGIN
-OK

IO0 VALUE LOW
-OK

SPI0 TXRX BUF0 3
-OK

IO0 VALUE HIGH
-OK

SPI0 END
-OK
```

### END

Stops the SPI controller. This command ends the SPI session until a BEGIN command restarts the SPI controller.

Stop the SPI controller: `SPI0 END`

**Parameters:**

This command has no parameters.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in stopping the SPI controller. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
SPI0 END
-OK
```



