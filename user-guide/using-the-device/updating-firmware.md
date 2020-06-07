# Updating Firmware

The _Binho Nova Multi-Protocol USB Host Adapter_ is designed for ease of use in every aspect, and that's especially true when it comes to updating the device firmware. In an effort to provide the greatest flexibility in accommodating our different customers' development and testing environments, we offer a variety of ways to update the Nova device firmware. Below we detail how to update the firmware through three main methods: Automatic Update in Mission Control, Manual Update in Mission Control, and Manual Update through a Serial Terminal.

{% hint style="info" %}
While we do not detail how to do it below, it is also possible to update the Nova firmware programmatically if you so desire!
{% endhint %}

## Updating Firmware through Mission Control

The easiest way to update your Nova device is through our Mission Control GUI software, which allows you to both automatically and manually update the device firmware. Watch the video below where we show how to automatically and manually update the device firmware through our Mission Control software.

{% embed url="https://youtu.be/5THJAmqKlss" %}

### _Automatic Update_

### Step \#1: Connect to the Binho Nova in Mission Control

From the drop down menu in the Device tab of Mission Control select the connect device you want to update and click the "Connect" button.

### Step \#2: Click "Continue" to automatically update to latest firmware version

If your Nova device does not have the latest firmware already installed, a prompt will automatically appear when the device is successfully connected. To update your Nova device, simply click "Continue"

{% hint style="info" %}
If you select "Cancel", you will still be able to use your Nova device. Firmware updates, while recommended, are not required.
{% endhint %}

{% hint style="info" %}
If you select "Cancel" but still want to update your firmware, simply disconnect and reconnect to the desired Nova device.
{% endhint %}

### Step \#3: Verify the firmware version in Device tab

After reconnecting to the newly updated device, you can verify the firmware version in the Device tab of Mission Control.

### _Manual Update_

### Step \#1: Connect to the Binho Nova in Mission Control

From the drop down menu in the Device tab of Mission Control select the connect device you want to update and click the "Connect" button.

### Step \#2: Click "Bootloader" in the Device tab

Clicking the "Bootloader" button in the Device tab of Mission Control resets the Binho Nova device into the bootloader. The device will then show up as a Mass Storage Device \(similar to a USB drive or SD card\) with a Volume Name of "BINHOBTLDR".

### Step \#3: Download firmware file from Binho Support Portal

Download the firmware version that you want to load on to your Nova device from the Binho Support portal [here](../../firmware-releases/). The firmware file is downloaded as a .fig file.

### Step \#4: Transfer the firmware file to the device

Now that the _Binho Nova Multi-Protocol USB Host Adapter_ looks like a storage device and you have downloaded the .fig file of the desired firmware, all one needs to do is save the firmware file \(.FIG extension\) to the drive. This is as simple as copy/paste or drag and drop. When a valid firmware file has been written to the drive, it will automatically restart in a few seconds.

### Step \#5: Verify the Update

After reconnecting to the newly updated device, you can verify the firmware version in the Device tab of Mission Control.

## Updating Firmware through Serial Terminal

### Step \#1: Reset into the Device Bootloader

The first step is reboot the device into the bootloader. This is achieved simply by sending the [+BTLDR ASCII Command](https://support.binho.io/user-guide/ascii-interface/device-commands#btldr) to the device while it's connected to your host PC. This will immediately terminate the virtual comport connection. After a few seconds, the device will show up as new Mass Storage Device \(similar to a USB drive or SD Card\) with a Volume Name of "BINHOBTLDR".

![](../../.gitbook/assets/firmwareupdate.gif)

### Step \#2: Transfer the Firmware File to the Device

Now that the _Binho Multi-Protocol USB Host Adapter_ looks like a storage device, all one needs to do is save the firmware file \(.FIG extension\) to the drive. This is as simple as copy/paste or drag and drop. When a valid firmware file has been written to the drive, it will automatically restart in a few seconds.

### Step \#3: Verify the Update

Once the device has restarted, simply open up the serial connection and verify the new firmware version by sending the [+FWVER ASCII Command](https://support.binho.io/user-guide/ascii-interface/device-commands#fwver).

That's all there is to it! We'll be releasing new functionality via firmware updates frequently, so be sure to check for new releases and update often. The firmware releases can be found here:

{% page-ref page="../../firmware-releases/" %}



