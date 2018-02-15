# University of Leodis Solution Document

## Assumptions
1. The user running the playbook has the correct sudo rights.
2. The user on the server has generated public/private key and has added the public key to authorized_keys file using ssh-copy-id on the target servers in the inventory list.

## Invocation
`ansible-playbook -i {inventory} leodis-apache2/playbook.yml`
Assuming there's an inventory file with the target servers.  This is an idempotent script and can be run as many times as necessary. Should you encounter issue with apt please manually run  'apt-get update'.

Startup
Apache2 has been set start from boot up.


## Upgrades and future enhancements

Apache
Apache2/utils 2.2.22 stable version is being used and the version is locked in the ( ) to avoid accidental upgrades.  Should the need for an upgrade be necessary, the version number can be changed.  

Vhosts
The desired vhost configuration file can be created and added to the templates/etc/apache2 with the j2 extension. Then the 

Modules
Additional modules can be enabled by editing ( ) and adding the names of the desired modules.

# Security and Issues
There are a currently 104 security vulnerabilities in for Ubuntu LTS 12
https://www.cvedetails.com/version/127611/Canonical-Ubuntu-Linux-12.04.html
Ubuntu LTS 12 is officially on End of Life notice except Ubuntu Advantage Customers who are only Entitled to  Extended security maintenance.
https://www.ubuntu.com/info/release-end-of-life.

The httpd service was started by hand and will not be available after a reboot or when an alternate init state is transitioned.

Firstly the apache instance was running as root from /opt/apache.  That instance is deprecated and the new instance is being used.  A repository can be created within the campus network to avoid unnecessary traffic to pull the install binaries for apache2.

# Critical Security Issues
All files in the /opt/directory had the wrong permissions allowing any user access to modify and run executables from that location.

   No password set on GRUB bootloader.
   Couldn't find 2 responsive nameservers. 
   Root can directly login via SSH.
   Found SSL certificate expiration (/etc/ssl/certs/ca-certificates.crt).

  #Important Security / Vulnerability Issues

   Update to the latest stable release.
   Run grub-md5-crypt and create a hashed password. After that, add a line below the line saying timeout=<value>: password --md5 <password hash> 
   Configure password aging limits to enforce password changing on a regular basis.
   Default umask in /etc/profile could be more strict like 027. 
   Default umask in /etc/login.defs could be more strict like 027. 
   Default umask in /etc/init.d/rc could be more strict like 027. 
   To decrease the impact of a full /home file system, place /home on a separated partition
   To decrease the impact of a full /tmp file system, place /tmp on a separated partition 
   Disable drivers like USB storage when not used, to prevent unauthorized storage or data theft 
   Disable drivers like firewire storage when not used, to prevent unauthorized storage or data theft 
   Install package apt-show-versions for patch management purposes 
   Check your resolv.conf file and fill in a backup nameserver if possible 
   Configure a firewall/packet filter to filter incoming and outgoing traffic 
   Add legal banner to /etc/issue, to warn unauthorized users. 
   Add legal banner to /etc/issue.net, to warn unauthorized users.
   Enable auditd to collect audit information. 
   Renew SSL expired certificates. 
   Harden the system by removing unneeded compilers. This can decrease the chance of customized trojans, backdoors and rootkits to be compiled and installed.
   Harden the system by installing one or malware scanners to perform periodic file system scans.

