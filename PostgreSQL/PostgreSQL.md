# PostgreSQL

- [PostgreSQL](#postgresql)
  - [Fedora install](#fedora-install)
  - [Initialisation de la base](#initialisation-de-la-base)
  - [Server administration](#server-administration)
    - [Admin user *postgres*](#admin-user-postgres)
    - [Start/stop server](#startstop-server)
    - [Server configuration](#server-configuration)
    - [Set PostgreSQL password for role *postgres*](#set-postgresql-password-for-role-postgres)
  - [PostgreSQL CLI](#postgresql-cli)
    - [SQL commands](#sql-commands)
      - [`CREATE DATABASE`](#create-database)
    - [Meta-commands](#meta-commands)
  - [Data Administration](#data-administration)
    - [Create roles](#create-roles)
    - [Administration CLIs](#administration-clis)

## Fedora install

For installing the *PostgreSQL server*:

```bash
sudo dnf install postgresql-server postgresql-contrib
```
## Initialisation de la base

The file system of the database configuration files must be created first:

```
$ postgresql-setup --initdb --unit postgresql
 * Initializing database in '/var/lib/pgsql/data'
 * Initialized, logs are in /var/lib/pgsql/initdb_postgresql.log
```

Here, the location of the file system of the database is at `/var/lib/pgsql/data`.

```bash
$ sudo ls -l /var/lib/pgsql/data
drwx------. 5 postgres postgres  4096 May  9 10:01 base
drwx------. 2 postgres postgres  4096 May  9 10:01 global
drwx------. 2 postgres postgres  4096 May  9 10:01 pg_commit_ts
drwx------. 2 postgres postgres  4096 May  9 10:01 pg_dynshmem
-rw-------. 1 postgres postgres  4269 May  9 10:01 pg_hba.conf
-rw-------. 1 postgres postgres  1636 May  9 10:01 pg_ident.conf
drwx------. 2 postgres postgres  4096 May  9 10:01 pg_log
drwx------. 4 postgres postgres  4096 May  9 10:01 pg_logical
drwx------. 4 postgres postgres  4096 May  9 10:01 pg_multixact
drwx------. 2 postgres postgres  4096 May  9 10:01 pg_notify
drwx------. 2 postgres postgres  4096 May  9 10:01 pg_replslot
drwx------. 2 postgres postgres  4096 May  9 10:01 pg_serial
drwx------. 2 postgres postgres  4096 May  9 10:01 pg_snapshots
drwx------. 2 postgres postgres  4096 May  9 10:01 pg_stat
drwx------. 2 postgres postgres  4096 May  9 10:01 pg_stat_tmp
drwx------. 2 postgres postgres  4096 May  9 10:01 pg_subtrans
drwx------. 2 postgres postgres  4096 May  9 10:01 pg_tblspc
drwx------. 2 postgres postgres  4096 May  9 10:01 pg_twophase
-rw-------. 1 postgres postgres     3 May  9 10:01 PG_VERSION
drwx------. 3 postgres postgres  4096 May  9 10:01 pg_wal
drwx------. 2 postgres postgres  4096 May  9 10:01 pg_xact
-rw-------. 1 postgres postgres    88 May  9 10:01 postgresql.auto.conf
-rw-------. 1 postgres postgres 22849 May  9 10:01 postgresql.conf
```

## Server administration

[Official v10.x documentation](https://www.postgresql.org/docs/10/server-start.html)

### Admin user *postgres*
The installation of the *PostgreSQL server* created a Linux user *postgres* for administration task of the databases. You must switch to that user for starting the server.
However, this user has initially no password:
```
$ sudo passwd postgres
Changing password for user postgres.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
```

### Start/stop server

The server can only be started and stopped with the user *postgres*.

```
$ su postgres
$ pg_ctl start -D /var/lib/pgsql/data
```

or

```
$ sudo runuser -l postgres -c 'pg_ctl start -D /var/lib/pgsql/data'
waiting for server to start....2019-05-09 10:54:16.524 CEST [16027] LOG:  listening on IPv6 address "::1", port 5432
2019-05-09 10:54:16.524 CEST [16027] LOG:  listening on IPv4 address "127.0.0.1", port 5432
2019-05-09 10:54:16.528 CEST [16027] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2019-05-09 10:54:16.532 CEST [16027] LOG:  listening on Unix socket "/tmp/.s.PGSQL.5432"
2019-05-09 10:54:16.549 CEST [16027] LOG:  redirecting log output to logging collector process
2019-05-09 10:54:16.549 CEST [16027] HINT:  Future log output will appear in directory "log".
 done
server started
```

Instead of specifying the file system location with the option `-D` the environment variable `PGDATA` can be used. Edit (or create) the file `~/.bashrc` for the user *postgres*:

```bash
cd $HOME
export PGDATA=/var/lib/pgsql/data
```

### Server configuration

The server access configuration is in the file `data/pg_hba.conf`. The initial state of the access setting is:

```conf
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     peer
# IPv4 local connections:
host    all             all             127.0.0.1/32            ident
# IPv6 local connections:
host    all             all             ::1/128                 ident
```

The `ident` authentication method works by taking the OS username you’re operating as and comparing it with the allowed database username(s). There is optional username mapping.

This means that in order to connect to PostgreSQL you must be logged in as the correct OS user. For applicative access to the database with the *postgres* role, being logged to Linux as *postgres* user is not suitable. It consequently more desirable to use a specific authentication password for connection to database. File `data/pg_hba.conf` should then be edited as follows:

```conf
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     md5
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
# IPv6 local connections:
host    all             all             ::1/128                 md5
```

### Set PostgreSQL password for role *postgres*

Edit the configuration file `data/pg_hba.conf` and modify the identification method for role *postgres* or (*all* roles) to `trust`:

```conf
# Database administrative login by Unix domain socket
local   all             postgres                                trust
```

Then restart the server as Linux user *postgres*.
```bash
$ su postgres
$ pg_ctl restart
```

Connect to *postgres* database as user *postgres* and use the meta-command `\password`:
```
$ psql
psql (10.7)
Type "help" for help.

postgres=# \password postgres
Enter new password:
Enter it again:
postgres=# \q
```

Again edit file `data/pg_hba.conf` and enable encrypted authentification:

```conf
# Database administrative login by Unix domain socket
local   all             postgres                                md5
```
Eventually restart the server.

## PostgreSQL CLI

The *PostgreSQL CLI* is accessed with command `psql`.

For connecting to a databse `<databse>` as a PostgreSQL user `<user>`:

```
psql --username <user> --dbname <database>
```

*E.g.* for connecting to database *postgres*:

```
su postgres
Password:
bash-4.4$ psql --username postgres --dbname postgres
psql (10.7)
Type "help" for help.

postgres=#
```

### SQL commands


#### `CREATE DATABASE`
Create a new databse. This command requires priviledge and is granted to user *postgres* by default.

```SQL
CREATE DATABASE data_base_name;
```

### Meta-commands

| Command | Description |
|---------|-------------|
| `\?`  | List of `psql` meta-commands |
| `\c` or `\connect` | Connect to new database |
| `\d`  | List tables, views, and sequences |
| `\du` | List the attributes of existing roles |
| `\l`  | List existing databases |
| `\q`  | Quit the CLI |

## Data Administration

### Create roles

A role which can connect to a database and create databases:
```SQL
CREATE ROLE admin_role LOGIN INHERIT CREATEDB PASSWORD 'AdminRolePassword';
```
This *admin role* can connect to the *postgres* data base in order to create new tables.

A group is a role without inheritance that is granted to other roles:

```SQL
CREATE ROLE new_group NOINHERIT NOLOGIN;
```

### Administration CLIs

For creating **a new database** (same as SQL `CREATE DATABASE`):
```
$ createdb –U <user name> <database name>
```

For **deleting a database** (same as SQL `DROP DATABASE`):
```
$ dropdb –i –U <user name> <database name>
```

For creating a **new role**:
```
$ createuser <options> <user name>
```
The attributes of the role are specified through the options. A role can connect to a database if it is granted the attribute *login* or *superuser*.

For **deleting a role**:
```
$ dropuser <options> <user name>
```


