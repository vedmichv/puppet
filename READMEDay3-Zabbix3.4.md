# Install zabbix by hand

## Add the Zabbix repository
```bash
rpm -ivh http://repo.zabbix.com/zabbix/3.4/rhel/7/x86_64/zabbix-release-3.4-1.el7.centos.noarch.rpm
```

## Install the packages and dependencies
```bash
yum -y install mysql mariadb-server httpd php
yum -y install zabbix-server-mysql zabbix-agent zabbix-web-mysql
```

## Configure the database
Install mysql module
```bash
systemctl start mariadb
mysql_secure_installation

mysql -u root -p
create database zabbix;
grant all privileges on zabbix.* to zabbix@localhost identified by 'Zabbix_2017';
flush privileges;
exit
```
Fill zabbix database
```bash
zcat /usr/share/doc/zabbix-server-mysql-3.4.1/create.sql.gz | mysql -uzabbix -pZabbix_2017
```


## Configure the webserver
```bash
sed -i 's/^max_execution_time.*/max_execution_time=600/' /etc/php.ini
sed -i 's/^max_input_time.*/max_input_time=600/' /etc/php.ini
sed -i 's/^memory_limit.*/memory_limit=256M/' /etc/php.ini
sed -i 's/^post_max_size.*/post_max_size=32M/' /etc/php.ini
sed -i 's/^upload_max_filesize.*/upload_max_filesize=16M/' /etc/php.ini
sed -i "s/^\;date.timezone.*/date.timezone=\'Europe\/Brussels\'/" /etc/php.ini
```

## Configure apache for zabbix
```bash
vim /etc/httpd/conf.d/zabbix.conf
```
```config
Alias /zabbix /usr/share/zabbix

<Directory "/usr/share/zabbix">
    Options FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>

<Directory "/usr/share/zabbix/conf">
    Require all denied
</Directory>

<Directory "/usr/share/zabbix/include">
    Require all denied
</Directory>
```

## Configure Zabbix parameters:
```bash
sed -i 's/^# DBPassword=.*/DBPassword=Zabbix_2017/' /etc/zabbix/zabbix_server.conf
sed -i 's/^# CacheSize=.*/CacheSize=32M/' /etc/zabbix/zabbix_server.conf
sed -i 's/^# StartPingers=.*/StartPingers=5/' /etc/zabbix/zabbix_server.conf
```

## Security considerations
```bash
firewall-cmd --zone=public --add-port=80/tcp --permanent
#With iptables:
iptables -I INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
#When using SELinux, we need to allow Apache to communicate with Zabbix:
setsebool -P httpd_can_connect_zabbix=1
```

## Start and initialize Zabbix
```bash
systemctl start zabbix-agent
systemctl start zabbix-server
systemctl start httpd

systemctl enable zabbix-agent
systemctl enable zabbix-server
systemctl enable httpd
systemctl enable mariadb
```
