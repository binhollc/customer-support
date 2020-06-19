# Software Installation

### Driver Installation

**Great News!** Most modern operating systems already have the device drivers needed for your _Binho Nova Multi-Protocol USB Host Adapter_ to work properly installed. The _Binho Nova Mult-Protocol USB Host Adapter_ utilizes the standardized USB Communications Device Class driver in order to achieve maximum compatibility with as many systems as possible. As such, there's no driver to download and install.

{% hint style="warning" %}
Windows 7 does **not** have the standard USB CDC driver mentioned above. Please see the troubleshooting article below.
{% endhint %}

{% page-ref page="../../troubleshooting/windows-driver-installation.md" %}

If this is the first time connecting to a hardware device from your account on your Mac, you'll need to grant permission to your user account. Please see the following support article for instructions to perform this task:

{% page-ref page="../../troubleshooting/macos-permissions.md" %}

If you're on Ubuntu or another Linux distro and running into issues with your device, please perform the procedure in the following troubleshooting article:

{% page-ref page="../../troubleshooting/linux-permissions.md" %}

### Binho GUI Software Installation

The easiest way to get familiar with your _Binho Nova Multi-Protocol USB Host Adapter_ is by taking it for a test drive with our cross-platform desktop GUI software called Mission Control. Even if you intend to use it in automated environments and applications, Mission Control is a great starting point before diving in to writing scripts. Check out the QuickStart guide here to learn how to install and use it:

{% page-ref page="../../getting-started/quickstart-with-binho-gui/" %}

### Serial Console Installation

Chances are that you already have a serial console application installed on your system. Go ahead and use the one you prefer. This documentation is written for [CoolTerm](https://freeware.the-meiers.org/), but there's nothing particular about the serial connection, so any serial console application should be just fine. You can see the specifics of the device connection properties on the following page:

{% page-ref page="sending-commands.md" %}

If you don't already have a serial terminal application installed, or you'd like to try out something new/different from what you've been using, feel free to download one of these popular serial consoles:

* [CoolTerm ](https://freeware.the-meiers.org/)\(Cross-Platform\)
* [PuTTY ](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)\(Cross-Platform\)

{% hint style="info" %}
Our friends over at [Sparkfun Electronics](https://www.sparkfun.com) have created a comprehensive guide for a number of different serial terminal applications, complete with a history lesson and glossary of common terms. If you'd like to go down that rabbit hole, here you go: [Serial Terminal Basics](https://learn.sparkfun.com/tutorials/terminal-basics/serial-terminal-overview) 
{% endhint %}

### Python Installation

More than likely you'll want to start writing scripts to employ the _Binho Nova Multi-Protocol USB Host Adapter_ in automated testing. You'll need Python 3 to use the provided libraries. If you don't already have Python 3 installed, you can download it [here](https://www.python.org/downloads/).

{% hint style="warning" %}
Our Python Libraries do not work with Python 2.7. Python 2.7 was [retired on January 1, 2020](https://pythonclock.org/), so it's a great time to make the switch. 
{% endhint %}

All of the documentation for the Python libraries can be found on the following page:

{% page-ref page="../../python-libraries/" %}

We recommend spending a few moments sending manual commands to get familiar with the command structure of the _Binho Nova Multi-Protocol USB Host Adapter_ as it will then make it easier to understand the structure of the Python Libraries and function names.

