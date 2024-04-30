# slax-xfs-boot
Boot slax from a single partition formatted with xfs using syslinux.

This procedure will create a normal slax installation.  Only extlinux is upgraded in this procedure.

Usually, slax will require an ext3, ext4 filesystem to boot from.  XFS seems to be the way a lot of distributions are heading.  Personally, I like the thin file copies.  Although not the same as a snapshot, you can create a relatively consistent copy of an entire disk quite quickly.

To install slax onto an xfs filesystem, use the following procedure.  The packages installed during this procedure are not required to run slax after installation.  XFS is built into the kernel.

Get the required packages
<PRE>apt -y install xfsprogs extlinux syslinux-common</PRE>
Format the drive with 1 partition (or however you want it).
<PRE>echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sda</PRE>
Make an XFS filesystem on the boot partition.  This should auto mount /dev/sda1 on /media/sda1 when complete.
<PRE>mkfs.xfs -L Slax /dev/sda1</PRE>
Copy slax to the desired location.
<PRE>cp -r /run/initramfs/memory/data/slax /media/sda1</PRE>

Update extlinux and associated files in the boot directory from the package.  Select 64 or 32 bit as required.
<PRE>cd /media/sda1/slax/boot
cp /usr/bin/extlinux extlinux.x64
cp /usr/lib/EXTLINUX/mbr.bin .
for Each in *.c32; do cp /usr/lib/syslinux/modules/bios/$Each .; done</PRE>
Update the mbr.
<PRE>/media/sda1/slax/boot/bootinst.sh</PRE>

A bit of clean-up.  You would have to be really tight for space or just like things tidy.  You can remove EFI if you are not using it.
<pre>rm -rf /media/sda1/EFI /media/sda1/slax/boot/EFI</pre>
