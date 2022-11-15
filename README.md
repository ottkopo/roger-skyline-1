# roger-skyline-1
System and Network administration project.

<img width="883" alt="image" src="https://user-images.githubusercontent.com/58331418/201898040-0365be17-ac12-4531-a5a2-28efcba9b249.png">

**Project description**

roger-skyline-1 is our entry level project for system administration. The object of the project was to boot up a debian virtual machine and set it up following instructions given in the subject. I personlly only completed the mandatory part of the project which gave me the final score of only 60/100 due to a bit of a time crunch. I may return to also do the optional part later which involves setting up a basic website.


Since this project does not contain any code, I will just add my notes for the project here. To evaluate this project we needed to perform these actions to 3 different people so I wrote down all the actions I needed to do and just followed this list during the evaluation to do all the needed actions. 

**My notes from this project**

**Add a user:**

sudo adduser *name*


**Make a user a sudoer**

chmod +w /etc/sudoers

*name* ALL=(ALL:ALL) ALL


**Connect to VM with SSH:**

sudo vim /etc/ssh/sshd_config

check that PasswordAuthentication and PermitEmptyPasswords are yes

systemctl restart sshd

in the host: ssh-copy-id new@10.11.203.96 -p 56969

while adding key, the firewall might block ip because of multiple calls. To avoid this either disable firewall before adding ssh-key or put this command in a couple of times: sudo fail2ban-client set ssh unbanip 10.11.3.3


**new key has now been added and can connect using**

*name*@10.11.254.96 -p 6969


**Delete user**

sudo userdel -r *name*


**To check partitions:**

sudo parted

unit GB

print


**From the shell run a command that lets you know if all packages are up to date:**

apt list update

apt list --upgradable


**Check all installed packages:**

apt list --installed


**Check that the DHCP service is disabled:**

sudo vim network/interfaces

primary network interface needs to be

auto enp0s3

sudo vim network/interfaces.d/enp0s3

static instead of DHCP


**Change netmask to something other than /30:**

use https://www.calculator.net/ip-subnet-calculator.html to find port for other number (for example 29 is 255.255.255.248, and use IP found with command ‘ipconfig getoption en0 router’. Modify /etc/network/interfaces.d/enp0s3 address to any of the addresses listed (for example 10.11.254.8) and netmask to 255.255.255.248. Save the file and run the command  sudo systemctl restart networking. To check if it is still active run sudo systemctl status networking


**From the shell of the VM, check that the port of the SSH has been successfully modified. SSH access MUST be done with publickeys. The root user should not be able to connect with SSH:**

sudo vim /etc/ssh/sshd_config

check that PubkeyAuthentication is yes, and PasswordAuthentication & PermitEmptyPasswords are no


**Run the command that lists all firewall rules:**

sudo ufw status


**Run the command that allows you to test a DOS (slowloris)**

to check banned: sudo fail2ban-client banned

to check auth log tail: sudo tail -f /var/log/auth.log

to run fail2ban python3 slowloris.py -p 56969 10.11.203.96 --sleeptime 1

to unban: sudo fail2ban-client set ssh unbanip 10.11.3.3


**Run the command that lists open ports:**

sudo netstat -tulpn | grep LISTEN

**Check if the active services are necessary for proper functioning**

systemctl list-unit-files --state=enabled --type=service


apparmor.service	enabled //for web server

cron.service		enabled //for cron

e2scrub_reap.service	enabled //not sure but scared to disable

fail2ban.service	enabled //for fail2ban

getty@.service	enabled //login

networking.service	enabled //raises or downs the network interfaces

rsyslog.service	enabled //for logs

ssh.service		enabled //for ssh

systemd_pstore.service|enabled //for archives the contents of the Linux persistent psotore

systemd-timesyncd.service|enabled //sync local clock

ufw.service		enabled //for ufw


**Check /usr/scripts for necessary scripts**

monitor mail will appear in /var/mail/otto folder


**Check /etc/crontab**

sudo crontab -e
