# Steam Machine

## Запуск в Windows

### Шаги настройки

1. - [x] Download ISO with Windows 10
    - https://tb.rg-adguard.net/public.php
2. - [x] Create VHD disk  and install Windows on VHD with VirtualBox
    - https://habr.com/ru/post/49670/
3. - [x] Repare boot
    - `BCDEDIT /set nointegritychecks OFF` https://www.technipages.com/enable-disable-device-driver-signing
4. - [x] Create user `steam` for games with autologon
5. - [x] Install Steam
6. - [x] Activate start on BigPicture in steam
7. - [x] Use `steam.exe` instead of default shell `explorer.exe`
8. - [ ] Change default logo on windows boot screen
9. - [ ] Change default text with reebout and shutdown

## Запуск в linux

### Дополнительное оборудование

#### Внешний ntfs-диск

Добавить в `/etc/fstab` запись `UUID=2E17E25E7FAAA87C /media/akioo/EXTERNAL_256GB ntfs-3g rw,nosuid,nodev,relatime,uid=1000,gid=1000,iocharset=utf8,uhelper=udisks2,x-systemd.automount,x-systemd.device-timeout=10 0 0`, для корректной работы Proton

`UUID` можно получить командой `sudo blkid /dev/sda1`. Опции монтирования можно получить командой `mount |grep /media/akioo/EXTERNAL_256GB`, удалив из них `window_names`.

Опции автомонтирования/размонтирования `x-systemd.automount,x-systemd.device-timeout=10`

Источник https://wiki.archlinux.org/title/Steam/Troubleshooting#Steam_Library_in_NTFS_partition

#### 8BitDo Ultimate 2.4GHz (from https://gist.github.com/ammuench/0dcf14faf4e3b000020992612a2711e2)

FROM https://www.reddit.com/r/Fedora/comments/zmvkdj/8bitdo_ultimate_bluetooth_controller_working_in/

-----

I've bought this new controller from 8BitDo and wished to use on linux, to my sadness the controller didn't work out of the box, neither by cable, the 2.4G dongle or bluetooth.  


So I've tried a number of solutions and this one from u/GodOfEmus over in the 8bitdo community was the one to work for me:  


1. Create a new file /etc/udev/rules.d/99-8bitdo-xinput.rules
2. Paste this udev rule in there, then save and exit the file: 
  
  ```bash
    ACTION=="add", ATTRS{idVendor}=="2dc8", ATTRS{idProduct}=="3106", RUN+="/sbin/modprobe xpad", RUN+="/bin/sh -c 'echo 2dc8 3106 > /sys/bus/usb/drivers/xpad/new_id'"
  ```
3. Run the following command in a terminal: `sudo udevadm control --reload`
4. Unplug and replug the controller if it was already plugged in, it might take a second if you have the bluetooth version

It will basically "cheat" the OS to see the controller as an generic xbox device, so sadly no bluetooth nor gyro control if you care about that, but the rumbling is working for me.  


Link to the original post: [https://www.reddit.com/r/8bitdo/comments/ykdsmv/ultimate\_24\_ghz\_model\_right\_analog\_not\_working\_in/](https://www.reddit.com/r/8bitdo/comments/ykdsmv/ultimate_24_ghz_model_right_analog_not_working_in/)  


And link to the comment of u/GodOfEmus with the solution: [https://www.reddit.com/r/8bitdo/comments/ykdsmv/comment/iv48s4k/?utm\_source=share&utm\_medium=web2x&context=3](https://www.reddit.com/r/8bitdo/comments/ykdsmv/comment/iv48s4k/?utm_source=share&utm_medium=web2x&context=3)  


Sharing this solution here to spread the word in our community
