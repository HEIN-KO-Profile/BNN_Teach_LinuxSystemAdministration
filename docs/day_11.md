---
Title: "Day 11: OpenSSH-Server ( data transfer, user authentication, key authentication, port forwarding), E Linux, Manage Kernel version and KVM Intro"
---

# Day 11: SE Linux & Open SSH Server

## Subtitle: Interface Management, DHCP, Static, NTP Setup, Firewalls, SELinux, SSH, Kernel Management
### SE Linux (Security Enhance Linux)

For security 
1. File Permission
2. Users/Groups Management
3. Firewall
4. SE Linux

- SE Linux Mode (1. Enforcing , 2. Permissive, 3. Disable)
- Permissive (Labeling)
- Enforcing (Labeling  & Policy)

Route for SE Linux
- client <----> Firewall <---> SE Linux <---> Server

- `getenforce` (check the mode of SE Linux)
- `setenforce 0` (temporary changing mode)
- 0 = permissive
- 1 = Enforcing

for permanent changing mode use vim
- `vim /etc/selinux/config`

- `semanage` (to manage SE Linux)
- `dnf provides semanage` (to check the package manager)
- `seinfo`       (to get the information about SE Linux)
- `dnf provide seinfo`

### Open SSH Server 
Route
- Remote Machine (Server, Personal) <---- ssh -----> client devices

Use SSH Server for 
1. Remote login
2. Data transfer
3. Key Authentication
4. User Authentication

#### Server side
- Remote Machine ( install openssh services)

1. package install (openssh-server)
- `dnf install openssh-server -y`
  
2. enable sshd(check)
- `systemctl status sshd`
- `systemctl start sshd`
- `systemctl enable sshd`
  
3. add firewall
- `firewall-cmd --permanent --add-service--ssh`
- `firewal-cmd -- permanent --add-port=22/tc;`
- `firewall-cmd --reload`

- to check key (`cd /root/.ssh/` )
- `cat -n autorized_keys`
- `cat /dev/nullssh-copy-id -i /root/.ssh/id.rsa.pub root@servera`
> authorized_keys (deleting the keys)

4.User Authentication
- `tail /etc/passwd` (check user)
- `passwd user1`     (set passwd)
- `vim /etc/ssh/sshd_config`
- Shift+a,
- Shift+g
- AllowUsers user_name
- :wq!
- `systemctl restart sshd` (restart service)
- `last`
- `who`

### SSH Port Forwarding (changing port number)
- `default port SSH =22/tcp`
- `vim /etc/ssh/sshd_config` (change port number in line 21)
- `systemctl restart sshd`
- `systemctl status sshd`
- change the port number in firewall
- `firewal-cmd -- permanent --add-port=2222/tcp`
- add the port number in SE Linux
- `semanage port -l | grep "ssh"`
- `semanage port -a -t ssh_port_t -p tcp port_number` (adding port)
- `semanage port -d -t ssh_port_t -p tcp port_number` (deleting port)

- checking ports
- `ss -ntlp`

#### Client side

2. Data Transfer
vim /etc/hosts (to change severname from ip , change ip to name)
scp source_file root@server:~/ (sending file from client to server)
scp -r source_dir root@server:~/
scp root@servera:~/ ./              (getting file from server)
sftp root@servera        (ssh file transfer protocoal)
sftp     (to check in server command - ls , to check in local command  - lls)
sftp> put 

3. Key Authentication
a. generate SSH Key
ssh-keygen
b. send key to server
ssh-copy-id -i /root/.ssh/id.rsa.pub root@servera
to check  (ssh root@servera)

4.User Authentication
ssh user_name@servera
After SSH Port forwarding (changing port number)
ssh -p new_port_number root@servera

Managing Karnel Version (setting default karnel)
uname -r (to check karnel version)
yum list kernal
yum update karnel
grub2-editenv list (to check default karnel)
grup2-setdefault "karnel_version"
grup2-mkconfig -o /boot/grub2/grub.cfg
reboot  (init 6)

Using KVM instead of Orcale VM
kvm setup on Ubuntu_version (search in browser)
egep -c '(vmx | svm)' /proc/cpuinfo
