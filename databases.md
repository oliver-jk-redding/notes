#Notes on Databases

##How to export to and import from a mysql dump

###Export database to sql file
```bash
mysqldump -u $username -p$password -h $hostname -P $port $database_name > $file.sql
```
e.g.
```bash
mysqldump -u root -proot -h 127.0.0.1 -P 3306 wordpress > dump.sql
```
Note: if the database is local, you usually won't need to put the hostname and you will only need the port if there are multiple databases hosted locally. So usually the following will be fine:
```bash
mysqldump -u root -proot wordpress > dump.sql
```
###Import database from sql file
```bash
mysql -u $username -p$password -h $hostname -P $port $database_name < $file.sql
```
e.g.
```bash
mysql -u root -proot -h 127.0.0.1 -P 3306 wordpress < dump.sql
```
Note: again, if the database is local, you usually won't need to put the hostname and you will only need the port if there are multiple databases hosted locally. So usually the following will be fine:
```bash
mysql -u root -proot wordpress < dump.sql
```

##How to connect to an ubuntu EC2 instance via MySQL utility
1. **Ensure the EC2 instance security settings allow for remote connections.**
Login to the instance on the AWS console and edit the security group for the instance. Set TCP traffic to allow for connections from either local IP address or all IP addresses (less secure but more flexible).
2. **Install mysql on instance and create database with permissions.**
	SSH into the EC2 instance and setup mysql. Set the mysql password as root:
	```bash
	sudo chmod 400 pemfile.pem
	ssh -i path/to/pemfile.pem ubuntu@ec2-url-or-ip
	sudo apt-get update
	sudo apt-get install mysql-server
	sudo mysql_install_db
	sudo mysql_secure_installation
	mysql -u root -proot
	GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION; FLUSH PRIVILEGES; CREATE DATABASE IF NOT EXISTS db;
	exit
	```
3. **Modify mysql config to allow for remote connections**
	```bash
	sudo nano /etc/mysql/my.cnf
	```
	Look for the [mysqld] section. Comment out the following lines:
	```bash
	# skip-external-locking
	# skip-networking
	# bind-address = 127.0.0.1
	```
	Another option is to leave the bind-address option uncommented but to change the line to the following:
	```bash
	bind-address = 0.0.0.0
	```
	Save changes and restart mysql:
	```bash
	sudo service mysql restart
	exit
	```
4. **You should now be able to connect to the instance via the mysql utility**
	```bash
	mysql -u root -proot -h ec2-url-or-ip
	```