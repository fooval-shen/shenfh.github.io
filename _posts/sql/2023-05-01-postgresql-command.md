---
layout: post
title: postgresql command
author: shefh
catalog:  true
header-img: img/home-mountain.jpg
tags:
    - sql
---


## Connect to a database 


```
psql -d <db-name> -U <username> -W

psql -h <host> -p <port> -d <db-name> -U <username> -W

// example
psql -d demo -U postgres -W

```
The above command includes three flags:
* `-h` - specify the host address of the database.
* `-p` - specify the port of the database to connect to.
* `-d` - specify the name of the database to connect to
* `-U` - specify the name of the user to connect as
* `-W` - forces psql to ask for the user password before connecting to the database


## Common psql commands

### List all databases - `\l`
In many cases, you will work with more than one database. You can list all the available databases with the following command:
```
\l

\l+

eg:

   Name    |  Owner   | Encoding |   Collate   |    Ctype    | ICU Locale | Locale Provider |   Access privileges   
-----------+----------+----------+-------------+-------------+------------+-----------------+-----------------------
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 |            | libc            | 
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 |            | libc            | =c/postgres          +
           |          |          |             |             |            |                 | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 |            | libc            | =c/postgres          +
           |          |          |             |             |            |                 | postgres=CTc/postgres
 test      | test     | UTF8     | en_US.UTF-8 | en_US.UTF-8 |            | libc            | =Tc/test             +
           |          |          |             |             |            |                 | test=CTc/test

```

### Switch to another database - `\c`
You can also switch to another database with the following command:
```
\c <db-name>

// eg
\c test
```


### List database tables - `\dt`

List all database tables as follows:

```
\dt
```


### Describe a table - `\d`
psql also has a command that lets you see the table's structure.

```
\d <table-name>
\d+ <table-name>

// example
\d test
```

### List all schemas - `\dn`
The `\dn` psql command lists all the database schem


### List users and their roles - `\du`
Sometimes, you might need to change the user. Postgres has a command that lists all the users and their roles.

```
\du
```


### Retrieve a specific user - `\du`
You can also retrieve information about a specific user with the following command:

```
\du <username>

//example
/du postgres
```
### Turn on query execution time - `\timing`

```
\timing
select * from product;
```

### Import from SQL file

```

psql -h host -U user -f /data/xx.sql -d database
```

### Quit psql - `\q`
You quit the psql interface with the `\q` command.