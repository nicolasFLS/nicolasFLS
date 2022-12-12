# Checklist

## 1) Update Computer 🖥️
* Do sudo apt-get update && sudo apt update && sudo apt upgrade

## 2) Forensic Questions 🔎

* Solve the forensic questions. These may rely on files or programs you will eventually delete, which is why we do them first.

## 3) Configure Update Settings 📦

* Go to software & updates and make sure the update settings are put as:
* Subscribed to all updates
* Automatically check for updates daily
* When there are security updates download and install automatically
* When there are other updates display immediately
* Notify me of a new ubuntu verson for long-term support versions

## 4) Privacy Settings ⚙️

* In setting go to privacy and go to file history and trash
* Make sure the settings are set as:
* File history off
* Automatically delete trash content
* Automatically delete temporary files
* Automatically delete period 1 hour

## 5) Remove Users 👤

* Remove any users that aren't supposed to have access on the computer
* Remove admin abilities of users that aren't supposed to have admin

## 5) Root Access 🚫

* Run awk -F: '($3=="0") {print}' /etc/passwd
* Make sure root is the only thing showing
* If root is not the only thing showing remove the user (if they aren't supposed to be on the computer)

## 6) Check For Empty Passwords

* Run sudo cat /etc/shadow | awk -F: '($2=="") {print $1}'
* If anything shows make it so they change their password at next login or give them you're password

## 7) Configure SSH

* Run sudo nano /etc/ssh/sshd_config
* Go find and configure things as:
* PermitRootLogin no
* Port 22222
* PermitEmptyPasswords no
* Then save and exit and in the terminal run service ssh restart

## 8) Remove Unwanted Files With Bleachbit

* Install bleachbit with sudo apt install bleachbit
* Run bleachbit as root

## 9) Check For Rootkits 🔒

* Install Rootkit Hunter with sudo apt install rkhunter
* Run Rookit Hunter with `sudo rkhunter -c`
* The following may result in positives, but are safe to leave:
* `/usr/bin/unhide.rb`
* `passwd` file changes
* Group file changes
* `/dev/.udev/rules.d/root.rules`
* `/dev/.udev`
* `/etc/.java`
* `/dev/.initramfs`
* Rootkit Hunter will detail its findings in `/var/log/rkhunter.log`
* If Rootkit Hunter finds anything aside from `unhide.rb`, agree to let it clean the system
* Set Rootkit Hunter to run periodically by opening `/etc/default/rkhunter` and changing `CRON_DAILY_RUN` and `CRON_DB_UPDATE` to true

## 10) Run Lynis 📝

* Lynis gives an incredibly comprehensive system audit report
* Install lynis with sudo apt install lynis
* Run sudo lynis audit system to begin the report

## 11) Check Password Files 🔑

* Check hidden users or undesirable groups.
* sudo gedit /etc/passwd
* sudo gedit /etc/shadow
* sudo gedit /etc/sudoers.d

## 12) Check Log Files 📄

* Log files can allow you to find potential security issues caused by malicious activity.
* /var/log/messages
* /var/log/boot
* /var/log/debug
* /var/log/auth.log
* /var/log/daemon.log
* /var/log/kern.log
* /var/log/dpkg.log
* The dpkg log can be very useful to find programs installed around the same time as known malware

## 11. Check Running Processes 💿

* `ps aef` lists all processes. You might want to scan these to see if anything weird is listed
* `ps aux | grep netcat` returns all running processes with `netcat` in the name. Netcat is a networking utility that we don't want on the machine.
* Note: If Netcat isn't running, the above command will still return one entry for `grep --color=auto netcat` (the process you invoke by running the command)
* htop is a useful tool for monitoring processes. Learn more [here](https://www.tecmint.com/htop-cpu-monitoring-tool-in-linux/)
* Learn more about `ps` [here](https://www.computernetworkingnotes.com/linux-tutorials/ps-aux-command-and-ps-command-explained.html)

## 12. Configure Firefox 🦊

* Go through Firefox settings and make everything as secure as possible. Make sure Firefox is updated before you do this, as settings may change between versions.

## 13. Check for Startup Programs 🚀

* Use `sudo nano <file name>` to check the contents of the following files:
* /etc/rc.local
* /etc/crontab
* ~/.config/autostart
* /etc/xdg/autostart
* /etc/rc0.d
* /etc/rc1.d
* /etc/rc2.d
* /etc/rc3.d
* /etc/rc4.d
* /etc/rc5.d
* /etc/rc6.d
* /etc/rcS.d
* /etc/cron.d
* /etc/cron.daily
* /etc/cron.hourly
* /etc/cron.monthly
* /etc/cron.weekly
* /etc/init.d

## 14. Ensure postfix is configured for local use only 📬

* Postfix should be removed, but I think Lynis installs it by default, so if you're able to install Lynis, you'll need to configure postfix to only send mail locally.
* `sudo nano /etc/postfix/main.cf`
* Add or modify the following line in the `RECEIVING MAIL` section:
* `inet_interfaces = loopback-only`
* Restart postfix with `sudo systemctl restart postfix`

## 5. Check Ports 🚤

* Run `sudo lsof -i -P -n | grep -v "(ESTABLISHED)"` to list all services listening on ports
* Run `sudo apt purge <package_name>` to remove any unwanted services
* If you can't remove the package for dependency reasons, you can stop and mask the service with `sudo systemctl --now mask <service_name>`
