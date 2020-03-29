# Windows 7 Driver Installation

{% hint style="success" %}
Mac, Linux, and Windows 8+ do not require any driver installation. Only users of Windows 7 and below may need to install device drivers.
{% endhint %}

We're in the process of creating a streamlined way to install the device drivers. In the meantime, please follow the manual steps shown below to install the driver for your _Binho Nova Multi-Protocol USB Host Adapter_. Thank you very much for your patience while we work on improving the driver installation experience, and please feel free to reach out to us at **support@binho.io** for any assistance related to driver installation issues.

### Step 1: Download the Driver & Unzip it

#### Download [Binho Win7 Driver 1.2.5.3](https://cdn.binho.io/driver/nova/win7/BinhoWin7Driver1.2.5.3.zip)

### Step 2: Plug in the Binho Nova to the PC

![](../.gitbook/assets/1_devicemanagerunknown.png)

### Step 3: Right Click and Select "Update Driver Software..."

![](../.gitbook/assets/2_updatedriversoftware.png)

### Step 4: Choose "Browse my computer for driver software"

![](../.gitbook/assets/3_browsemycomputer.png)

### Step 5: Select the location of the unzipped driver file from Step 1 and click Next

![](../.gitbook/assets/4_driverlocationnext.png)

{% hint style="danger" %}
If Step 5 above fails, please try the [alternate installation process](https://support.binho.io/troubleshooting/windows-driver-installation#alternative-driver-installation-method) detailed below.
{% endhint %}

### Step 6: Select "Install this driver software anyway"

![](../.gitbook/assets/5_warninginstalldriveranyway.png)

### Step 7: Installation Completed Successfully, Close the Installer

![](../.gitbook/assets/6_success.png)

### Step 8: Confirm Device Driver Installation Success in Device Manager

![](../.gitbook/assets/7_correctdevicemanager.png)

### Step 9: Enjoy the Binho Nova on Your Windows 7 PC

![](../.gitbook/assets/8_guiwin7.png)

{% hint style="danger" %}
At this time, our Windows 7 driver regrettably does not support communicating with the Binho Nova while it's in Bootloader mode. You will need to connect the device to a computer running Windows 8+, MacOS, or Ubuntu in order to update the device firmware.
{% endhint %}

### Alternative Driver Installation Method

If you are unable to install the driver following the method above by "Browsing", please try the manual process below, beginning after completing [Step 4](https://support.binho.io/troubleshooting/windows-driver-installation#step-4-choose-browse-my-computer-for-driver-software) above.

### Step A: Select "Let me pick from a list of device drivers on my computer"

![](../.gitbook/assets/image%20%2829%29.png)

### Step B: Select "Ports \(COM & LPT\)" From the Common Hardware Types List

![](../.gitbook/assets/image%20%2825%29.png)

### Step C: Click the "Have Disk..." button

![](../.gitbook/assets/image%20%281%29.png)

### Step D: Browse to the binho\_usbser.inf file downloaded from this page and click OK

![](../.gitbook/assets/image%20%2824%29.png)

### Step E: Select "Binho USB Host Adapter" and click "Next"

![](../.gitbook/assets/image%20%2811%29.png)

#### At this point, you can return to [Step 6](https://support.binho.io/troubleshooting/windows-driver-installation#step-6-select-install-this-driver-software-anyway) above to complete the process.

