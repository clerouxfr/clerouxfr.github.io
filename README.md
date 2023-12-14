# clerouxfr.github.io


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

### system

```Almalinux
unzip -o uic-installer-7.30.3-system-almalinux8.zip
```
```Almalinux
nano /var/akio/installer/SysInstaller-AutoInstall.xml
```
Adapter le fichier  **SysInstaller-AutoInstall.xml**
```Almalinux
./UicSysInstaller-almalinux8-7.30.3.sh -- -s /var/akio/installer/SysInstaller-AutoInstall.xml
```

<span style="color:#92d050">Test elasticsearch</span>
```Almalinux
systemctl status elasticsearch
```

```sh

● elasticsearch.service - Elasticsearch
   Loaded: loaded (/usr/lib/systemd/system/elasticsearch.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2023-11-21 13:33:02 EST; 2 days ago
     Docs: https://www.elastic.co
 Main PID: 899 (java)
    Tasks: 79 (limit: 47670)
   Memory: 4.6G
   CGroup: /system.slice/elasticsearch.service
           ├─ 899 /usr/share/elasticsearch/jdk/bin/java -Xshare:auto -Des.networkaddress.cache.ttl=60 -Des.networkaddress.cache.negative.ttl=10 -XX:+AlwaysPreTouch -Xss1m -Djava.awt.headless=true -Dfile.enco>
           └─2193 /usr/share/elasticsearch/modules/x-pack-ml/platform/linux-x86_64/bin/controller

Nov 21 13:32:23 elastic.company.com systemd[1]: Starting Elasticsearch...
Nov 21 13:32:23 elastic.company.com systemd-entrypoint[899]: env-akio.sh (unspecified version) loaded
Nov 21 13:32:23 elastic.company.com systemd-entrypoint[899]: env-akio.sh (unspecified version) loaded
Nov 21 13:32:23 elastic.company.com systemd-entrypoint[899]: overriding file for elasticsearch not defined.
Nov 21 13:32:23 elastic.company.com systemd-entrypoint[899]: Elasticsearch jvm options :
Nov 21 13:32:23 elastic.company.com systemd-entrypoint[899]: -Xms4g -Xmx4g -Dcom.sun.management.jmxremote.port=8584 -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false >
Nov 21 13:33:02 elastic.company.com systemd[1]: Started Elasticsearch.
lines 1-18/18 (END)
```


```Almalinux
curl -u 'akio:superuser!2K23' -X GET 'http://localhost:9200/_cat/indices'
```

