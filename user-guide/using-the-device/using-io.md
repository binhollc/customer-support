# Using IO

The _Binho Nova Multi-Protocol USB Host Adapter_ features a total of 5 x IO pins. Each of the IO pins is capable of multiple functions. The graphic below present the pin out as well as the functions of each of the IO pins.

{% hint style="info" %}
The operating mode of the _Binho Nova Multi-Protocol USB Host Adapter_ is set to IO by default at power-on. If the host adapter is in another mode of operation, then only the subset of IO pins that are unused will be available for use as IO.
{% endhint %}

![](../../.gitbook/assets/20200619\_novaPinout.png)

Discussion regarding the specific protocols will follow this chapter, and for now the focus will be on the general use of the IO pins. These functions include Digital Input, Digital Output, Analog Input, Analog Output, PWM output, as well as interrupt-enabled inputs.

Despite the vast configuration options, this can all be achieved using just three commands.

{% hint style="info" %}
All IO Commands must be prefaced by `IO[n]` , where _n_ is the pin number. The _Binho Nova Multi-Protocol USB Host Adapter_ features 5 pins, therefore the only valid values for _n_ are integers from 0 through 4.
{% endhint %}

![](../../.gitbook/assets/IOCommands.gif)

| Command     | Description                                                                                                                                                                                             | Link                                                                               |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| **MODE**    | Gets/sets the current mode of a specific IO signal.                                                                                                                                                     | [Details](https://support.binho.io/user-guide/ascii-interface/io-commands#mode)    |
| **INT**     | Gets/sets the interrupt configuration of a specific IO signal.                                                                                                                                          | [Details](https://support.binho.io/user-guide/ascii-interface/io-commands#int)     |
| **VALUE**   | Gets/sets the current value of a specific IO signal. The meaning of the value is dependent on the current mode setting of the signal.                                                                   | [Details](https://support.binho.io/user-guide/ascii-interface/io-commands#value)   |
| **PWMFREQ** | Gets/sets the current value of the PWM frequency of a specific IO signal. This is only applicable when in PWM Mode. Note that IO0 and IO2 share a PWM Frequency, and IO3 and IO4 share a PWM Frequency. | [Details](https://support.binho.io/user-guide/ascii-interface/io-commands#pwmfreq) |

### Digital Input

All 5 of the IO pins are capable of functioning as a digital input pin. A pin can be set as a digital input by setting the `MODE` to `DIN`. The value of the input can then be queried. The following example demonstrates how to use IO2 as a digital input and get it's current value.

```
IO2 MODE DIN
-OK

IO2 VALUE ?
-IO2 VALUE 0
```

### Digital Output

All 5 of the IO pins are capable of functioning as a digital output pin. A pin can be set as a digital output by setting the `MODE` to `DOUT`. The value of the pin can then be set using the `VALUE` command. The following example demonstrates how to use IO2 as a digital output and set its output to HIGH.

```
IO2 MODE DOUT
-OK

IO2 VALUE HIGH
-OK
```

### Analog Input

All 5 of the IO pins are capable of functioning as analog inputs. A pin can be set as an analog input by setting the `MODE` to `AIN`. The value of the pin can then be queried. The measured value will be returned in both ADC counts (12-bit integer) and in units of volts. Note that the voltage reference is 3.3 Volts. The following example demonstrates how to use IO2 as an analog input and get it's current value.

```
IO2 MODE AIN
-OK

IO2 VALUE ?
-IO2 VALUE 4095 3.30V
```

### Analog Output

Only IO1 is capable of functioning as an analog output. This pin can be configured to use the 10-bit DAC to output a voltage between 0 and 3.3 Volts. IO1 can be set as an analog output by setting the `MODE` to `AOUT`. The output voltage can then be set using the `VALUE` command. The output voltage can be set either by passing a voltage or a 10-bit integer value. Note that the voltage reference is 3.3 Volts. The following example demonstrates how to use IO1 as an analog output.

```
IO1 MODE AOUT
-OK

IO1 VALUE 2.2V
-OK

IO1 Value 830
-OK
```

### PWM Output

IO0, IO2, IO3, and IO4 pins have PWM output capabilities. The duty cycle and frequency can both be controlled programmatically. The duty cycle can be set either in _counts_ from 0 to 1024, or in _percentage_ from 0% to 100%. Both are implemented with the `VALUE` command.

```
IO0 MODE PWM
-OK

IO0 VALUE 50%
-OK

IO0 VALUE ?
-IO0 VALUE 512 50%

IO0 VALUE 0
-OK

IO0 VALUE ?
-IO0 VALUE 0 0%

IO0 VALUE 25%
-OK

IO0 VALUE ?
-IO0 VALUE 256 25%
```

The default PWM Frequency is 10kHz, but can be changed from 750Hz up to 80kHz.

```
IO0 MODE PWM
-OK

IO0 PWMFREQ ?
-IO0 PWMFREQ 10000

IO0 PWMFREQ 25000
-OK

IO0 PWMFREQ 90000
-NG

IO0 PWMFREQ ?
-IO0 PWMFREQ 25000
```

IO0 and IO2 PWM peripheral share a timer, so the PWMFREQ setting for these channels will always be the same. IO3 and IO4 PWM peripheral also share another time, so the PWMFREQ setting for these channels will always be the same.

```
IO0 MODE PWM
-OK

IO2 MODE PWM
-OK

IO2 PWMFREQ ?
-IO2 PWMFREQ 10000

IO0 PWMFREQ 33000
-OK

IO0 PWMFREQ ?
-IO0 PWMFREQ 33000

IO2 PWMFREQ ?
-IO2 PWMFREQ 33000
```

When changing the PWM Frequency, the current duty cycle assigned to that signal will remain unchanged.

```
IO0 MODE PWM
-OK

IO0 VALUE 512
-OK

IO0 PWMFREQ 25000
-OK

IO0 VALUE ?
-IO0 VALUE 512 50%
```

### Interrupts

IO1, IO2, IO3, and IO4 signals support hardware interrupts. The interrupts can be configured to fire on rising edge, on falling edge, or on any edge. There is much more discussion on how to handle interrupts on [this page.](https://support.binho.io/user-guide/using-the-device/receiving-interrupts)

```
IO1 INT ?
-IO1 INT NONE

IO1 INT CHANGING
-OK

IO1 INT RISING
-OK

IO1 INT FALLING
-OK

IO1 INT NONE
-OK
```

