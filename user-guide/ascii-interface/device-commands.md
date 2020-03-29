# Device Commands

### +ECHO

Toggles the status of the host adapter's echoing back of received characters. This is useful for manual control when the serial console application does not provide local echo functionality. Echo is turned off by default on device power-on.

**Parameters:**

This function takes no parameters.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) after toggling the status of the echo setting.

**Example Usage:**

```text
+ECHO
-OK
```

### **+PING**

Returns an ACK response, useful in testing the status of the serial connection.

**Parameters:**

This function takes no parameters.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response).

**Example Usage:**

```text
+PING
-OK
```

### **+BASE**

Gets/Sets the display base. The host adapter is able to use binary, decimal, or hex numeric bases for displaying values. The default value is `HEX` \(Base-16\).

Set display base value: `+BASE [base]`

Get display base value: `+BASE ?`

**Parameters:**

* For Binary \(Base-2\) display, set _base_ to `BIN` or `2`
* For Decimal \(Base-10\) display, set _base_ to`DEC` or `10`
* For Hexadecimal \(Base-16\) display, set _base_ to `HEX` or `16`
* To query the current setting value, set _base_ to `?`

**Response:**

Set: This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds. If an invalid value is provided, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

Get: This function returns `-BASE` followed by either `BIN`, `DEC`, or `HEX`

**Example Usage:**

```text
+BASE BIN
-OK

+BASE DEC
-OK

+BASE HEX
-OK

+BASE 2
-OK

+BASE 10
-OK

+BASE 16
-OK

+BASE OCT
-NG

+BASE ?
-BASE HEX
```

### **+LED**

Gets/Sets the color of the status LED. The RGB Status LED is user-programmable and can be especially helpful for indicating status during testing or identifying host adapters when multiple devices are being used simultaneously.

Set the LED color to preset color: `+LED [color]`

Set the LED color to RGB values: `+LED [red] [green] [blue]`

**Parameters:**

The following are valid values for _color:_

* `OFF`
* `WHITE`
* `RED`
* `GREEN` or `LIME`
* `BLUE`
* `YELLOW`
* `CYAN` or `AQUA`
* `MAGENTA` or `FUCHSIA` or `PURPLE`

The values for _red_, _green_, and _blue_ parameters are unsigned 8-bit integers from 0 to 256. Values outside of this range will be truncated to 8-bits.

**Response:**

This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds. If an invalid value is provided, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

**Example Usage:**

```text
+LED RED
-OK

+LED BLUE
-OK

+LED OFF
-OK

+LED 255 128 128
-OK

+LED 255 0
-NG

+LED ORANGE
-NG

+LED 333 128 128
-OK
```

### **+MODE**

Gets/Sets the mode of operation \(protocol\) for a given "core" of the _Binho Multi-Protocol USB Host Adapter_. The default value is `IO`.

Set display base value: `+MODE [coreIndex] [opMode]`

Get display base value: `+MODE [coreIndex] ?`

**Parameters:**

{% hint style="info" %}
The _Binho Multi-Protocol USB Host Adapter_ only has one "core". The \[`coreIndex`\] parameter is implemented to ensure the command set can be easily expanded. As such, `coreIndex` = 0 for all usage of this command.
{% endhint %}

* For IO operating mode, set _opMode_ to `IO`
* For SPI operating mode, set _opMode_ to `SPI`
* For I2C operating mode, set _opMode_ to `I2C` or `IIC`
* For 1-WIRE operating mode, set _opMode_ to `1WIRE` or `1-WIRE` or `ONEWIRE`
* For SWI operating mode, set _opMode_ to `SWI` or `SINGLEWIRE`
* For UART operating mode, set _opMode_ to `UART` or `USART` or `SERIAL`
* To query the current operating mode, set _opMode_ to `?`

**Response:**

Set: This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds. If an invalid value is provided, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

Get: This function returns `-MODE` followed by `[coreIndex]` followed by either `IO`, `SPI`, `I2C`, `UART`, `1WIRE`, or `SWI`.

**Example Usage:**

```text
+MODE 0 IO
-OK

+MODE 0 SPI
-OK

+MODE 1 SPI
-NG

+MODE 0 I2C
-OK

+MODE 0 ?
-MODE 0 I2C

+MODE 0 MODBUS
-NG
```

### **+ID**

Gets the globally-unique identifier of the host adapter. This identifier is very useful when working with multiple host adapters.

**Parameters:**

This function takes no parameters.

**Response:**

This function returns `-ID` followed by the 256-bit globally-unique device identifier. Note that the response is always displayed in base-16 \(hexadecimal\) regardless of the base setting.

**Example Usage:**

```text
+ID
-ID 0xc59bb495504e5336362e3120ff042d2a
```

### **+FWVER**

Gets the version of the firmware running on the host adapter. This can be used to determine if the latest firmware version is installed or if the host adapter should be updated.

**Parameters:**

This function takes no parameters.

**Response:**

This function returns `-FWVER` followed by the `[major].[minor].[build]` number of the firmware . Note that the response is always displayed in base-10 \(decimal\) regardless of the base setting.

**Example Usage:**

```text
+FWVER
-FWVER 1.0.0
```

### **+HWVER**

Gets the version of the host adapter hardware. This is useful to programmatically determine which host adapter product hardware is connected.

**Parameters:**

This function takes no parameters.

**Response:**

This function returns `-HWVER` followed by the `[major].[minor]` identifier of the hardware . Note that the response is always displayed in base-10 \(decimal\) regardless of the base setting.

**Example Usage:**

```text
+HWVER
-HWVER 1.0
```

### **+RESET**

Resets the host adapter, all settings return to defaults.

**Parameters:**

This function takes no parameters.

**Response:**

This function returns `-OK`. Note that immediately following the transmission of the response, the host adapter will reset, which will cause the serial connection with the computer to be lost.

{% hint style="warning" %}
Depending on the implementation of the serial console application, an Error may be raised due to the lost connection before the response to this command has been received/displayed.
{% endhint %}

**Example Usage:**

```text
+RESET
-OK
```

### **+BTLDR**

Resets the host adapter and restart in bootloader mode. The host adapter must be in bootloader mode in order to update the device firmware. While in the bootloader, the device is not available over the serial / Virtual COM port. Instead, it will enumerate as a USB Mass Storage Device. More details about firmware update can be found in the [Updating Firmware](https://support.binho.io/user-guide/using-the-device/updating-firmware) section of this guide.

**Parameters:**

This function takes no parameters.

**Response:**

This function returns `-OK` . Note that immediately following the transmission of the response, the host adapter will reset, which will cause the serial connection with the computer to be lost. The device will then enumerate as a mass storage device.

{% hint style="warning" %}
Depending on the implementation of the serial console application, an Error may be raised due to the lost connection before the response to this command has been received/displayed.
{% endhint %}

**Example Usage:**

```text
+BTLDR
-OK
```

