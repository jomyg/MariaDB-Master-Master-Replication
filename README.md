# MariaDB Master-Master/Slave Replication


[![Build](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

## Description

Replication in SQL databases is the process of copying data from the source database to another one (or multiple ones) and vice versa. Data from one database server are constantly copied to one or more servers. You can use replication to distribute and balance requests  across a pool of replicated servers, provide failover and high availability of MariaDB databases. The  MariaDB (and MySQL) allows to use two types database replication mades: Master-Master and Master-Slave.

In a Master-Master replication scheme, any of the MariaDB/MySQL database servers may be used both to write or read data. Replication is based on a special binlog file, a Master server saves all operations with the database to. A Slave server connects to the Master and applies the commands to its databases.

## Pre-Requests
```
One Mysql client server : AWS linux Ec2
Two DB servers named DB1 and DB2 for our Master-Master/Slave Replication with private IPs
```

### Lets go to the deployment

Install mariadb-server on both DB1 and DB2
```
yum install mariadb-server -y
systemctl mariadb.service enable
systemctl mariadb.service start
```
After installation we can enable the log_bin on both DB servers.
```
vim /etc/my.cnf.d/server.cnf
```
On DB1
```
[mysqld]

server-id=1
log_bin=/var/log/mariadb/mariadb-bin.log
```
On DB1
```
[mysqld]

server-id=2
log_bin=/var/log/mariadb/mariadb-bin.log
```
Restart mariadb on both servers

Now we can setup root password using "mysql_secure_installation" and create a secure password on both DB servers.

Restart mariadb on both servers


### Configuring Simple Master-Master Replication on both MariaDB:
Access mysql on DB1:
```
mysql -u root -p
grant replication slave on *.* to master_user1 identified by 'password';
flush privileges;
flush tables with read lock;
MariaDB [(none)]> show master status;

+--------------------+----------+--------------+------------------+
| File               | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+--------------------+----------+--------------+------------------+
| mariadb-bin.000001 |     1710 |              |                  |
+--------------------+----------+--------------+------------------+
1 row in set (0.00 sec)
```

Access mysql on DB2:
```
mysql -u root -p
grant replication slave on *.* to master_user2 identified by 'password';
flush privileges;
flush tables with read lock;
MariaDB [(none)]> show master status;

+--------------------+----------+--------------+------------------+
| File               | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+--------------------+----------+--------------+------------------+
| mariadb-bin.000001 |      470 |              |                  |
+--------------------+----------+--------------+------------------+
1 row in set (0.00 sec)
```
Restart mariadb on both servers

### Access mysql on DB1:
```
MariaDB [(none)]> stop slave;
MariaDB [(none)]> CHANGE MASTER TO MASTER_HOST='DB2_private_IP', 
                  MASTER_USER='DB2_master_user', 
                  MASTER_PASSWORD='master_user_password', 
                  MASTER_LOG_FILE='DB2Log_file_name', 
                  MASTER_LOG_POS=DB2Log_file_position;
MariaDB [(none)]> start slave;
```

### Access mysql on DB1:
```
MariaDB [(none)]> stop slave;
MariaDB [(none)]> CHANGE MASTER TO MASTER_HOST='DB1_private_IP', 
                  MASTER_USER='DB1_master_user', 
                  MASTER_PASSWORD='DB1master_user_password', 
                  MASTER_LOG_FILE='DB1Log_file_name', 
                  MASTER_LOG_POS=DB1Log_file_position;
MariaDB [(none)]> start slave;
```

### Now our Mariadb master-master replication is enabled.

### Lets create a user,db and some contents on DB1 to our mysql test client:
```
Creating User 'project' with remote access.
create user 'project'@'%' identified by 'dbuserpass';
grant all on *.* to 'project'@'%';
flush privileges;
```
Creating Database 'company' and table 'employees' with dummy data:
```
MariaDB [(none)]> create database company;

MariaDB [(none)]> use company;

MariaDB [company]> create table employees(ID int(5), Name varchar(50));

MariaDB [company]> insert into employees(ID, Name) values('11', 'Robin');
Query OK, 1 row affected (0.00 sec)

MariaDB [company]> insert into employees(ID, Name) values('15', 'Mike');
Query OK, 1 row affected (0.00 sec)

MariaDB [company]> insert into employees(ID, Name) values('20', 'Jessica');
Query OK, 1 row affected (0.00 sec)

MariaDB [company]> insert into employees(ID, Name) values('21', 'Carl');
Query OK, 1 row affected (0.00 sec)

MariaDB [company]> insert into employees(ID, Name) values('22', 'Robert');
Query OK, 1 row affected (0.00 sec)

MariaDB [company]> insert into employees(ID, Name) values('23', 'Louis');
Query OK, 1 row affected (0.00 sec)

exit
```

## Accessing both DB servers from 'test-server' and listing table 'employees':
Install mysql-client on test server:
```
yum install mysql -y
```
#### Access DB1 using its private IP:
```
mysql -u project -p -h DB1_private_IP -e 'select * from company.employees;'
Enter password:
+------+---------+
| ID   | Name    |
+------+---------+
|   11 | Robin   |
|   15 | Mike    |
|   20 | Jessica |
|   21 | Carl    |
|   22 | Robert  |
|   23 | Louis   |
+------+---------+
```
#### Access DB2 using its private IP:
```
mysql -u project -p -h DB2_private_IP -e 'select * from company.employees;'
Enter password:
+------+---------+
| ID   | Name    |
+------+---------+
|   11 | Robin   |
|   15 | Mike    |
|   20 | Jessica |
|   21 | Carl    |
|   22 | Robert  |
|   23 | Louis   |
+------+---------+
```

## Conclusion


#### ⚙️ Connect with Me

<p align="center">
<a href="mailto:jomyambattil@gmail.com"><img src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white"/></a>
<a href="https://www.linkedin.com/in/jomygeorge11"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/></a> 
<a href="https://www.instagram.com/therealjomy"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white"/></a><br />
