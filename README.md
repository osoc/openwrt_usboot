#!/bin/sh
#build by oceanlee.20150225.for openwrt usb device rootfs.

mkdir -p /root
mkdir -p /proc
mkdir -p /sys
mkdir -p /dev
mkdir -p /var
mkdir -p /tmp
mount -t proc none /proc
mount -t sysfs none /sys
mount -t ramfs none /dev
mount -t tmpfs none /tmp
mount -t ramfs none /var
echo /sbin/mdev > /proc/sys/kernel/hotplug
mdev -s

echo -e "\033[31m \033[01m Waiting for openWRT device \033[0m"

tr ()
{
sp='/-\|'
printf ' '
while true; do
        printf '\b%.1s' "$sp"
	        sp=${sp#?}${sp%???}
		done
		}
		tr &
		TR_PID=$!
		sleep 10
		kill -9 $TR_PID

		sed -n 's/.*=//'p /proc/cmdline > /tmp/uuid
		blkid | grep $(cat /tmp/uuid) > /tmp/mountdev
		echo -e "\033[31m \033[01m Your openWRT device is $(cat /tmp/mountdev | cut -d ':' -f 1) \033[0m"
		mount $(cat /tmp/mountdev | cut -d ':' -f 1) /root
		exec switch_root /root /etc/preinit
