# Can multiple devices be used at the same time?

**Yes!** One of the key features is that it's easy to use multiple devices on the same computer at the same time. Each device will be enumerated as a different COM port on the machine. This eliminates device management headaches and limitations that occur when using other types of USB device classes. 

There are some nuances to identifying devices associated with each COM port, however the binhoUtilities python library provides an elegant example of device management that can easily be implemented in any language.

{% page-ref page="../python-libraries/python-wrapper/binhoutilities.md" %}

Device management is made easier by querying the unique device ID of each Binho Multi-Protocol USB Host Adapter. For easy visual identification of multiple devices, the RGB LED can be programmatically set to particular colors to identify the devices.

