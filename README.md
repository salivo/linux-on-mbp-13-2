# linux-on-mbp-13-2
My notes on how to make Linux usable on a MacBook Pro 13,2.
I use Arch Linux as my distro.

# Wi-Fi
I am very thankful to everyone who found a solution for the problem with the Broadcom 43602.
[Here](https://bugzilla.kernel.org/show_bug.cgi?id=193121) is where I found the fix.

To fix this, just copy `brcmfmac43602-pcie.txt` to `/usr/lib/firmware/brcmf/`.

And also install regualatory db
```bash
pacman -S wireless-regdb
```
