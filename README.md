Danube cloud smartos-platform-upgrade
========================

A script to simplify upgrades of USB booted DC SmartOS installations.

Installation
------------

```
[root@acro ~]# curl -kL https://raw.githubusercontent.com/marcheschi/smartos-platform-upgrade/master/platform-upgrade > /opt/local/bin/platform-upgrade
[root@acro ~]# chmod 755 /opt/local/bin/platform-upgrade
```

Usage
-----

Run the script. It'll find the USB stick, download the latest platform
from Joyent, do the upgrade and report its status. The download and
update platform stages will probably take a few minutes each. Kernel and
boot archive checksums are verified to make sure the files can be read
correctly from the USB stick.

```
[root@paolo ~]# ./platform-upgrade -u file:///zones/PAOLO/platform-20201105T132431Z.tgz -s file:///zones/PAOLO/md5.txt
mkdir: Failed to make directory "/zones/tmp"; File exists
 brand= esdc
Actual kernel=  20200715T230801Z
New Version=  20201105T132431Z
Downloading platform-20201105T132431Z.tgz... OK
Verifying checksum... OK
Extracting latest platform... OK
Marking release version... OK
Checking current boot device... detected c1t0d0, GPT labelusb path=  /dev/dsk/c1t0d0s2
, mounted, OK
Updating platform on boot device... OK
Remounting boot device... OK
Verifying kernel checksum on boot device... OK
Verifying boot_archive checksum on boot device... OK
Activating new platform on /dev/dsk/c1t0d0s2... please wait...Finished: OK


Boot device upgraded. To do:

 1) Sanity check the contents of /tmp/tmp.I8aw9L/usb
 2) umount /dev/dsk/c0t0d0p1
 3) reboot

```

Once the upgrade is done, verify that the contents of the USB stick looks sane,
unmount it and reboot to the new platform.

```
[root@acro ~]# ls -l /tmp/tmp.I8aw9L/usb
total 24
drwxrwxrwx   1 root     root        4096 Jan  3  2011 boot
drwxrwxrwx   1 root     root        4096 Mar 13 07:33 old
drwxrwxrwx   1 root     root        4096 Mar 13 07:29 platform
[root@acro ~]# ls -l /tmp/tmp.I8aw9L/usb/platform/
total 10
drwxrwxrwx   1 root     root        4096 Mar 13 07:29 i86pc
-rwxrwxrwx   1 root     root          17 Mar  6 23:36 root.password
-rwxrwxrwx   1 root     root          17 Mar 13 07:29 version
[root@acro ~]# umount /dev/dsk/c0t0d0p1
[root@acro ~]# reboot
```

By default, the first removable device found is assumed to be the boot USB
stick. If this is not the case, the correct device name can be given on the
command line:

```
[root@acro ~]# platform-upgrade /dev/dsk/c1t0d0p1
Downloading latest platform (platform-20150306T202346Z.tgz)...
```

License
-------

MIT
