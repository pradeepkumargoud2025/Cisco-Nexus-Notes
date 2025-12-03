# NXOS IOS Upgrade Procedure

## 1. Initial Setup
- Confirm device is powered on and accessible.
- Set admin password:
  ```bash
  Enter password for admin:
  Confirm password:

Skip basic configuration dialog:

Would you like to enter the basic configuration dialog? NO

2. User Authentication

Set login credentials:

User Auth Login: admin
Password: << >>

3. Verify System Information

Check system clock and license:
show clock
show license

4. Check USB for NX-OS Image

List files on USB:
dir usb1:

Note: Confirm the desired NX-OS image is present.

5. Copy NX-OS Image to Bootflash

Copy image from USB to bootflash:
copy usb1:nxos64-cs.10.5.2.F.bin bootflash:

Note: Wait until the copy completes.

6. Install NX-OS Image

Start installation:

install all nxos bootflash:nxos64-cs.10.5.2.F.bin

* Installation runs 3–4 stages. Look for success messages.
* Verify upgraded images using the upgrade table.

7. Reload Switch for Upgrade

After installation, reload is required:

Do you want to continue with installation (y/n)? Y

8. Post-Upgrade Verification

Verify NX-OS version and boot details:

show version
show boot


