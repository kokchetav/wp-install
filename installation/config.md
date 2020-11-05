# configure apache for wordpress

```bash
vim wp.conf
```
```
<VirtualHost *:80>
	ServerAdmin root@localhost
	DocumentRoot /var/www/wordpress
	<Directory "/var/www/wordpress">
		Options Indexes FollowSymLinks
		AllowOverride all
		Require all granted
	</Directory>
	ErrorLog /var/log/httpd/wordpress-error.log
	CustomLog /var/log/httpd/wordpress.log common
</VirtualHost>
```

**All commands are to be run with sudo or as root**
- Copy the apache config file to apache dir and test syntax for errors:
```bash
cp wp.conf /etc/httpd/conf.d/wp.conf
httpd -t
```
- Restart apache to get the new settings:

```bash
systemctl restart httpd
```
- Check if we can see the page:
```bash
IP=$(hostname -I)
curl -I http://$IP
##output:##
#  HTTP/1.1 302 Found
#  Date: Thu, 05 Jun 2021 06:08:00 GMT
#  Server: Apache/2.4.37 (centos)
#  X-Powered-By: PHP/7.2.24
#  Location: http://192.168.1.212/wp-admin/setup-config.php
#  Content-Type: text/html; charset=UTF-8
####
```
- Check SELinux status and relax it 

```bash
getenforce
setenforce 0
```
- Check that apache is bound to port 80 on default interface:
```bash
ss -tl

###output:####
#	# State              Recv-Q   ocal Address:Port        Peer Address:Port             
#	LISTEN             0                                    0.0.0.0:ssh                                  0.0.0.0:*                
#	LISTEN             0                                        *:websm                                      *:*                
#	LISTEN             0                                         *:mysql                                      *:*                
#	LISTEN             0                                         *:http                                       *:*                
##	LISTEN             0                                      [::]:ssh                                     [::]:*     
#############
```

- Add http to firewalld rules:
```bash
firewall-cmd --list-services

###output:###
# cockpit dhcpv6-client ssh
###

firewall-cmd --add-service=http
firewall-cmd --list-services

###output:###
# cockpit dhcpv6-client ssh http
###

```
- Continue installation in the browser.

![Browser config screen](wordpress.png)
