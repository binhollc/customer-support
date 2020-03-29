# Receiving Interrupts

In an effort to ensure that the full functionality of the _Binho Nova Multi-Protocol USB Host Adapter_ can be easily accessed from as many platforms and environments as possible, it's been deliberately chosen to implement interrupts without using any of the flow control signals which are not always be available and often difficult to use.

As such, interrupts are actually injected into the serial connection as ASCII text strings. Like every engineering decision, there are always some trade-offs. In this case, it means that some additional parsing of received data must be performed so that an Interrupt Packet won't be mistaken for a Response Packet when automating the device. We'll address how to handle this in a moment, but first, some good news:

{% hint style="success" %}
All interrupts are disabled by default when the host adapter powers on. You'll only need to implement parsing for interrupts if you plan on using them in your scripts.
{% endhint %}

And some **even better news:**

{% hint style="success" %}
The binho-host-adapter python libraries already take care of this behind the scenes!
{% endhint %}

At this time, the firmware supports 4 configurable hardware interrupts from the interrupt-enabled IO pins on the _Binho Nova Multi-Protocol USB Host Adapter_:

* `!IO1`
* `!IO2`
* `!IO3`
* `!IO4`

{% hint style="info" %}
Every interrupt sent from the host adapter will begin with`!`.
{% endhint %}

These strings will start to appear in the stream of data from the host adapter when interrupts are enabled and the interrupt condition occurs. When manually interacting with the host adapter from a serial console, this is trivial to deal with. However, automated control will require a bit of extra code.

There are a number of ways to deal with this, and it will depend on the language and environment used for automation. If the language is event driven, then checking the received data from the serial connection to match with the interrupt strings above is quite easy. But languages which do not support the firing of events on serial data arrival become tricky, as one must constantly poll for the arrival of an interrupt.

Thankfully, our python library is an excellent reference for how to implement a separate thread which manages reading the serial port and checking for interrupts. It then determines if the received packet is a response packet or an interrupt packet, and places it into a queue accordingly. The items in the Queues are then consumed by the main program thread.

Anyways, let's come out of this rabbit hole for now -- there will be more discussion on this topic in the "Using IO" section coming up.

The next pages in this section of the guide will provide instructions to utilize the full set of supported protocols and features of your host adapter.

