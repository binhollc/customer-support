# Using UART

The _Binho Multi-Protocol USB Host Adapter_ also supports UART communication. This is slightly different than how other supported protocols are implemented as in UART mode, the Binho host adapter is just a pass-through bridge. However, this basic mode of operation affords several advantages versus using a separate USB to UART bridge IC. In particular, it's possible to use the DAC and the I2C bus along with UART since they are all on separate signals not overlapping with UART TX and UART RX signals, or use those 3 available IO pins as some combination of ADC inputs, PWM outputs, or GPIO.

Again for easy reference, here's the connector pinout with the UART signals shown in green:

![](../../.gitbook/assets/20200619_novapinout.png)

| Command | Description | Link |
| :--- | :--- | :--- |
| **CFG** | Gets/sets the UART configuration by passing a configuration string. | [Details](https://support.binho.io/user-guide/ascii-interface/uart-commands#cfg) |
| **BAUD** | Gets/sets the baudrate of the UART connection. | [Details](https://support.binho.io/user-guide/ascii-interface/uart-commands#baud) |
| **DATABITS** | Gets/sets the number of databits for the UART connection | [Details](https://support.binho.io/user-guide/ascii-interface/uart-commands#databits) |
| **PARITY** | Gets/sets the parity bit configuration for the UART connection. | [Details](https://support.binho.io/user-guide/ascii-interface/uart-commands#parity) |
| **STOPBITS** | Gets/sets the number of stop bits for the UART connection. | [Details](https://support.binho.io/user-guide/ascii-interface/uart-commands#stopbits) |
| **ESC** | Gets/sets the escape sequence that can be used to break out of the UART bridge mode. | [Details](https://support.binho.io/user-guide/ascii-interface/uart-commands#esc) |
| **BEGIN** | Starts the UART bridge. The adapter will remain in UART bridge mode until the escape sequence is sent from the host computer. | [Details](https://support.binho.io/user-guide/ascii-interface/uart-commands#begin) |

Feel free to jump ahead to the ASCII Command Set reference to learn the specifics of each command, or continue below to see an example of how to use these commands to achieve communication using the UART bridge.

{% page-ref page="../ascii-interface/uart-commands.md" %}

### Configuring the Connection

The first step in using the Binho Multi-Protocol USB Host Adapter as a UART bridge is to put the adapter into UART mode using the [Device Operating Mode](https://support.binho.io/user-guide/using-the-device/device-settings#operating-mode) command.

```text
+MODE 0 UART
-OK
```

Now let's configure the desired parameters of the UART bridge to match the downstream UART device. Here are the default settings:

* 9600 Baud rate
* 8 Databits
* No Parity bit
* 1 Stop bit
* Escape sequence = `+++UART0`

Let's change the baudrate to match our desired communication speed:

```text
UART0 BAUD 115200
-OK
```

The other settings are quite commonplace, so we'll leave those unchanged in this example. However, there is one important thing to discuss before starting the UART bridge: how to stop the UART bridge.

{% hint style="warning" %}
Once opened, the UART bridge will pass on every character received to it. The only graceful way to close the bridge and get back to the command interface of the Binho Multi-Protocol USB Host Adapter is to transmit the escape sequence from the computer to the host adapter.
{% endhint %}

Let's do a quick exercise to solidify our understanding. The ESC command can be queried so that nothing needs to be memorized:

```text
UART0 ESC ?
-UART0 ESC +++UART0
```

Okay, here we can see that the escape sequence is currently set to `+++UART0`, which is the default setting.

But wait, what if I actually need to send the same string over the UART bridge and I don't want it to stop the bridge. Glad you asked! The escape sequence can be user defined to something even more unique:

```text
UART0 ESC +!+!RQZ86!
-OK

UART0 ESC ?
-UART0 ESC +!+!RQZ86!
```

It's certainly a best practice to always read back the ESC value after writing to it to confirm that it's been set to the expected value. 

### Starting the UART Bridge

Now that we know the escape sequence, we can safely open up the UART bridge.

```text
UART0 BEGIN
-OK
```

Once the -OK response is received, the UART bridge is open and all data will pass through the host adapter. The Binho Multi-Protocol USB Host Adapter will not respond to any commands until the UART bridge is closed.

Data can be simply passed through the host adapter as if it were a direct serial connection to the downstream device. Taking full advantage of the features available, one can easily jump out of the UART bridge to toggle pins, change the output voltage of the DAC, or otherwise stimulate the device, and then re-open the bridge. 

### Closing the UART Bridge

Now let's close the UART bridge and go back to command mode. Simply send the escape sequence that was defined before starting the bridge:

```text
+!+!RQZ86!
-OK
```

Feel free to take a look in the examples section to see how to use the python library to use this in an automated way:

{% page-ref page="../../python-libraries/python-wrapper/examples/" %}

That's it for guides through the various supported protocols. Congratulations on making it this far through the User Guide. The next and final chapter will show you how to perform a device firmware update so that you always have the latest and greatest features on your _Binho Multi-Protocol USB Host Adapter_.

