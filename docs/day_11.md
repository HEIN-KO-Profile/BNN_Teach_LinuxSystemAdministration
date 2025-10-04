---
Title: "Day 11: SE Linux & Open SSH Server"
---

# Day 11: SE Linux & Open SSH Server

## Subtitle: E Linux (Security Enhance Linux), Open SSH Server, SH Port Forwarding (changing port number), Managing Karnel Version (setting default karnel),Using KVM instead of Orcale VM 

### SE Linux (Security Enhance Linux)

For security 
1. File Permission
2. Users/Groups Management
3. Firewall
4. SE Linux

- SE Linux Mode (1. Enforcing , 2. Permissive, 3. Disable)

Modes of SE Linux
1. Enforcing (Labeling & Policy)
2. Permissive (Labeling)
3. Disable

Route for SE Linux
- client <----> Firewall <---> SE Linux <---> Server

- `getenforce` (check the mode of SE Linux)
- `setenforce 0` (temporary changing mode)
- 0 = permissive
- 1 = Enforcing

For permanent changing mode use vim
- `vim /etc/selinux/config`
- SELINUX=enforcing/permissice/disabled
- ESE
- :wq!

- `semanage` (to manage SE Linux)
- `dnf provides semanage` (to check the package manager)
- `dnf install policycoreutils-python-utils`
- `seinfo`       (to get the information about SE Linux)
- `dnf provides seinfo`
- `dnf install setools-console-4.5.1-4.el10.x86_64`

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
  
2. status/start/enable sshd(check)
- `systemctl status sshd`
- `systemctl start sshd`
- `systemctl enable sshd`
  
3. allow firewall rules (survice & port)
- `firewall-cmd --permanent --add-service--ssh`
- `firewal-cmd -- permanent --add-port=22/tc;`
- `firewall-cmd --reload`

4. Checking Users Key
- to check key (`cd /root/.ssh/` )
- `cat -n autorized_keys`
- `cat /dev/nullssh-copy-id -i /root/.ssh/id.rsa.pub root@servera`
- `rm -rf authorized_keys` (deleting the keys)

5. User Authentication
- `tail /etc/passwd` (check user)
- `passwd user1`     (set passwd)
- `vim /etc/ssh/sshd_config`
- Shift+g
- Shift+a
- AllowUsers user_name
- ESC
- :wq!
- `systemctl restart sshd` (restart service)
- `last`
- `who`

### SSH Port Forwarding (changing port number)

#### Server Side
- `default port SSH =22/tcp`
1. Change port to desire_port_number  
- `vim /etc/ssh/sshd_config` (change port number in line 21)
- ESC
- :set number 21
- :21
- Port desire_port_number  (e.g 2222)
- `systemctl restart sshd`
2. change the port number in firewall
- `firewal-cmd -- permanent --add-port=2222/tcp`
- `firewall-cmd --reload`
3. add the port number in SE Linux
- `semanage port -l | grep "ssh"`
- `semanage port -a -t ssh_port_t -p tcp port_number` (adding port)
- `semanage port -d -t ssh_port_t -p tcp port_number` (deleting port)
- `systemctl status sshd`
- Checking for open ports
- `ss -ntlp`

#### Client side

1. Binding Server IP and Host Name
- `vim /etc/hosts`       (to change severname from ip , change ip to name)
- i
- server_ip_address     server_name
- ESC
- :wq!
  
2. Data Transfer
- `scp source_file root@server:~/ `     (sending file from client to server)
- `scp -r source_dir root@server:~/`
- `scp root@servera:~/file_name ./ `             (getting file from server)
- `sftp root@servera`       (ssh file transfer protocoal)
- sftp    (to check in server command - ls , to check in local command  - lls)
- sftp> get file_name
- sftp> put 

3. Key Authentication
a. generate SSH Key in Client
- `ssh-keygen`
b. copy & send key to server
- `ssh-copy-id -i /root/.ssh/id.rsa.pub root@servera`
to check  (`ssh root@servera`)

4. User Authentication
- `ssh user_name@servera`
- `whoami`
- `su - root`     (Changing to root user)

After SSH Port forwarding (changing port number)
ssh -p new_port_number root@servera

### SSH Port Forwarding (changing port number)

#### Client Side

- checking ports
- `ss -ntlp`
- `ssh -p 2222 root@server_name`

### Managing Karnel Version (setting default karnel)
- `uname -r`(to check karnel version)
- `yum list kernel`
- `yum update kernel`
- `grub2-editenv list` (to check default karnel)
- `grup2-set-default "karnel_version"`
- `grup2-mkconfig -o /boot/grub2/grub.cfg`
- `reboot`  (init 6)

### Using KVM instead of Orcale VM
kvm setup on Ubuntu_version (search in browser)
- `egrep -c '(vmx | svm)' /proc/cpuinfo`   (Checking for Virtualization support, if not zero supportive)
