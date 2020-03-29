# Updating Firmware

The _Binho Multi-Protocol USB Host Adapter_ is designed for ease of use in every aspect, and that's especially true when it comes to updating the device firmware. No software installation is required. In fact, even it can be done programmatically from your own scripts if so desired!

### Step \#1: Reset into the Device Bootloader

The first step is reboot the device into the bootloader. This is achieved simply by sending the [+BTLDR ASCII Command](https://support.binho.io/user-guide/ascii-interface/device-commands#btldr) to the device while it's connected to your host PC. This will immediately terminate the virtual comport connection. After a few seconds, the device will show up as new Mass Storage Device \(similar to a USB drive or SD Card\) with a Volume Name of "BINHOBTLDR".

![](../../.gitbook/assets/firmwareupdate.gif)

### Step \#2: Transfer the Firmware File to the Device

Now that the _Binho Multi-Protocol USB Host Adapter_ looks like a storage device, all one needs to do is save the firmware file \(.FIG extension\) to the drive. This is as simple as copy/paste or drag and drop. When a valid firmware file has been written to the drive, it will automatically restart in a few seconds.

### Step \#3: Verify the Update

Once the device has restarted, simply open up the serial connection and verify the new firmware version by sending the [+FWVER ASCII Command](https://support.binho.io/user-guide/ascii-interface/device-commands#fwver).

That's all there is to it! We'll be releasing new functionality via firmware updates frequently, so be sure to check for new releases and update often. The firmware releases can be found here:

{% page-ref page="../../firmware-releases/" %}



