# MacOS Permissions

On MacOS, you'll need to ensure that your user account is added to the `wheel` group in order to grant it the permissions needed to access the hardware. This is only necessary once per user.

Simply run the following command:

```
sudo dscl . -append /groups/wheel GroupMembership <username>
```

