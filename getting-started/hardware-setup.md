# Hardware Setup

### Driver Installation <a id="driver-installation"></a>

**Great News!** Most modern operating systems already have the device drivers needed for your _Binho Nova Multi-Protocol USB Host Adapter_ to work properly installed. The _Binho Nova_ utilizes the standardized USB Communications Device Class driver in order to achieve maximum compatibility with as many systems as possible. As such, there's no driver to download and install.

Windows 7 does **not** have the standard USB CDC driver mentioned above. Please see the troubleshooting article below.

{% page-ref page="../troubleshooting/windows-driver-installation.md" %}

If this is the first time connecting to a hardware device from your account on your Mac, you'll need to grant permission to your user account. Please see the following support article for instructions to perform this task:

{% page-ref page="../troubleshooting/macos-permissions.md" %}

If you're on Ubuntu or another Linux distribution and running into issues with your device, please perform the procedure in the following troubleshooting article:

{% page-ref page="../troubleshooting/linux-permissions.md" %}

### Connection to the Computer

Use the provided USB Type-C \(male\) to USB Type-A \(male\) cable to connect the _Binho Nova Multi-Protocol USB Host Adapter_ to your host PC. The device can be plugged into any available USB port - there is no requirement to plug it into a USB 3.0 port.

Upon power up, the Status LED on the _Binho Nova_ will shine _yellow_ while it is waiting for the COM port to be opened. When connected properly, the device should immediately enumerate and become available for use as a new COM port. Once a serial connection has been established between the device and the host computer, the Status LED will shine _blue_.

{% hint style="info" %}
The Status LED is user-programmable, so after initial power up and establishment of the serial connection, the Status LED color meaning is not correlated with any particular device state unless specifically set from within the test scripts.
{% endhint %}

### Connection to the Test Circuit

The _Binho Nova Multi-Protocol USB Host Adapter_ features an integrated wire harness terminated with a 1.27mm pitch 2x5 IDC Connector. This harness contains the 5 signal pins, 1 x 3V3 power signal, 1 x VBUS power signal, and 3 x GND signals.

The figure below shows the connector pinout and channel functions:

![](../.gitbook/assets/image%20%2828%29.png)

### Clear for Takeoff!

Now you're ready to begin interacting with the device. See the QuickStart Guides in this section to learn how to interact with the device via your preferred interface:

{% page-ref page="quickstart-with-binho-gui/" %}

{% page-ref page="quickstart-with-python.md" %}

{% page-ref page="quickstart-with-coolterm.md" %}



