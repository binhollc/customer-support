# Buffer Commands

{% hint style="info" %}
All Buffer Commands must be prefaced by `BUF[n]` , where _n_ is the index of the internal buffer. The _Binho Multi-Protocol USB Host Adapter_ features just one internal buffer, therefore the only valid value for _n_ at this time is _n_=0.
{% endhint %}

### CLEAR

Clears the contents of the entire 256-byte buffer.

**Parameters:**

This function takes no parameters.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) after clearing the buffer.

**Example Usage:**

```
BUF0 CLEAR
-OK
```

### ADD

Adds a single byte of data into the buffer and increments the pointer index by +1.

Add value to buffer: `BUF[n] ADD [data]`

**Parameters:**

This function takes one parameter, _data_. This is the 8-bit unsigned integer (byte) value that will be written to the buffer. Values outside of this range will be truncated to 8 bits.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) after adding the byte the buffer if the command succeeds. If an invalid parameter is provided, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
BUF0 ADD 0xAA
-OK

BUF0 READ 1
-BUF0 0xAA
```

### WRITE

Writes _n_ bytes of data into the buffer, up to a maximum of 32 bytes at a time.

Add _n_ bytes into the buffer beginning at _startIndex_: `BUF[n] WRITE [startIndex] [data_0] [data_1] ... [data_n]`

**Parameters:**

_startIndex _is the 8-bit integer index of the location that the first byte should be written to the buffer.

_data\_n _is the 8-bit integer (byte) that will be written to the buffer. Values outside of this range will be truncated to 8 bits.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) after writing the data into the buffer if the command succeeds. If an invalid value is provided, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
BUF0 WRITE 0 10 15 20
-OK

BUF0 READ 4
-BUF0 10 15 20 0
```

### READ

Reads n bytes of data from the buffer beginning at the start of the buffer, up to a maximum of 256 bytes at a time (the entire buffer).

`BUF[n] READ [byteCount]`

**Parameters:**

_byteCount _is the 8-bit integer number of bytes to read from the buffer. Valid range is from 1 to 256 bytes.

**Response:**

This function returns a [Data Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#data-response) if the read command was successful. If an invalid value is provided, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
BUF0 READ 16
-BUF0 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00
```
