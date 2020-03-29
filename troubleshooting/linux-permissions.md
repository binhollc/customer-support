# Linux Permissions

To use _Binho Multi-Protocol USB Host Adapters_ with most modern Linux distributions, you'll want to ensure a few udev rules are applied. These rules apply special configuration for USB devices like this to workaround or fix common issues. The rules also fix an issue with ModemManager hanging on to /dev/ttyACM devices.

To install the rules you;ll want to download them and copy to the location of udev rules on your system. For most Linux systems like Ubuntu, etc. udev rules are stored in the /etc/udev/rules.d/ \(check your distribution's documentation / help forums if you don't see this folder existing in your file system\). Run the following commands:

```bash
wget 'https://bitbucket.org/!api/2.0/snippets/binho-llc/eanz95/ae0b3f04efccb0ec3b3de11fd068d7af6d6e12dd/files/binhoHostAdapter.rules'
sudo cp binhoHostAdapter.rules /etc/udev/rules.d/
```

Note that you might need to change the rule if you're using a distribution other than Ubuntu/Debian. In particular, the first rule in the file applies the 'dialout' group to the binho Host Adapter. Some distributions use a different group for this instead of 'dialout'. Check your distribution docs or help forums to see what group should apply to devices that can be accessed by non-root users.

Next you'll need to reload udev's rules so that they are properly applied. You can restart your machine, or run a command like the following:

```bash
sudo udevadm control --reload-rules
sudo udevadm trigger
```

And if that fails, reboot your system as it will ensure udev picks up the new configuration.

You can also add yourself to the 'dialout' group with

```bash
sudo usermod -a -G dialout $USER
```

or

```bash
sudo usermod -a -G plugdev $USER
```

Finally, I'd to send out a special thank you to the team at [Adafruit ](https://www.adafruit.com)for putting together a similar set of instructions for their boards which really reduced the burden in figuring this out.

