#### Becoming a Linux Server Administrator - 1.0 Understanding Server Administration
============================================================

Filename: techskills-linuxserveradmin-1-2-hardening_a_server
Title: Hardening a Server
Subtitle: Becoming a Linux Server Administrator

#### 1.2 Hardening a Server
------------------------------------------------------------

* What do we need to do to secure the server?
	+ Step 4: Secure the server
	+ Configure passwords/certificates
		- `passwd jdoe`
		- `chage -m <mindays> -M <maxdays> -E <expiredate> -W <warndays> jdoe`
		- `visudo`
	+ Configure firewall
		- Determine firewall status
			+ `firewall-cmd --state`
		- Determine default zone
			+ `firewall-cmd --get-zones`
			+ `firewall-cmd --get-default-zone`
		- Allow a standard service through the firewall
			+ `firewall-cmd --get-services` for a list of supported services
				- Default services are stored in `/usr/lib/firewalld/services/*.xml`
				- Custom services can be added in `/etc/firewalld/services/*.xml`
			+ `firewall-cmd --zone=dmz --permanent --add-service=http`
			+ `firewall-cmd --zone=dmz --permanent --list-services`
		- Allow a non-standard service through the firewall
			+ `firewall-cmd --zone=dmz --permanent --add-port=8080/tcp`
			+ `firewall-cmd --zone=dmz --permanent --list-ports`
			+ Can also accept ranges like `10000-20000/udp`
	+ Configure TCP wrappers
		+ Network access control system to filter connections Applies to anything that depends on libwrap (like xinetd)
		+ Determine if a service supports TCP wrappers
			- `ldd /usr/sbin/sshd | grep wrap`
		+ Allow some and deny the rest
			- `/etc/hosts.allow`
		+ Deny some and allow the rest
			- `/etc/hosts.deny
		+ Syntax
			- `sshd : 192.168.0.1`
			-  OR `sshd : 192.168.0.0/255.255.255.0`
			- `ALL : ALL` Deny is checked first, then allow overrides it
	+ Configure SELinux
		- Verify status of SELinux
			+ `sestatus`
		- Define status at time of boot
			+ `vi /etc/selinux/config`
		- Modify status temporarily
			+ `setenforce enforcing` or `setenforce 1`
			+ `setenforce permissive` or `setenforce 0`
			+ `getenforce` to verify
		- View SELinux log
			+ `tail /var/log/audit/audit.log`
		- View SELinux context for a process or file
			+ `ps auxZ | grep httpd`
			+ `ls -laZ /var/www/html/index.html`
	+ Configure service related security
		- Varies based on service
* Once that is all done, how do we know it all worked?
	+ Step 5: Monitor the server
	+ Configure logging (Covered later)
		- *rsyslogd* and *journald*
	+ Create an activity baseline (Covered later)
		- *sysstat* and *sar*
	+ Perform updates
		- *apt*, *yum*, *yum-cron*, and *dnf*
