# 1-WIRE Commands

### BEGIN

Starts the 1-Wire Host on the given IO pin. The 1-Wire protocol can be used on any of the IO pins, however it is especially convenient to use it on IO0 and IO2 as the internal pull-up resistor can be used thus eliminating the need for an external pull-up resistor.

Start the 1-WIRE Host: `1WIRE0 BEGIN [pin] [pull]`

**Parameters:**

The `pin` parameter can be set to any of the IO pins `0`, `1`, `2`, `3`, or `4`.

The `pull` parameter can be omitted if not using the internal pull-up resistors or set to `PULL` to enable the pull-up resistor (available on channels 0 and 2).

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in starting the 1-Wire host on the desired IO pin. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
1WIRE0 BEGIN 0 PULL
-OK

1WIRE BEGIN 1
-OK
```

### RESET

Resets the 1-Wire devices on the bus and prepares them to receive commands. This is needed before communicating with any device on the bus.

Reset: `1WIRE0 RESET`

**Parameters:**

This command has no parameters.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the device on the 1-Wire bus asserted a presence pulse as a result of the result on the 1-WIRE bus. If the command fails or no device asserts a pulse after the reset, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
1WIRE0 RESET
-OK
```

### WRITE

This command writes a byte to the selected 1-WIRE device.

Write byte: `1WIRE0 WRITE [data] [power]`

**Parameters:**

The `data` parameter is the 8-bit integer (byte) to write on the bus.

The `power` parameter indicates whether or not to leave the power applied to the device after the write has completed. This parameter is optional and will turn off power by default. To leave power on after the write, set power equal to `PWR`.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in writing the data. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
1WIRE0 WRITE 0xAB
-OK

1WIRE0 WRITE 0xAC PWR
-OK
```

### READ

This command reads a byte from the selected 1-WIRE device.

Read byte: `1WIRE0 READ`

**Parameters:**

This command has no parameters.

**Response:**

This function returns a Data Response containing the received byte in the format of `1WIRE0 READ [data]`&#x20;

**Example Usage:**

```
1WIRE0 READ
-1WIRE0 READ 0xAB
```

### SELECT

This command is used to select the device that was found as a result of the most recent `SEARCH` command.

Reset: `1WIRE0 SELECT`

**Parameters:**

This command has no parameters.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in sending the SELECT command. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
1WIRE0 SELECT
-OK
```

### SKIP

This command is used to skip the device selection process.

Reset: `1WIRE0 SKIP`

**Parameters:**

This command has no parameters.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in sending the SKIP command. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
1WIRE0 SKIP
-OK
```

### SEARCH

Searches the 1-WIRE bus for the next device.

Search: `1WIRE0 SEARCH [searchMode]`

**Parameters:**

This command has one parameter, `searchmode`:

* Set `searchmode` to `RESET` to restart the search from the first device.
* Set `searchmode` to `COND` to begin a conditional search.
* Set `searchmode` to the family code of a 1-Wire device to search for devices of the same device family.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in searching for a device on the bus. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
1WIRE0 SEARCH
-OK

1WIRE0 SEARCH RESET
-OK

1WIRE0 SEARCH COND
-OK

1WIRE0 SEARCH 0xAA
-OK
```

### DEPOWER

Power down the 1-WIRE bus.

Depower: `1WIRE0 DEPOWER`

**Parameters:**

This command has no parameters.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in depowering the bus. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
1WIRE0 DEPOWER
-OK
```

### ADDR

Gets the address of the device found using the SEARCH command.

Get address: `1WIRE0 ADDR ?`

**Parameters:**

This command has no parameters.

**Response:**

This function returns a Data Response containing the address of the device found using the SEARCH command.

**Example Usage:**

```
1WIRE0 ADDR ?
-1WIRE0 ADDR 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00
```

### WHR

Performs an optional SKIP or SELECT command, followed by writing 0 to 1024 bytes, and then reading 0 to 1024 bytes.

Syntax: `1WIRE0 WHR [cmd] [bytesToRead] [bytesToWrite] [hexPayload]`

**Parameters:**

The `cmd` parameter instructs Nova to optionally begin the transaction with a SKIP or SELECT command. The possible values for this parameter are `SKIP`, `SELECT`, or `NONE`.

The `bytesToRead` parameter indicates the number of bytes to read after writing the hexPayload to the 1-Wire bus. This value can be from `0` to `1024`.

The `bytesToWrite` parameter indicates the number of bytes to read to the 1-Wire bus. This value can be from `0` to `1024` and must match the length of the `hexPayload` parameter.

The `hexPayload` parameter is the data that will be written to the 1-Wire bus. This parameter should be entered as a string of hex values without a leading "0x" and no spaces. The length must match the bytesToWrite parameter.

**Response:**

This function returns either `OK` or `NG` when the WHR command is used only to write data (_bytesToRead_ = 0) to the 1-Wire bus. When the WHR command is used to perform a read operation, the response will contain the requested number of data bytes read from the bus, or `NG` indicating that command failed to execute successfully.

**Example Usage:**

```
1WIRE0 WHR SKIP 4 3 F00000
-1WIRE0 RXD DEADBEEF
```

{% hint style="success" %}
We've produced a video tutorial demonstrating how to use this command [here](https://www.youtube.com/watch?v=cAAF-zOJbJo).
{% endhint %}
