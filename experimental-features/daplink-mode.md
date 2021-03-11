# DAPLink Mode

## What is it?

DAPLink mode allows a Binho Nova to be utilized as a programmer / debugger for ARM microcontrollers. By entering DAPLink mode, the Nova will enumerate as a CMSIS-DAP interface that can be discovered by OpenOCD, pyOCD, and even your favorite IDEs. As such, the Nova becomes a robust device which bridges the gap between expensive & powerful debuggers such as [Segger J-Link devices](https://www.segger.com/products/debug-probes/j-link/) and cheap bare board tools or illegal knock-off devices.

Binho Nova's DAPLink mode supports the SWD interface \(SWDIO, SWCLK, and RESET signals\) as well as a UART bridge to a virtual COM port \(using the same TX, RX signals as in normal Nova operation\).

DAPLink is an open-source software project that is developed and maintained by ARM that enables programming and debugging of ARM Cortex CPUs. Please check out the official DAPLink website for more specific details:

{% embed url="https://armmbed.github.io/DAPLink/" %}

## How to try it out

### Step \#1: Enter DAPLink Mode

You can put your Nova into DAPLink mode using our python library CLI with the following command:

```text
binho daplink
```

This will then download our DAPLink-capable firmware to the device. Note that the device cannot be used as a host adapter while in DAPLink mode. \(One of the big limitations is that the pins for the SWD interface are the same as other protocols\).  Note that you can confirm that your device is in DAPLink mode by using the `binho info` command:

```text
PS C:\Users\Jonathan> binho info
Found a Binho Nova [in DAPLink Mode]
  Port: COM3
  Device ID: 0X1C4780B050515950362E3120FF141C2A
  Note: This device is in DAPlink Mode! It can be returned to host adapter (normal) mode
        by issuing 'binho daplink -q' command.
```

### Step \#2: Connect to the target device SWD bus

The pinout of the wire harness for Nova when in DAPLink mode is as shown in the table below:

| Pin Number | Function |
| :--- | :--- |
| IO0 | SWDIO |
| IO1 | nRESET |
| IO2 | SWCLK |
| IO3 | UART RX \(same as normal mode\) |
| IO4 | UART TX \(same as normal mode\) |

It's only necessary to connect the SWDIO and SWCLK signals to the target MCU, most can be reset without the reset signal through various commands. At Binho, we've tested this on ARM-M0 devices from Atmel, Nordic Semiconductor, and Nuvoton.

### Step \#3: Try out pyOCD / OpenOCD

These common tools have no problem discovering the device and using it as if it were a 'CMSIS-DAP' device \(the predecessor of the DAPLink project\). 

You can verify it's working with pyOCD by sending the '`pyocd list`' command:

```text
PS C:\Users\Jonathan> pyocd list
  #   Probe             Unique ID
----------------------------------------------------------
  0   Binho CMSIS-DAP   1C4780B050515950362E3120FF141C2A
```

You can verify it's working with OpenOCD with this command:

```text
openocd.exe -f interface/binho-nova.cfg
```

```text
PS C:\Program Files\openocd-0.10.0\bin-x64> .\openocd.exe -f interface/binho-nova.cfg
Open On-Chip Debugger 0.10.0
Licensed under GNU GPL v2
For bug reports, read
        http://openocd.org/doc/doxygen/bugs.html
adapter speed: 500 kHz
Info : CMSIS-DAP: SWD  Supported
Info : CMSIS-DAP: JTAG Supported
Info : CMSIS-DAP: Interface Initialised (SWD)
Info : CMSIS-DAP: FW Version = 1.10
Info : SWCLK/TCK = 0 SWDIO/TMS = 1 TDI = 0 TDO = 0 nTRST = 0 nRESET = 1
Info : CMSIS-DAP: Interface ready
Info : clock speed 500 kHz
Error: BUG: current_target out of bounds
```

Two quick notes:

1\) You'll need to add the binho-nova.cfg file to the scripts/interfaces directory of your OpenOCD installation. The file can be downloaded here:

{% file src="../.gitbook/assets/binho-nova-openocd-cfg.zip" caption="binho-nova.cfg" %}

2\) The last line 'Error' is what will be displayed if the MCU isn't connected or powered on. When an MCU is connected, it will display some information about the target MCU instead of the error message.

### Step \#4: Try out our Flasher CLI Tool

Hey! Wouldn't it be great if I could simply issue a single command line statement to flash a .hex or .bin file to my microcontroller? We put together a demo script that uses our Python library + the pyOCD scripting interface to show how that can be done.

It's implemented in python library as the `binho flasher` command, and takes a few parameters. You can see the various arguments by passing the -h / --help flag to the command to display them on the console:

```text
binho flasher -h
```

Or you can just take a look at the code for the[ script in our repository](https://github.com/binhollc/binho-python-package/blob/main/binho/commands/binho_flasher.py).

### Step \#5: Try out 3rd-Party tools

Seeed produced a great guide demonstrating how to use a DAPLink device with OpenOCD, Eclipse, Keil, and IAR Embedded Workbench. The configuration for Nova DAPLink mode should be extremely similar to their guide: [Debug and Flash Example for IDEs \[Seeed\]](https://wiki.seeedstudio.com/Arduino-DAPLink/#debug-and-flash-example-for-ides)

### Step \#5: Exit DAPLink Mode

When you're finished using the device in DAPLink mode, issue the following command to return Binho Nova back to normal operation:

```text
binho daplink -q
```

You can verify that your device is back in normal mode using the `binho info` command, just as we did above:

```text
PS C:\Users\Jonathan> binho info
Found a Binho Nova
  Port: COM3
  Device ID: 0X1C4780B050515950362E3120FF141C2A
  Firmware Version: 0.2.5 [Up To Date]
```

## Feedback & Suggestions 

If you've made it this far, please reach out and let us know your thoughts about this feature. We'd love to hear from you!

