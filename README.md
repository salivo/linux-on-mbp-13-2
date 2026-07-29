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
Bluetooth

The connection works out of the box.

However, Bluetooth audio is not quite perfect. There might be a way to fix it, but I haven't tried it yet.
