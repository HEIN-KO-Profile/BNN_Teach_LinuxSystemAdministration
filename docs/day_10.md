---
title: "Day 10: Linux Networking - Interface Management, DHCP, Static, NTP Setup, Firewalls, SELinux, SSH, Kernel Management"
---

# Day 10: Linux Networking

## Subtitle: Interface Management, DHCP, Static, NTP Setup, Firewalls, SELinux, SSH, Kernel Management

This is the Day 10 notes.

---

### NTP Service (Network Time Protocol use in Bank)

- `timedatectl` → Check NTP service  
- `systemctl status chronyd`  
- `systemctl start chronyd`  
- `systemctl enable chronyd`  
- Find NTP server address for your region in browser  
- Edit configuration:  
  ```bash
- `vim /etc/chrony.conf`

- Restart service:
- `systemctl restart chronyd`


Linux Networking — Linux to Linux
Managing Interfaces

Check network interfaces:

ifconfig


(If not installed:
sudo apt install net-tools for Ubuntu
dnf install net-tools -y for Rocky/Fedora)

Ethernet → enp...

Loopback → lo (127.0.0.1)

IPv4/IPv6: ip a

Subnet:

dnf install ipcalc
ipcalc 192.168.7.121/24


Gateway/router IP:

arp -a


DNS

Managing Connections
nmcli con show
nmcli con add/del/mod/down/up
systemctl restart NetworkManager


Example for DHCP connection:

nmcli con add con-name "dhcp" type ethernet ifname enp0s3 ipv4.method auto ipv4.dns "8.8.8.8, 8.8.4.4" autoconnect yes


Example for Static connection:

nmcli con add con-name "static" type ethernet ifname enp0s3 ipv4.method manual ipv4.addresses 192.168.31.210/24 ipv4.gateway 192.168.31.1 ipv4.dns "8.8.8.8, 8.8.4.4" autoconnect yes
nmcli con up static


Homework:

Create one DHCP connection (down) and one static connection (up).

SELinux (Security Enhanced Linux)

Modes:

Enforcing

Permissive

Disabled

Commands:

getenforce
setenforce 0    # permissive
setenforce 1    # enforcing


Permanent change:

vim /etc/selinux/config


Manage:

semanage port -l
seinfo

SSH Server

Server side

dnf install openssh-server -y
systemctl status sshd
systemctl start sshd
systemctl enable sshd
firewall-cmd --permanent --add-service=ssh
firewall-cmd --reload


Client side

ssh user@server
scp source_file user@server:~/
sftp user@server

Kernel Management

Check:

uname -r
yum list kernel


Update:

yum update kernel
grub2-set-default "kernel_version"
grub2-mkconfig -o /boot/grub2/grub.cfg
reboot

Example Image

📌 This file should be saved as:

docs/day10.md


Then commit changes to your repo.

