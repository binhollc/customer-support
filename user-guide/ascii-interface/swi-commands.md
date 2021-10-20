# SWI Commands

### BEGIN

Starts the Atmel SWI host on the given IO pin. The SWI protocol can be used on any of the IO pins, however it is especially convenient to use it on IO0 and IO2 as the internal pullup resistor can be used thus eliminating the need for an external pullup resistor.

Start the SWI host: `SWI0 BEGIN [pin] [pull]`

**Parameters:**

The `pin` parameter can be set to any of the IO pins 0, 1, 2, 3, or 4.

The `pull` parameter can be omitted if not using the internal pullup resistors or set to PULL to enable the pullup resistor (available on channels 0 and 2).

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in starting the SWI host on the desired IO pin. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
SWI0 BEGIN 0 PULL
-OK

SWI BEGIN 1
-OK
```

### TOKEN

This command is used to send either a WAKE, ONE, or ZERO token, as defined by the Atmel SWI specification. Note that due to the timing constraints of the protocol, the delay between communication with the host adapter and the PC may prohibit the construction of full data packets by stringing together ZERO and ONE tokens strung together with this command. For actual data packet transmission, use the PACKET command.

Send Token: `SWI0 TOKEN [type]`

**Parameters:**

The `type` parameter can be set to the following values:

* to send a Wake token, use `WAKE`
* to send a Zero token, use `ZERO` or `0`
* to send a One token, use `ONE` or `1`

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in sending the token. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
SWI0 TOKEN WAKE
-OK

SWI0 TOKEN ZERO
-OK

SWI0 TOKEN 1
-OK
```

### FLAG

This command transmits a flag as defined by the Atmel SWI protocol. Note that this command is simply using the TX command below to send the predefined byte value of each of the four flags.

Transmit an SWI flag: `SWI0 FLAG [flag]`

**Parameters:**

The `flag` parameter can be set to either `COMMAND`, `TRANSMIT`, `IDLE`, or `SLEEP`.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in transmitting the SWI flag. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
SWI0 FLAG COMMAND
-OK

SWI0 FLAG TRANSMIT
-OK

SWI0 FLAG IDLE
-OK

SWI0 FLAG SLEEP
-OK
```

### PACKET

This command is used to construct and transmit full data packets as defined by the Atmel SWI protocol. This command features a number of sub-commands. The typical flow is to set the `OPCODE`, set `PARAM1` and `PARAM2`, then set the `DATA`. At this point, send the packet using the `SEND` subcommand and then finally `CLEAR` it and start preparing the next packet to send.

#### OPCODE

This subcommand is used to set the OPCODE byte of the packet: `SWI0 PACKET OPCODE [code]`

**Parameters:**

The following is a complete list of the supported opcodes that can be used for the `code` parameter:

* `DERIVEKEY`
* `DEVREV`
* `GENDIG`
* `HMAC`
* `CHECKMAC`
* `LOCK`
* `MAC`
* `NONCE`
* `PAUSE`
* `RANDOM`
* `READ`
* `SHA`
* `UPDATEEXTRA`
* `WRITE`
* `GENKEY`
* `INFO`
* `TEMPSENSE`
* `VERIFY`
* `SIGN`

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the opcode. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
SWI0 PACKET OPCODE INFO
-OK
```

#### PARAM1

This subcommand is used to set the PARAM1 byte of the packet: `SWI0 PACKET PARAM1 [data]`

**Parameters:**

The `data` parameter accepts an 8-bit integer (byte) value.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the `PARAM1` value. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
SWI0 PACKET PARAM1 0xAB
-OK
```

#### PARAM2

This subcommand is used to set the PARAM2 bits of the packet: `SWI0 PACKET PARAM2 [data]`

**Parameters:**

The `data` parameter accepts a 16-bit integer value.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the `PARAM2` value. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
SWI0 PACKET PARAM2 0xABCD
-OK
```

#### DATA

This subcommand is used to set the payload data of the packet.

To load 1 byte of data: `SWI0 PACKET DATA [data]`

To load up to 64 bytes of data from BUF\[n] into the payload: `SWI0 PACKET DATA BUF[n] [count]`

**Parameters:**

The `data` parameter accepts an 8-bit integer (byte) value.

In the case of using the buffer to load data, the `count `parameter can be from 1 to 64.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the payload value. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
SWI0 PACKET DATA 0xBB
-OK

BUF0 WRITE 0 0xA1 0xA2 0xA3 0xA4 0xA5 0xA6 0xA7 0xA8
-OK

SWI0 PACKET DATA BUF0 8
-OK
```

#### CLEAR

This command clears the `OPCODE`, `PARAM1`, `PARAM2`, and `DATA` fields of the packet: `SWI0 PACKET CLEAR`

**Parameters:**

This command has no parameters.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in clearing the packet data. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
SWI0 PACKET CLEAR
-OK
```

#### SEND

This command sends the constructed packet: `SWI0 PACKET SEND`

**Parameters:**

This command has no parameters.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in sending the packet. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
SWI0 PACKET SEND
-OK
```

### TX

This command transmits a byte of data as defined by the Atmel SWI protocol.&#x20;

Transmit a byte: `SWI0 TX [data]`

**Parameters:**

The `data` parameter is the 8-bit integer (byte) value to be transmitted.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in transmitting the byte. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
SWI0 TX 0xAA
-OK
```

### RX

This command receives n bytes of data as defined by the Atmel SWI protocol.&#x20;

Transmit a byte: `SWI0 RX [count]`

**Parameters:**

The `count` parameter is the number of bytes to receive up to a max of 255.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in receiving the bytes. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
SWI0 RX 5
-SWI0 RXD 0x0A 0x0B 0x0C 0x0D 0x0E
```
