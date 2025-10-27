
Download minimal installation CD (https://distfiles.gentoo.org/releases/amd64/autobuilds/current-install-amd64-minimal/)
`install-amd64-minimal-20250907T165007Z.iso` in my case
`install-amd64-minimal-20250907T165007Z.iso.asc`

## Make the USB accessible from WSL (https://learn.microsoft.com/en-us/windows/wsl/connect-usb)

Install usbipd-win.

Not strictly needed, but doesn't harm:
```powershell
wsl --shutdown
wsl --update
```

Find BUSID of USB device and attach it to WSL: 
```powershell
usbipd list
usbipd bind --busid 2-2  # replace 2-2 by busid found in previous command ; also, this command must be run in admin mode powershell
usbipd attach --wsl --busid 2-2
```

Find USB device in WSL
```bash
$ lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda      8:0    0 388.4M  1 disk
sdb      8:16   0   186M  1 disk
sdc      8:32   0     1G  0 disk [SWAP]
sdd      8:48   0   256G  0 disk /snap
                                 /mnt/wslg/distro
                                 /
sde      8:64   1   7.5G  0 disk                   <=== This is my USB
└─sde1   8:65   1   7.5G  0 part
```

Copy image
```bash
$ sudo dd if=install-amd64-minimal-20250907T165007Z.iso of=/dev/<sde> bs=4096 status=progress && sync  <=== I had to sudo, otherwise permission denied to /dev/sde
796086272 bytes (796 MB, 759 MiB) copied, 33 s, 24.1 MB/s
194924+0 records in
194924+0 records out
798408704 bytes (798 MB, 761 MiB) copied, 144.752 s, 5.5 MB/s
```

Detach USB device (as I don't need anymore)
```powershell
 usbipd detach --busid 2-2
```
