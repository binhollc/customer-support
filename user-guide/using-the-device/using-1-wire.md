# Using 1-Wire

The _Binho Nova Multi-Protocol USB Host Adapte_r can function as a Dallas 1-WIRE host device. This protocol can be configured to operate on any of the IO pins on the device, however it is especially convenient to use it on IO0 and IO2 as the internal pull-up resistor can be used thus eliminating the need for an external pull-up resistor.

The following table gives a brief overview of the available 1WIRE commands.

| Command | Description | Link |
| :--- | :--- | :--- |
| **BEGIN** | Starts the 1-Wire host on the given IO pin. | [Details](https://support.binho.io/user-guide/ascii-interface/1-wire-commands#begin) |
| **RESET** | Resets the 1-Wire devices on the bus and prepares them to receive commands. | [Details](https://support.binho.io/user-guide/ascii-interface/1-wire-commands#reset) |
| **WRITE** | Writes a byte to the selected 1-WIRE device. | [Details](https://support.binho.io/user-guide/ascii-interface/1-wire-commands#write) |
| **READ** | Reads a byte from the selected 1-WIRE device. | [Details](https://support.binho.io/user-guide/ascii-interface/1-wire-commands#read) |
| **SELECT** | Select the target 1-Wire device on the bus. | [Details](https://support.binho.io/user-guide/ascii-interface/1-wire-commands#select) |
| **SKIP** | Skip the device selection process. | [Details](https://support.binho.io/user-guide/ascii-interface/1-wire-commands#skip) |
| **SEARCH** | Search for the next 1-Wire device on the bus. | [Details](https://support.binho.io/user-guide/ascii-interface/1-wire-commands#search) |
| **DEPOWER** | Power down the 1-Wire bus. | [Details](https://support.binho.io/user-guide/ascii-interface/1-wire-commands#depower) |
| **ADDR** | Get the address of the device found using the SEARCH command. | [Details](https://support.binho.io/user-guide/ascii-interface/1-wire-commands#addr) |
| **WHR** | Writes 0 to 1024 bytes and then Reads 0 to 1024 bytes | [Details](https://support.binho.io/user-guide/ascii-interface/1-wire-commands#whr) |

Feel free to jump ahead to the ASCII Command Set reference to learn the specifics of each command, or continue below to see an example of how to use these commands to achieve communication on the 1-WIRE bus.

{% page-ref page="../ascii-interface/1-wire-commands.md" %}

### Setting Up the 1-WIRE Bus

The first step in using the Binho Nova Multi-Protocol USB Host Adapter as a 1-Wire host is to put the adapter into 1-WIRE mode using the [Device Operating Mode](https://support.binho.io/user-guide/using-the-device/device-settings#operating-mode) command.

```text
+MODE 0 1WIRE
-OK
```

Now the host adapter is in 1-WIRE communication mode. The 1-Wire bus has no configurable settings, so all we need to do is decide which IO pin we want to use. Let's take advantage of the internal pull-up resistor on IO0:

```text
1WIRE0 BEGIN 0 PULL
-OK
```

And just like that, we're ready to start communicating with 1-Wire devices on the bus.

### Selecting a Target Device

Let's get things started off right by first resetting all the devices on the bus:

```text
1WIRE0 RESET
-OK
```

With all the devices reset, it's now possible to begin the process of selecting the device that will be communicated with. Let's start a search for devices on the bus -- but before doing the search, let's make sure we start at the beginning of searching through the devices by reseting the search.

```text
1WIRE SEARCH RESET
-OK
```

Alright, now let's begin the search:

```text
1WIRE SEARCH
-OK
```

Let's see what the search found. We'll query the address to see what device was found:

```text
1WIRE0 ADDR ?
-1WIRE0 ADDR 0x3A 0xE3 0x2B 0x46 0x00 0x00 0x00 0x8E
```

That's great - the exact device we were looking for! Now that we know the address of desired device is in the `ADDR` field, we can move ahead with sending and receiving data.

### Sending and Receiving Data

Now let's get the device's attention and ready to interact with us. This is done by sending another `RESET` command:

```text
1WIRE0 RESET
-OK
```

Now let's select the device we'll be communicating with -- recall that it's address is already in memory from the result of our `SEARCH` command.

```text
1WIRE0 SELECT
-OK
```

Finally let's do some reading and writing:

```text
1WIRE0 WRITE 90
-OK

1WIRE0 WRITE 255
-OK

1WIRE0 WRITE 0
-OK

1WIRE0 READ
-1WIRE READ 0xAA

1WIRE0 READ
-1WIRE READ 0x0F
```

That's all there is to it! Of course getting the transaction complete before any timeouts occur can be challenging when manually interacting with the devices on the bus using these commands, however this isn't an issue when scripting the interactions. When it comes to manually interacting with 1-Wire devices, we highly recommend using the [WHR ](https://support.binho.io/user-guide/ascii-interface/1-wire-commands#whr)command. You can see how to do this in our video tutorial embedded below:

{% embed url="https://www.youtube.com/watch?v=cAAF-zOJbJo" %}



Check out the examples section to see how to use 1-Wire in an automated way:

{% page-ref page="../../python-libraries/examples/" %}

1-Wire is just one of the common protocols which utilizes just one signal for bi-directional communication. On the next page we'll look at Atmel's Single-Wire Interface which is another similar protocol.