```sh
green open phone_activities_10ee4f7a-a2ac-4bb1-8f26-0383a78dbac7-000001  xiu7UhouR_Ko5T4dIs0XBA 1 0   0 0    226b    226b
green open ivr_cdr_-000001                                               5kGf4O_rQMeAuR8CULYBVw 1 0   0 0    226b    226b
green open statcallsqueue_10ee4f7a-a2ac-4bb1-8f26-0383a78dbac7-000001    o2yZoqRoR6u9Jpxqj272Lw 1 0   0 0    226b    226b
green open call_stats_10ee4f7a-a2ac-4bb1-8f26-0383a78dbac7-000001        7a2YiUyBQfe5eubOJS03CQ 1 0   0 0    226b    226b
green open realtime_counters_10ee4f7a-a2ac-4bb1-8f26-0383a78dbac7-000003 445mxCKlQFqUZ8LXUS9VQQ 1 0   0 0    226b    226b
green open realtime_counters_10ee4f7a-a2ac-4bb1-8f26-0383a78dbac7-000002 BRfUX-jnRA6wFoLPZ9rLvw 1 0 208 0 106.1kb 106.1kb
green open realtime_counters_10ee4f7a-a2ac-4bb1-8f26-0383a78dbac7-000001 _maxUi3OSraRyzLSsPhB-A 1 0  71 0 136.4kb 136.4kb
green open phone_activities_10ee4f7a-a2ac-4bb1-8f26-0383a78dbac7-000002  Latx7OA1TU-muQ5R9CtDWQ 1 0   0 0    226b    226b
green open phone_activities_10ee4f7a-a2ac-4bb1-8f26-0383a78dbac7-000003  ujGfUyMsTp-Fhhg-g8Ewhg 1 0   0 0    226b    226b
green open .geoip_databases                                              MryxnNoQQcC8Lt7h24gLPg 1 0  40 5  37.5mb  37.5mb
green open interaction_10ee4f7a-a2ac-4bb1-8f26-0383a78dbac7              7daEwfo2TF2t5DkG0U3o1g 1 0   2 0  15.3kb  15.3kb
green open .security-7                                                   ghBkofKvQtyORRbbGM2-zg 1 0   2 0   8.3kb   8.3kb
green open realtimelogs_evm_10ee4f7a-a2ac-4bb1-8f26-0383a78dbac7-000001  QrlDbPEkRwm0tlfZyYNlhw 1 0   8 0  59.3kb  59.3kb
green open statcallsagent_10ee4f7a-a2ac-4bb1-8f26-0383a78dbac7-000001    x1mC3dAvSmqcJGlmM4qIzQ 1 0   0 0    226b    226b
green open ale_ws_io-000001                                              vwa7ws7CSf-0_d_UIGmQWQ 1 0   0 0    226b    226b
green open ivr_cdr_10ee4f7a-a2ac-4bb1-8f26-0383a78dbac7-000001           ffNAztrYTlmBzBXa1slSNg 1 0   0 0    226b    226b
```
#### system
```Almalinux
unzip -o uic-installer-7.30.3-system-almalinux8.zip
```

Adapter le fichier SysInstaller-AutoInstall.xml
```Almalinux
./UicSysInstaller-almalinux8-7.30.3.sh -- -s /var/akio/installer/SysInstaller-AutoInstall.xml
```

```Almalinux
systemctl stop postgresql-14
```

```Almalinux
nano /var/akio/db/UIC/pg_hba.conf
```

```sh
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     ident
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
host    all             all             10.33.50.180/32            trust
# IPv6 local connections:
host    all             all             ::1/128                 ident
# Allow replication connections from localhost, by a user with the
# replication privilege.
#local   replication     postgres                                trust
#host    replication     postgres        127.0.0.1/32            trust
#host    replication     postgres        ::1/128                 trust

```

>host    all             all             IP ADDRESS CORE SERVER/32       trust

```Almalinux
host    all             all             10.33.50.180/32            trust
```

```Almalinux
systemctl start postgresql-14
```

```Almalinux
su - akio
```

```Almalinux
psql -U postgres
```

```sql
psql (14.7)
Saisissez « help » pour l'aide.

postgres=# \l
                                   Liste des bases de données
    Nom    | Propriétaire | Encodage | Collationnement | Type caract. |     Droits d'accès
-----------+--------------+----------+-----------------+--------------+-------------------------
 bi        | postgres     | UTF8     | en_US.UTF-8     | en_US.UTF-8  | =Tc/postgres           +
           |              |          |                 |              | postgres=CTc/postgres  +
           |              |          |                 |              | bi_select=C/postgres
 postgres  | postgres     | UTF8     | en_US.UTF-8     | en_US.UTF-8  | =Tc/postgres           +
           |              |          |                 |              | postgres=CTc/postgres
 template0 | postgres     | UTF8     | en_US.UTF-8     | en_US.UTF-8  | =c/postgres            +
           |              |          |                 |              | postgres=CTc/postgres
 template1 | postgres     | UTF8     | en_US.UTF-8     | en_US.UTF-8  | =c/postgres            +
           |              |          |                 |              | postgres=CTc/postgres
 user6     | postgres     | UTF8     | en_US.UTF-8     | en_US.UTF-8  | =Tc/postgres           +
           |              |          |                 |              | postgres=CTc/postgres  +
           |              |          |                 |              | user6_select=C/postgres
(5 lignes)

postgres=#
```

* \\q pour quitter
* \\l pour lister les bases
* \\c database_name pour changer de bdd
* \\dt pour lister les tables 

