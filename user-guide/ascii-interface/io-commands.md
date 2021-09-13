# IO Commands

### MODE

Gets/sets the current mode of a specific IO signal. Note that not all modes of operation are available on all pins. Please see the pin out diagram on the [Using IO](https://support.binho.io/user-guide/using-the-device/using-io) page for reference.

Set current mode value: `IO[n] MODE [mode]`

Get current mode value: `IO[n] MODE ?`

**Parameters:**

This function takes on parameter, _`mode`_:

* To set IO pin mode to Digital Input, use `DIN`
* To set IO pin mode to Digital Output, use `DOUT`
* To set IO pin mode to Analog Input, use `AIN`
* To set IO pin mode to Analog Output, use `AOUT`
* To set IO pin mode to PWM Output, use `PWM`

**Response:**

Set: This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in setting the supplied mode of operation to the specified IO pin. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

Get: This function returns `-IO[n]` followed by `MODE` followed by the current mode.

**Example Usage:**

```text
INT0 MODE DOUT
-OK

IO0 MODE ?
-IO0 MODE DOUT

IO1 MODE PWM
-NG
```

### INT

Gets/sets the interrupt configuration of a specific IO signal. Note that not all IO signals are capable of supporting hardware interrupts. Please see the pin out diagram on the [Using IO](https://support.binho.io/user-guide/using-the-device/using-io) page for reference.

Set Interrupt configuration: `IO[n] INT [trigger]`

Get Interrupt configuration: `IO[n] INT ?`

**Parameters:**

This function takes on parameter, _`trigger`_:

* To configure interrupt on rising edge, use `RISE`
* To configure interrupt on falling edge, use `FALL`
* To configure interrupt on either edge, use `CHANGE`
* To turn off interrupts on this pin, use `NONE`

**Response:**

Set: This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in configuring the interrupt on the specified IO pin. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

Get: This function returns `-IO[n]` followed by `INT` followed by the current interrupt configuration.

**Example Usage:**

```text
IO1 INT FALL
-OK

IO1 INT RISE
-OK

IO1 INT CHANGE
-OK

IO1 INT NONE
-OK
```

### VALUE

Gets/sets the current value of a specific IO signal. The meaning of the value is dependent on the current mode setting of the signal. The acceptable units for value depends on the current pin mode of operation. 

Set Interrupt configuration: `IO[n] VALUE [val]`

Get Interrupt configuration: `IO[n] VALUE ?`

**Parameters:**

This function takes on parameter, _`val`_:

* When the pin is configured as a digital output:
  * val can be 0 or LOW to set the output low
  * val can be 1 or HIGH to set the output high
* When the pin is configured as an analog output:
  * val can be 0 to 1024 to set the DAC output in counts
  * val can be 0V to 3.3V to set the DAC output in voltage
* When the pin is configured as PWM output:
  * val can be 0 to 1024 to set the duty cycle in counts
  * val can be 0% to 100% to set the duty cycle by percentage
* When the pin is configured as an Analog or Digital Input:
  * Setting is disabled, Query VALUE to read the input state

**Response:**

Set: This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in configuring the output on the specified IO pin. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

Get: This function returns `-IO[n]` followed by `VALUE` followed by the current value of the IO pin, which depends on the current operating mode of the pin.

* When the pin is configured as a digital input or output, returned value will be either 0 or 1
  * Example: `IO[n] VALUE 0`
* When the pin is configured as an analog input or output, returned value will be in counts and volts
  * Example: `IO[n] VALUE 2512 2.02V`
* When the pin is configured as a PWM output, the returned value will be in counts and percentage
  * Example: `IO[n] VALUE 512 50%`

**Example Usage:**

```text
IO0 MODE DIN
-OK

IO0 VALUE ?
-IO0 VALUE 0

IO1 MODE AOUT
-OK

IO1 VALUE 2.5V
-OK

IO1 VALUE ?
-IO1 VALUE 775 2.50V

IO0 MODE PWM
-OK

IO0 VALUE 50%
-OK

IO0 VALUE ?
-IO0 VALUE 512 50%
```

### PWMFREQ

Gets/sets the current value of the PWM frequency of a specific IO signal. This is only applicable when in PWM Mode. Note that IO0 and IO2 share a PWM Frequency, and IO3 and IO4 share a PWM Frequency. The default PWM frequency is 10kHz.

Set Frequency: `IO[n] PWMFREQ [frequency]`

Get Frequency: `IO[n] PWMFREQ ?`

**Parameters:**

This function takes on parameter, _`frequency`_, which can be from 750Hz to 80000Hz.

**Response:**

Set: This function returns an [ACK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#ack-response) if the command succeeds in configuring the PWM frequency on the specified IO pin. If the command fails, the function will return a [NAK Response](https://support.binho.io/user-guide/using-the-device/receiving-responses#nak-response).

Get: This function returns `-IO[n]` followed by `PWMFREQ` followed by the currently configured PWM frequency.

**Example Usage:**

```text
IO3 MODE PWM
-OK

IO3 PWMFREQ ?
-IO3 PWMFREQ 10000

IO3 PWMFREQ 33000
-OK

IO3 PWMFREQ 99000
-NG

IO3 PWMFREQ ?
-IO3 PWMFREQ 33000
```

