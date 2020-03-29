# Receiving Responses

As mentioned previously, every command sent to the host adapter will receive a response. 

{% hint style="info" %}
The response to **every** command from the host adapter will begin with`-`.
{% endhint %}

The Response Packets fall into one of three categories:

### ACK Response

An **ACK** response from the host adapter indicates that the received command has been executed successfully. An **ACK** response is represented by `-OK`

For example, the PING command always elicits an **ACK** response from the device:

```text
+PING
-OK
```

### NAK Response

A **NAK** response from the host adapter indicates that the received command failed to execute. This could be indicative of a spelling error/typo, invalid command name or parameter values, or the failure to successfully execute the called function. A **NAK** response is represented by `-NG`

For example, a keyboard error caused `+PING` to be misspelled as `+PONG`, which is a non-existent command:

```text
+PONG
-NG
```

### Data Response

Many of the commands sent to the host adapter result in the host adapter responding with data. In these cases, the format of the response is specific to the executed command. Typically the response contains the name of the command followed by the response data.

For example, sending the +FWVER command causes the device to respond as such:

```text
+FWVER
-FWVER 0.1.5
```

The ASCII Command Set Reference Guide provides detailed example responses for of the host adapter commands. You can jump to that section of the guide here:

{% page-ref page="../ascii-interface/" %}

### But wait... What About Interrupts?

We thought you'd never ask. Interrupts aren't actually a response to a command - they are somewhat unsolicited transmissions. As such, they are worthy of their own page in this guide.

