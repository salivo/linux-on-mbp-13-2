# Linux on MBP 13,2

My notes on how to make Linux usable on a MacBook Pro 13,2. I use Arch Linux as my distribution.
Kernel: Linux 7.1.5 

## Wi-Fi

I am very thankful to everyone who found a solution to the problem with the Broadcom 43602 wireless chip. [Here](https://bugzilla.kernel.org/show_bug.cgi?id=193121) is the thread where I found the fix.

To resolve the issue, copy `brcmfmac43602-pcie.txt` to `/usr/lib/firmware/brcm/`.

Next, add `brcmfmac.feature_disable=0x82000` to your kernel command line.

Finally, install the regulatory database:

```bash
pacman -S wireless-regdb

```
## Sleep

Sleep works really well, but it required a few additional steps.

Copy the script `macbook-d3cold-prepare` to `/usr/lib/systemd/system-sleep/`, then make it executable:
```bash
chmod +x /usr/lib/systemd/system-sleep/macbook-d3cold-prepare
```
## Audio

Follow the instructions in the [davidjo/snd_hda_macbookpro](github.com/davidjo/snd_hda_macbookpro) repository on GitHub. I installed it using DKMS, and everything worked perfectly after a reboot.

## Bluetooth

The connection works out of the box.

However, Bluetooth audio is not quite perfect. There might be a way to fix it, but I haven't tried it yet.

## Touch Bar

> [!NOTE]
> You must save the macOS firmware files in EFI partition (`/EFI/APPLE/EMBEDEDOS`). This is required to boot the iBridge chip; without these files, it will get stuck in DFU mode.

I used this [fork](https://github.com/jimmykuo/macbook12-spi-driver) to install the necessary kernel modules.

To ensure the modules load in the correct sequence during early boot, add them to your `/etc/mkinitcpio.conf`:

```text
MODULES=(apple_ibridge applespi apple_ib_tb)
```

By default, usbhid will incorrectly take over the apple-ibridge interface. To prevent this, create a new configuration file in `/etc/modprobe.d/` (e.g., `/etc/modprobe.d/apple-ibridge.conf`) with the following content:
```text
options usbhid ignore_special_drivers=1 quirks=0x05ac:0x8600:0x4
```
Finally, regenerate your initramfs and reboot for the changes to take effect

## Useful Links
[xtocdra/macbookpro13-2 GitHub Repository](https://github.com/xtocdra/macbookpro13-2)
