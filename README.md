# clerouxfr.github.io


```dataview
table
From "AKIO"

```
## Prepare all ALL VMs


désactivation du firewall
```Almalinux
systemctl stop firewalld
```
```Almalinux
systemctl disable firewalld
```

```Almalinux
vi /etc/sysconfig/network-scripts/ifcfg_xxx
```
BOOTPROTO=none
IPADDR=10.33.50.180
PREFIX=24
GATEWAY=10.33.50.254
DNS1=10.33.50.51
ONBOOT=yes

==Reboot==


```Almalinux
nano /etc/sysconfig/selinux
```

```Almalinux
SELINUX=disabled
```

```Almalinux
nano /etc/sysctl.conf
```

```Almalinux
fs.file-max = 524288
net.ipv6.conf.all.disable_ipv6=1
```
```Almalinux
nano /etc/security/limits.conf
```

```Almalinux
akio     hard      nproc   10000
akio     soft      nproc   10000
akio     -         nofile    30000
```
installation des outils
```Almalinux
dnf install unzip tar wget nano mlocate
```

mise a jour de la base de donnée de recherche pour l'outil 'locate'
```Almalinux
updatedb
```

```Almalinux
nano /etc/hosts
```
IPADDR              FQDN   HOST

```Almalinux
nano /etc/hostname
```
HOST


Création de  l’utilisateur akio [[DONNER LES DROIT ROOT À UN UTILISATEUR]]
```Almalinux
/usr/sbin/useradd akio -c "AKIO's user account" -d /var/akio -G daemon -m

chmod 755 /var/akio
su – akio
```

```Almalinux
[akio] mkdir /var/akio/installer

[akio] cd /var/akio/installer
```
Déposer ici les fichiers ZIP

- ***uic-installer-7.x.y-package-almalinux8.zip (uniquement pour core)**
- ***uic-installer-7.x.y-system-almalinux8.zip**

```Almalinux
su - root
```

```Almalinux
Chown -R akio.akio /var/akio
```


==REBOOT --->> snapshots all VMs==
