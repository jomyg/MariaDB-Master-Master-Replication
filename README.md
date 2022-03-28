[root@db1 ~]# vi /etc/my.cnf.d/server.cnf
[root@db1 ~]# mysql -u root
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 2
Server version: 5.5.68-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> create user 'test_master'@'%' identified by 'test_master';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> GRANT REPLICATION SLAVE ON *.* TO test_master IDENTIFIED BY 'test_master';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> FLUSH TABLES WITH READ LOCK;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> SHOW MASTER STATUS;
+--------------------+----------+--------------+------------------+
| File               | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+--------------------+----------+--------------+------------------+
| mariadb-bin.000001 |      584 |              |                  |
+--------------------+----------+--------------+------------------+
1 row in set (0.00 sec)

MariaDB [(none)]> UNLOCK TABLES;
Query OK, 0 rows affected (0.00 sec)
[root@db1 ~]# vi /etc/my.cnf.d/server.cnf
[root@db1 ~]# mysql -u root
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 2
Server version: 5.5.68-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> create user 'test_master'@'%' identified by 'test_master';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> GRANT REPLICATION SLAVE ON *.* TO test_master IDENTIFIED BY 'test_master';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> FLUSH TABLES WITH READ LOCK;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> SHOW MASTER STATUS;
+--------------------+----------+--------------+------------------+
| File               | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+--------------------+----------+--------------+------------------+
| mariadb-bin.000001 |      584 |              |                  |
+--------------------+----------+--------------+------------------+
1 row in set (0.00 sec)

MariaDB [(none)]> UNLOCK TABLES;
Query OK, 0 rows affected (0.00 sec)





[root@ip-172-31-32-183 ~]# mysql -u root
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 2
Server version: 5.5.68-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> CHANGE MASTER TO
    -> MASTER_HOST='172.31.46.194',
    -> MASTER_USER='test_master',
    -> MASTER_PASSWORD='test_master',
    -> MASTER_PORT=3306,
    -> MASTER_LOG_FILE='mariadb-bin.000001',
    -> MASTER_LOG_POS=584,
    -> MASTER_CONNECT_RETRY=10;
Query OK, 0 rows affected (0.01 sec)

MariaDB [(none)]> START SLAVE;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
4 rows in set (0.00 sec)

MariaDB [(none)]> exit
Bye


