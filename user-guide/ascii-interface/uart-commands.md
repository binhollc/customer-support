# UART Commands

### CFG

Gets/sets the UART configuration by passing a configuration string. This allows for the configuration of databits, parity, and stopbits using a single command. The default configuration is 8 (databits), N (no parity), 1 (stopbits).

Set the UART configuration: `UART0 CFG [databits] [parity] [stopbits]`

Get the current UART configuration: `UART0 CFG ?`

**Parameters:**

The `databits` parameter can be set to 5, 6, 7, or 8.

The `parity` parameter can be set as follows:

* To set the parity to NONE, use `NONE`, `0`, or `N`
* To set the parity to ODD, use `ODD`, `1`, or `O`
* To set the parity to EVEN, use `EVEN`, `2`, or `E`

The `stopbits` parameter can be set to either `1` or `2`.

**Response:**

Set: This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the configuration. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

Get: This function returns `-UART0` followed by `CFG` followed by the current databits, parity, and stopbits values separated by whitespaces.

**Example Usage:**

```
UART0 CFG ?
-UART0 CFG 8 N 1

UART0 CFG 8 1 2
-OK
```

### BAUD

Gets/sets the baudrate of the UART connection. The default baudrate is 9600 bps.

Set the UART baudrate: `UART0 BAUD [rate]`

Get the current UART baudrate: `UART0 BAUD ?`

**Parameters:**

The `rate` parameter can be set to from 300 to 1000000 bps.

**Response:**

Set: This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the baudrate. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

Get: This function returns `-UART0` followed by `BAUD` followed by the current baudrate.

**Example Usage:**

```
UART0 BAUD ?
-UART0 BAUD 9600

UART0 BAUD 115200
-OK
```

### DATABITS

Gets/sets the number of databits for the UART connection. The default databits setting is 8.

Set the number of databits: `UART0 DATABITS [bits]`

Get the current number of databits: `UART0 DATABITS ?`

**Parameters:**

The `bits` parameter can be set to 5, 6, 7, or 8.

**Response:**

Set: This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the number of databits. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

Get: This function returns `-UART0` followed by `DATABITS` followed by the current number of databits.

**Example Usage:**

```
UART0 DATABITS ?
-UART0 DATABITS 8

UART0 DATABITS 7
-OK
```

### PARITY

Gets/sets the parity bit configuration for the UART connection. The default parity setting is None.

Set the UART parity: `UART0 PARITY [par]`

Get the current UART parity: `UART0 PARITY ?`

**Parameters:**

The `par` parameter can be set as follows:

* To set the parity to NONE, use `NONE`, `0`, or `N`
* To set the parity to ODD, use `ODD`, `1`, or `O`
* To set the parity to EVEN, use `EVEN`, `2`, or `E`

**Response:**

Set: This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the desired parity configuration. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

Get: This function returns `-UART0` followed by `PARITY` followed by the current parity setting.

**Example Usage:**

```
UART0 PARITY ?
-UART0 PARITY NONE

UART0 PARITY E
-OK

UART0 PARITY ?
-UART0 PARITY EVEN
```

### STOPBITS

Gets/sets the number of stop bits for the UART connection. The default number of stopbits is 1.

Set the number of stopbits: `UART0 STOPBITS [bits]`

Get the current number of stopbits: `UART0 STOPBITS ?`

**Parameters:**

The `bits` parameter can be set to either 1 or 2.

**Response:**

Set: This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the desired number of stopbits. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

Get: This function returns `-UART0` followed by `STOPBITS` followed by the current number of stopbits.

**Example Usage:**

```
UART0 STOPBITS ?
-UART0 STOPBITS 1

UART0 STOPBITS 2
-OK
```

### ESC

Gets/sets the escape sequence that can be used to break out of the UART bridge mode. The default escape sequence is `+++UART0`.

Set the escape sequence: `UART0 ESC [sequence]`

Get the current escape sequence: `UART0 ESC ?`

**Parameters:**

The `sequence` parameter must be a string of 6 to 16 characters.

**Response:**

Set: This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the desired escape sequence. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

Get: This function returns `-UART0` followed by `ESC` followed by the current escape sequence.

**Example Usage:**

```
UART0 ESC ?
-UART0 ESC +++UART0

UART0 ESC +!+!+!456
-OK
```

### BEGIN

Starts the UART bridge. The adapter will remain in UART bridge mode until the escape sequence is sent from the host computer.

Start the UART bridge: `UART0 BEGIN`

**Parameters:**

This command takes no parameters.

**Response:**

Set: This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in opening the UART bridge. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```
UART0 ESC ?
-UART0 ESC +++UART0

UART0 BEGIN
-OK

Hello!
Ascii Bridge Content

+++UART0
-OK

```
