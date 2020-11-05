# To install wordpress on centos, install the prerequisites: apache, php, mysql.

```bash
sudo yum install mariadb mariadb-server httpd httpd-tools php php-cli php-gd php-json php-mbstring php-pdo php-mysqlnd php-pecl-zip php-xml wget -y
```
## Verify that all the servers are installed and set to start at boot:

- Apache:
```bash
sudo systemctl status httpd
sudo systemctl enable --now httpd
sudo systemctl status httpd
```
- MariaDB:
```bash
sudo systemctl status mariadb
sudo systemctl enable --now mariadb
sudo systemctl status mariadb
```
## Prepare mysql:

```bash
mysql -u root -p
```

```sql
CREATE DATABASE mywp;
GRANT ALL ON mywp.* TO 'al'@'localhost' IDENTIFIED BY '12345';
FLUSH PRIVILEGES;
```

## Get the latest wordpress:

```bash
cd /var/www
wget https://wordpress.org/latest.tar.gz
tar xzvf latest.tar.gz && rm latest.tar.gz
```

## Change ownership and permissions on wordpress files:

```bash
cd /var/www
chown -Rf apache:apache wordpress
chmod -Rf 755 wordpress
```

## Proceed to the apache config section.
[Apache Config](config.md)

