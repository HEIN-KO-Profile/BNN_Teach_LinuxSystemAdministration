---
Title: "Day 10: Linux Networking - Interface Management, DHCP, Static, NTP Setup, Firewalls, SELinux, SSH, Kernel Management"
---

# Day 10: Linux Networking

## Subtitle: Interface Management, DHCP, Static, NTP Setup, Firewalls, SELinux, SSH, Kernel Management

These are the Day 10 notes.

---

### NTP Service (Network Time Protocol use in Bank)

- `timedatectl` → Check NTP service  
- `systemctl status chronyd`  
- `systemctl start chronyd`  
- `systemctl enable chronyd`  
- Find NTP server address for your region in browser  
- Edit configuration:  
 - `vim /etc/chrony.conf`
- Restart service:
- `systemctl restart chronyd`

 ### Linux Networking - Linux to Linux
Linux Networking — Linux to Linux
Managing Interfaces

- Check network interfaces: (ethernet, wireless)
- `ipconfig`
- (If not installed:
- `sudo apt install net-tools` for Ubuntu
- `dnf install net-tools -y` for Rocky/Fedora)

Ethernet → enp...
Loopback → lo (127.0.0.1)
1. IPv4/IPv6: `ip a`
   
3. Subnet:
- `dnf install ipcalc`
- `ipcalc 192.168.7.121/24`

4. Gateway/router IP:
   - `arp -a`
   - 
5. DNS (domain name servicer)



Managing Connections

On a Linux server, you typically manage several types of network connections, depending on what services and interfaces are in use. Here’s a breakdown:
🔹 1. Ethernet (Wired)
Standard physical network connection.
Interfaces usually named eth0, enp0s3, etc.
Most common for production servers.

🔹 2. Wi-Fi (Wireless)
Interfaces named wlan0, wlp2s0, etc.
Less common in servers, but possible (especially laptops/edge devices).
Managed with tools like iwconfig, nmcli, or wpa_supplicant.

🔹 3. Loopback
Interface lo (127.0.0.1).
Used for internal communication within the same server.
Always present and essential.

🔹 4. Virtual / Bridge Interfaces
Created for virtualization and containers.
Examples:
virbr0 → libvirt bridge.
br0 → manually configured bridge for VMs or Docker.
Allow guests/containers to share the host’s network.

🔹 5. VLAN Interfaces
Tagged network connections for separating traffic.
Example: eth0.10 (VLAN ID 10).
Used in enterprise/data center environments.

🔹 6. VPN Connections
Secure encrypted tunnels.
Types: OpenVPN, WireGuard, IPsec, L2TP, etc.
Managed with systemd-networkd, nmcli, or specific VPN clients.

🔹 7. Bonded (Teamed) Interfaces
Combine multiple NICs into one logical interface.
Provides redundancy (failover) or higher throughput.
Names: bond0, team0.

🔹 8. Point-to-Point (PPP/Serial/DSL)
Less common now but still used in some edge setups.
Interfaces: ppp0.

🔹 9. Container / Overlay Networks
Created by container platforms (Docker, Kubernetes).
Examples: docker0, cni0, flannel.1, vxlan0.
Manage internal networking between containers and pods.

to check connections (`nmcli con show`)
to change connection name (use two connections types - dhcp (auto ) or static (manual)
- `nmcli con show`
- `nmcli con add/del/mod/down/up`
- `systemctl restart NetworkManager`

Example for DHCP connection:
- `nmcli con add con-name "dhcp" type ethernet ifname enp0s3 ipv4.method auto ipv4.dns "8.8.8.8, 8.8.4.4" autoconnect yes`

Example for Static connection:
- `nmcli con add con-name "static" type ethernet ifname enp0s3 ipv4.method manual ipv4.addresses 192.168.31.210/24 ipv4.gateway 192.168.31.1 ipv4.dns "8.8.8.8, 8.8.4.4" autoconnect yes`

if the connection doesn't satrt automatically
- `nmcli con up static`

Homework:
- Create one DHCP connection (down) and one static connection (up).
- `use nmcli con shown`
- `ftp /21/tp/permanent add`