#### system
```Almalinux
unzip -o uic-installer-7.30.3-system-almalinux8.zip
```

```Almalinux
./UicSysInstaller-almalinux8-7.30.3.sh -- -s /var/akio/installer/SysInstaller-AutoInstall.xml --ale
```

Sous root | le fichier <span style="color:#00b050">SysInstaller-AutoInstall.xml</span> doit être adapté
#### package
```Almalinux
unzip -o uic-installer-7.30.3-package-almalinux8.zip
```

```Almalinux
chown -R akio.akio /var/akio
```


```Almalinux
su - akio
```

```Almalinux
cd installer
```

Le script <span style="color:#00b050">generateConfig</span> va déployer le répertoire /var/akio/installer-data
```Almalinux
./bin/start.sh -generateConfig
```

On surcharge les variables d'environnement
```Almalinux
nano /var/akio/conf.d/env-akio.sh
```

```sh
export SRV_DB=10.33.50.182
export PGHOST=10.33.50.182
export SQL_UIC_USER="/usr/bin/psql user6 -Ugandalf -h $PGHOST"
export SQL_UIC_BI="/usr/bin/psql bi -Ugandalf -h $PGHOST"
export SQL_UIC_ADMIN="/usr/bin/psql user6 -Ugandalf -h $PGHOST"
export SQL_AKIO_OWNER="/usr/bin/psql user6 -Uakio  -h  $PGHOST"
```
==SRV_DB , PGHOST==

le fichier <span style="color:#00b050">autoInstall.xml</span> doit être adapté
```Almalinux
./bin/start.sh -s /var/akio/installer-data/autoInstall.xml
```

Control the DSN with DB SERVER IP ADDR
```Almalinux
nano /var/akio/uic-exploitation/data/conf/exploitation.properties
```

```Almalinux
cp /var/akio/uic-exploitation/data/conf/env-tomcat* /var/akio/uic-exploitation/data/conf.d
```



```
nano /var/akio/aas/data/conf/oauth2-clients.xml
```

<span style="color:#00b050">oauth2-clients.xml</span> : Replace the FQDN field with the IP address

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<oauth2ClientsDetails xmlns="http://www.akio.com/aas/oauth2/client/1">
        <client clientId="CIM_user" clientSecret="changepassword1">
                <authorities>
                        <authority>ROLE_CLIENT</authority>
                        <authority>ROLE_TRUSTED_CLIENT</authority>
                </authorities>
                <scopes>
                        <scope>all</scope>
                </scopes>
                <authorizedGrantTypes>
                        <grantType>password</grantType>
                        <grantType>authorization_code</grantType>
                        <grantType>refresh_token</grantType>
                        <grantType>client_credentials</grantType>
                </authorizedGrantTypes>
                <resourcesIds>
                        <id>CIM_api</id>
                        <id>CIM_AAS_api</id>
                </resourcesIds>
                <registeredRedirectUri>
                        <uri>https://hspakiocore.hospitality.com/jamc/user/login</uri>
                        <uri>https://hspakiocore.hospitality.com/jamc/admin/login</uri>
                </registeredRedirectUri>

