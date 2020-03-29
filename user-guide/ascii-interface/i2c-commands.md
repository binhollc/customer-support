# I2C Commands

### CLK

Gets/sets the I2C clock frequency. The I2C clock frequency can be configured from 100kHz to 3.4MHz in 1kHz steps. The default clock frequency is 400kHz. 

Set the I2C clock frequency: `I2C0 CLK [frequency]`

Get the current I2C clock frequency: `I2C0 CLK ?`

**Parameters:**

The `frequency` parameter can be set to any frequency from 100000 Hz to 3400000 Hz in 1000 Hz steps.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the updated I2C clock frequency. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
I2C0 CLK ?
-I2C0 CLK 400000

I2C0 CLK 3400000
-OK
```

### ADDR

Gets/sets the format of I2C addresses. The I2C clock frequency can be configured for either 7-bit or 8-bit address formats. This does not impact I2C bus perfomance and is only provided for convenience to match the formatting of manufacturer datasheets.  The default address setting is 8-bit display.

Set the format of I2C address display: `I2C0 ADDR [format]`

Get the current format of I2C address display: `I2C0 ADDR ?`

**Parameters:**

The `format` parameter can be set to either `7BIT` or `8BIT`.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the updated I2C address format. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
I2C0 ADDR ?
-I2C0 ADDR 8BIT

I2C0 ADDR 7BIT
-OK
```

### PULL

Gets/sets the current state of the on-board pull-up resistors on the I2C SCL and I2C SDA signals. The internal pull-up resistors are disabled by default.

Set the state of the pull-up resistors: `I2C0 PULL [state]`

Get the current state of the pull-up resistors: `I2C0 PULL ?`

**Parameters:**

The `state` parameter can be controlled by the following:

* to engage the pull-up resistors, send `1`, `ON`, or `EN`.
* to disengage the pull-up resistors, send `0`, `OFF`, or `DIS`.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the update the state of the pull-up resistors. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
I2C0 PULL ?
-I2C0 PULL DISABLED

I2C0 PULL EN
-OK

I2C0 PULL ?
-I2C0 PULL ENABLED
```

### SCAN

Scans for devices on the I2C bus. This command can be used to scan for a particular device address, or for all possible addresses on the bus.

Scan the bus for devices: `I2C0 SCAN`

Scan the bus for a device on a particular address: `I2C0 SCAN [address]`

**Parameters:**

The `address` parameter can be set to any valid 7-bit I2C device address.

**Response:**

For each address scanned, the command returns `-I2C0 SCAN [address]` followed by either `OK` if a device was found with that address, or `NG` if no device was found. In the case of scanning the entire bus for device, upon the completion of the scan a summary will be reported: `-I2C0 SCAN OK [n] DEVICES` where `n` indicates the number of devices discovered on the bus.

**Example Usage:**

```text
I2C0 SCAN 0xC2
-I2C0 SCAN 0xC2 NG

I2C0 SCAN
-I2C0 SCAN 0x02 NG
-I2C0 SCAN 0x04 NG
-I2C0 SCAN 0x06 NG
-I2C0 SCAN 0x08 NG
-I2C0 SCAN 0x0A NG
-I2C0 SCAN 0x0C NG
[...]
-I2C0 SCAN 0xF8 NG
-I2C0 SCAN 0xFA NG
-I2C0 SCAN 0xFC NG
-I2C0 SCAN 0xFE NG
-I2C0 SCAN OK 0 DEVICES
```

### WRITE

Writes n bytes to a given I2C device. This command can write a single byte at a time when given the data directly, or up to 256 bytes at a time when using a buffer. This command can be used after starting the I2C transaction with the START command.

Write a single byte to an I2C device: `I2C0 WRITE [data]`

Write up to 256 bytes from a buffer: `I2C0 WRITE BUF[n] [count]`

**Parameters:**

The `data` parameter can be any valid 8-bit integer \(byte\) of data to send.

The `count` parameter is the number of bytes to be sent from the buffer, from 1 to 256.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in writing the data to the I2C device. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
I2C0 START 0xC2
-OK

I2C0 WRITE 0xAB
-OK

I2C0 END R
-OK

BUF0 WRITE 0xAA 0xAB 0xAC 0xAD
-OK

I2C0 START 0xC2
-OK

I2C0 WRITE BUF0 4
-OK

I2C0 END
-OK
```

### REQ

Requests n bytes from a given I2C device. The maximum number of bytes in a single request is 256.

Request n bytes: `I2C0 REQ [address] [count]`

Request n bytes read into a buffer: `I2C0 REQ [address] BUF[n] [count]`

**Parameters:**

The `address` parameter can be any valid 8-bit I2C device address.

The `count` parameter is the number of bytes to be requested, from 1 to 256.

**Response:**

This function returns a Data Response with the received bytes in the format of `I2C0 RXD` followed by the received bytes separated by spaces. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
I2C0 REQ 0xC2 4
-I2C0 RXD 0xAB 0xAC 0xAD 0xAE

I2C0 REQ 0xFF 1
-NG

BUF0 CLEAR
-OK

I2C0 REQ 0xC2 BUF0 4
-OK

BUF0 READ 4
-BUF0 0xAB 0xAC 0xAD 0xAE
```

### START

Starts an I2C transmission to a given device by sending a start bit on the I2C bus. 

Start an I2C transmission: `I2C0 START [address]`

**Parameters:**

The `address` parameter can be any valid 8-bit I2C device address.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in starting the transaction. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
I2C0 START 0xC2
-OK
```

### END

Ends an I2C transmission. It can be used to send a stop bit or a repeated start bit.

Send stop bit: `I2C0 END`

Send a repeated start bit: `I2C0 END R`

**Parameters:**

This command has no parameters.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in sending the desired bit. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
I2C0 END
-OK

I2C0 END R
-OK
```

