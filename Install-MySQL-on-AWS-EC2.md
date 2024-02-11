# Command to deploy MySQL on AWS EC2

## 1. Create EC2 Instance
Read on my Blog

## 2. Install Mysql

Login as super user
```
sudo su -
``` 

Install MySQL
```
sudo apt-get update -y
sudo apt install mysql-server
```
Check status of installed MySQL 
```
sudo systemctl status mysql
```

## 3. MySQL setup
Login to MySQL as Root
```
sudo mysql
```
Create root user with password
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'place-your-password-here';

FLUSH PRIVILEGES;
```
Create admin user to connect from other device
```
CREATE USER 'admin'@'%' IDENTIFIED WITH mysql_native_password BY 'your password';

GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%';

FLUSH PRIVILEGES;

```
'%' mean "all IP access", config to specific IP if you want

Next we will need to config MySQL to listen for external IP, because default it just listen to localhost

```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
Navigate to the line that begins with the `bind-address` directive. It will look like this:
```
. . .
bind-address            = 127.0.0.1
```

Set it to 0.0.0.0 like this:

```
bind-address            = 0.0.0.0

```
All done! We need to restart MySQL to make change:

```
sudo systemctl restart mysql
```

Now we can access it from MySQL WorkBench from our machine