etc ...
```

```Almalinux
nano env-tomcat0.sh
```

Ne conserver que cette ligne !
```Almalinux
export URL_STATUS=http://FQDN/admin/resources/v1/application/status
```

- [ ] env-tomcat0.sh 
- [ ] env-tomcat1.sh
- [ ] env-tomcat2.sh
- [ ] env-tomcat3.sh
- [ ] env-tomcat5.sh

## installation des Extensions

Copier l'ensemble des fichiers zip sous <span style="color:#00b050">/var/akio/SOURCES/SPECIFIC/</span>

```Almalinux
unzip -o '*.zip'
```

```Almalinux
chmod 750 -R /var/akio
```

```Almalinux
chown -R akio.akio /var/akio
```

```Almalinux
./install.sh
```

- [ ] livraison_7.30_akio_externalcrm_7.30.1-20230713-140359
- [ ] livraison_7.30_akio_router_7.30.1-20230713-134045
- [ ] livraison_7.30_akio_specific_api_7.30.1-20230713-141328
- [ ] livraison_7.30_akio_ui_extensions_7.30.1-20230713-140927
- [ ] livraison_7.30_akio_customerimport_7.30.1-20230713-134824
- [ ] livraison_7.30_akio_webhooks_7.30.1-20230713-135720
- [ ] livraison_webhooks-hub-3.0.1-20230331-1506 <span style="color:#c00000">( as root )</span>
- [ ] livraison_akio_webhooks-hub-db-1.0.1_20230309-143533 <span style="color:#c00000">( as root )</span>

---

>Dans le fichier ==uic-admin-configuration.xml==  sous /var/akio/admin/data/conf.d/
 remplacer localhost par l'@ip CORE pour afficher l'onglet des extensions dans le webadmin

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config>
  <integration>
    <menu>http://10.33.50.180:8080/jamc/rest/personalized/menu?operatorId=@|operator_id|</menu>
  </integration>
</config>
```


```Almalinux
su - akio
```

```Almalinux
$UIC start user
```


```Almalinux 
nano /etc/postfix/main.cf
```


<table>
    <tr>
        <th>remplacez cette ligne</th>
        <td>par cette ligne</td>
    </tr>
    <tr>
        <th bgcolor="darkgrey">alias_maps = hash:/etc/aliases</th>
        <td bgcolor="darkgreen">alias_maps = hash:/var/akio/user/data/conf/aliases, hash:/etc/aliases</td>
    </tr>
    <tr>
        <th bgcolor="darkgrey">alias_database = hash:/etc/aliases</th>
        <td bgcolor="darkgreen">alias_database = hash:/etc/aliases, hash:/var/akio/user/data/conf/aliases</td>
    </tr>
    <tr>
        <th bgcolor="darkgrey">inet_interfaces = localhost</th>
        <td bgcolor="darkgreen">inet_interfaces = all</td>
    </tr>
</table>
---

```Almalinux
systemctl restart postfix.service
```

```Almalinux
systemctl status postfix.service
```

```Almalinuxcat 
/var/akio/user/data/conf/aliases
```
```Almalinux
systemctl restart postfix.service
```
```Almalinux
systemctl status postfix.service
```

```Almalinux
su - akio
```
```Almalinux
dnf install fetchmail
```
```Almalinux
systemctl enable fetchmail
```
```Almalinux
systemctl start fetchmail
```
```Almalinux
systemctl status fetchmail
```
```Almalinux
su - akio
```
```Almalinux
dnf install telnet
```
>Do a test : telnet email_server_ip_address 143


```Almalinux
cd /var/log
```

```Almalinux
systemctl status fetchmail.service
```
```Almalinux
cd /var/log/maillog
```
```Almalinux
cd /var/log/
```

```Almalinux
cat maillog
```

```Almalinux
su - akio
```
```Almalinux
mkdir rc.fetchmail
```
```Almalinux
nano /var/akio/user/data/conf/rc.fetchmail/fetchmail.ale.conf
```
```config
poll <email server ip address> with proto IMAP port 143
user "mail.pod2@company.com" there with password "superuser" is "mail.pod2" options sslproto ''
fetchlimit 10
keep
```

```Almalinux
crontab -l
```
> empty list
```Almalinux
crontab -e
```

```Almalinux
*/5 * * * * . ~/env-akio.sh ; /usr/bin/fetchmail -f /var/akio/user/data/conf/rc.fetchmail/fetchmail.ale.conf
```


```Almalinux
crontab -l
```

```Almalinux
*/5 * * * * . ~/env-akio.sh ; /usr/bin/fetchmail -f /var/akio/user/data/conf/rc.fetchmail/fetchmail.ale.conf
```

```Almalinux
chmod 700 /var/akio/user/data/conf/rc.fetchmail/fetchmail.ale.conf
```

```Almalinux
/usr/bin/fetchmail -f /var/akio/user/data/conf/rc.fetchmail/fetchmail.ale.conf
```

>Email server account authentication ......






