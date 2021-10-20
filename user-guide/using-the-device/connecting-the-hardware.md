# Connecting the Hardware

### Connection to the Computer

Use the provided USB Type-C (male) to USB Type-A (male) cable to connect the _Binho Nova Multi-Protocol USB Host Adapter_ to your host PC. The device can be plugged into any available USB port - there is no requirement to plug it into a USB 3.0 port.

Upon power up, the Status LED on the _Binho Nova Multi-Protocol USB Host adapter _will shine _yellow _while it is waiting for the COM port to be opened. When connected properly, the device should immediately enumerate and become available for use as a new COM port. Once a serial connection has been established between the device and the host computer, the Status LED will shine _blue_.

{% hint style="info" %}
The Status LED is user-programmable, so after initial power up and establishment of the serial connection, the Status LED color meaning is not correlated with any particular device state unless specifically set from within the test scripts.
{% endhint %}

### Connection to the Test Circuit

The _Binho Nova Multi-Protocol USB Host Adapter_ features an integrated wire harness terminated with a 1.27mm pitch 2x5 IDC Connector. This harness contains the 5 signal pins, 1 x 3V3 power signal, 1 x VBUS power signal, and 3 x GND signals.

The figure below shows the connector pinout and channel functions:

![](../../.gitbook/assets/20200619\_novaPinout.png)

Use the link below to download a printable PDF version of this image for easy reference:

{% file src="../../.gitbook/assets/Printable Binho Nova Pinout.pdf" %}
Binho Nova Pinout PDF
{% endfile %}

{% hint style="success" %}
Dallas 1-WIRE and Atmel SWI (Single-Wire Interface) protocol can be configured to work with any of the five IO pins. It is especially convenient to use with IO0 or IO2 as it’s possible to engage a suitable internal pull up resistor on these channels.
{% endhint %}

Clearly the most convenient method to connect the signals is by including a footprint for the 2x5 IDC connector on your circuit board, but this isn't always an option. As such, you can use the included Breadboard Adapter board to breakout the signals to 0.1in/2.54mm pitch headers which can easily be connected with jumper wires.

![Using the Breadboard Breakout Adapter](../../.gitbook/assets/image.png)

Furthermore, there are several accessories available that make it easy to connect directly to your test circuitry. You can learn all about them here:

{% content-ref url="../accessories/" %}
[accessories](../accessories/)
{% endcontent-ref %}
