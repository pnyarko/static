# University of Leodis Solution Document

## Assumptions
1. The user running the playbook has the correct sudo rights.
2. The user on the server has generated public/private key and has added the public key to authorized_keys file using ssh-copy-id on the target servers in the inventory list in order for ansible to run locally.

## Invocation
First copy the playbook.yml from the leodis-apache2 directory and invoke `ansible-playbook playbook.yml -i {inventory}`
Assuming there's an inventory file with the target servers.  This is an idempotent script and can be run as many times as necessary. Should you encounter issue with apt please manually run  'apt-get update'.

Startup
Apache2 has been set start from boot up.

All /opt/apache directory files can now be safely archived.

## Upgrades and future enhancements

Apache
Apache2/utils 2.2.22 stable version is being used and the version is locked under `tasks/leodis.yml "apache2-utils=2.2.22-1ubuntu1.11" & "apache2=2.2.22-1ubuntu1.11" to avoid accidental upgrades.  Should the need for an upgrade be necessary, the version number can be changed.  

Vhosts
The desired vhost configuration file can be created and added to the templates/etc/apache2 with the j2 extension. Then the 

Modules
Additional modules can be enabled by editing `/leodis-apache2/tasks/leodis.yml` and adding the names of the desired modules to the modules list.

## Analysis 
There are a currently 104 security vulnerabilities in for Ubuntu LTS 12
https://www.cvedetails.com/version/127611/Canonical-Ubuntu-Linux-12.04.html

Ubuntu LTS 12 is officially on End of Life notice except Ubuntu Advantage Customers who are only Entitled to  Extended security maintenance.
https://www.ubuntu.com/info/release-end-of-life.

The httpd service was started manually and will not be available after a reboot or when an alternate init state is transitioned into.

The apache instance was running as root from /opt/apache.  That instance is deprecated and the new instance is being used.  A repository can be created within the campus network to avoid unnecessary traffic to pull the install binaries for apache2.

## Critical Security & System Issues
All files in the /opt/directory had the wrong permissions allowing any user access to modify and run executables from that location.

  1. No password set on GRUB bootloader.
  2. Couldn't find 2 responsive nameservers. 
  3. Root can directly login via SSH. (Remedied)
  4. Found SSL certificate expiration (/etc/ssl/certs/ca-certificates.crt).
  5. No immediate perimeter protection is available - UFW is NOT running. (Remedied)
  6. Ansible version 1.9.4 s deprecated and requires and upgrade to 2.x.

## Important Security / Vulnerability Issues

   1. Update to the latest stable release.
   2. Configure password aging limits to enforce password changing on a regular basis.
   3. Default umask in /etc/profile could be more strict like 027. 
   4. Default umask in /etc/login.defs could be more strict like 027. 
   5. Default umask in /etc/init.d/rc could be more strict like 027. 
   6. To decrease the impact of a full /home file system, place /home on a separated partition
   7. To decrease the impact of a full /tmp file system, place /tmp on a separated partition 
   8. Disable drivers like USB storage when not used, to prevent unauthorized storage or data theft 
   9. Disable drivers like firewire storage when not used, to prevent unauthorized storage or data theft 
   10. Install package apt-show-versions for patch management purposes 
   11. Check your resolv.conf file and fill in a backup nameserver if possible 
   12. Configure a firewall/packet filter to filter incoming and outgoing traffic 
   13. Add legal banner to /etc/issue, to warn unauthorized users. 
   14. Add legal banner to /etc/issue.net, to warn unauthorized users.
   15. Enable auditd to collect audit information. 
   16. Renew SSL expired certificates. 
   17. Harden the system by removing unneeded compilers. This can decrease the chance of customized trojans, backdoors and rootkits to be compiled and installed.
   18. Harden the system by installing one or malware scanners to perform periodic file system scans.
## Conclusion
The current solution presented solves an intermediate issue and allows the application to conform to basic application and security standards however overall the system requires hardening urgently as it risks becoming a backdoor for potential maliciously intended users.
