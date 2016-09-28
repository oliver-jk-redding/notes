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