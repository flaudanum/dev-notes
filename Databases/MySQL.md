# MySQL relational database management system

## Install and initial setup

Installation guidelines for MySQL *Community Edition* for *Linux Fedora*.

### Server and client

Relevant resources can be found for *Fedora* at [if-not-true-then-false.com](https://www.if-not-true-then-false.com/2010/install-mysql-on-fedora-centos-red-hat-rhel/).

Start the service:
```
# systemctl status mysqld.service
```
Check the status of the service:
```
# systemctl status mysqld.service
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2020-05-08 13:31:27 CEST; 24min ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 8445 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 8523 (mysqld)
   Status: "Server is operational"
    Tasks: 39 (limit: 19069)
   Memory: 457.9M
      CPU: 11.383s
   CGroup: /system.slice/mysqld.service
           └─8523 /usr/sbin/mysqld

May 08 13:31:14 localhost.localdomain systemd[1]: Starting MySQL Server...
May 08 13:31:27 localhost.localdomain systemd[1]: Started MySQL Server.
```

### Workbench

Install:

```
# dnf install mysql-workbench-community.x86_64
```

Start the workbench:

```
$ mysql-workbench
```

## MySQL client CLI

Connect as *root* user on localhost (the default).

```
mysql -h localhost -u root -p
```

Regarding the password option `--password,-p` the manual says:

`--password[=password], -p[password]`

The password of the MySQL account used for connecting to the server. The password value is optional. If not given, mysql prompts for one. If given, there must be no space between `--password=` or `-p` and the password
following it. If no password option is specified, the default is to send no password.

Specifying a password on the command line should be considered insecure. To avoid giving the password on the command line, use an option file. See Section 6.1.2.1, “End-User Guidelines for Password Security”.

To explicitly specify that there is no password and that mysql should not prompt for one, use the `--skip-password` option.

## MySQL operations

### Create a new schema

```SQL
CREATE SCHEMA `node-complete` ;
```

### Create a table

Example:

```SQL
CREATE TABLE IF NOT EXISTS tasks (
    task_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    start_date DATE,
    due_date DATE,
    status TINYINT NOT NULL,
    priority TINYINT NOT NULL,
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)  ENGINE=INNODB;
```


## MySQL third-party programming libraries

| | | |
|---|---|---|
| JavaScript | Node.js | `npm install --save mysql2` |
