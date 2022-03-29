# MariaDB Master-Master/Slave Replication


[![Build](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

## Description

Replication in SQL databases is the process of copying data from the source database to another one (or multiple ones) and vice versa. Data from one database server are constantly copied to one or more servers. You can use replication to distribute and balance requests  across a pool of replicated servers, provide failover and high availability of MariaDB databases. The  MariaDB (and MySQL) allows to use two types database replication mades: Master-Master and Master-Slave.

Configuring Simple Master-Master Replication on MariaDB:
In a Master-Master replication scheme, any of the MariaDB/MySQL database servers may be used both to write or read data. Replication is based on a special binlog file, a Master server saves all operations with the database to. A Slave server connects to the Master and applies the commands to its databases.

## Pre-Requests

