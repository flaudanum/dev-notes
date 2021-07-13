<style>
.main-title {
    text-align: center;
    font-size: 3em;
    font-weight: 500;
    margin-bottom: 50px
}
</style>
<div class="main-title">SQL Server / Transact-SQL</div>

- [SQL Server administration](#sql-server-administration)
  - [Open TCP/IP](#open-tcpip)
  - [Start-Stop-Restart the server](#start-stop-restart-the-server)
- [SQLCMD CLI](#sqlcmd-cli)
  - [Connection](#connection)
- [Transact SQL](#transact-sql)
  - [GO statement](#go-statement)
  - [Listing of databases](#listing-of-databases)
  - [Listing of schemas in current database](#listing-of-schemas-in-current-database)
  - [Set active database with `USE`](#set-active-database-with-use)
  - [Create database and schema](#create-database-and-schema)


# SQL Server administration

## Open TCP/IP

Open the **SQL Server Configuration Manager**.

Expand in the left browser panel the element _SQL Server Network Configuration_. Right click the element _TCP/IP_ in the right panel and enable the connection.

![SQL Server Network Configuration menu](./SQL-Server-T-SQL-pics/Enable%20TCP%20IP%20connections.png)

In the same place (right click on _TCP/IP_ and _properties_) you can get network information such as the IP address and the connection port.

Next enable the connection to IP addresses:

![Enable the connection to IP addresses](SQL-Server-T-SQL-pics/Enable%20connection%20to%20IP.png)

## Start-Stop-Restart the server

Open the **SQL Server Configuration Manager**.

Expand the SQL Server service.

![Expand SQL Server service and click on properties](./SQL-Server-T-SQL-pics/start-stop-restart%201-2.png)

![Properties window](./SQL-Server-T-SQL-pics/start-stop-restart%202-2.png)


# SQLCMD CLI

`sqlcmd` is the CLI for _SQL Server_:

- It provides an interactive environment for running _T-SQL_ queries.
- `sqlcmd` can run _T-SQL_ scripts

## Connection

Connecting to the default DB instance on machine `LYN-L129`:

```
PS > SQLCMD -S LYN-L129
1> EXIT
PS >
```

Connecting to the database `MY-DB` with the login `my_login` and password `Secret69`:

```
sqlcmd -S MY-DB -U my_login -P Secret69
```

<strong style="color:#8A0808;">BEWARE:</strong> the **SQL server authentication** must be first enabled in the _server_. This can be achieved with Microsoft SSMS.

In the _server properties_ go to _Security_ and check the option **SQL Server and Windows Authentication mode**. Then restart the server with _SQL Server Configuration manager_.

# Transact SQL

## GO statement

_T-SQL_ command are stored but not executed until a `GO` statement is entered.

```
2> select DB_NAME()
3> GO

--------------------------------------------------------------------------------------------------------------------------------
master

(1 rows affected)
```

## Listing of databases

```SQL
SELECT name, database_id, create_date  
FROM sys.databases ;  
GO 
```

## Listing of schemas in current database

```SQL
SELECT * FROM sys.schemas
GO
```


## Set active database with `USE`

Sets database `AdventureWorks2012` as **current database** then creates schema `Sprockets`:

```
USE AdventureWorks2012;  
GO  
CREATE SCHEMA Sprockets;
GO
```


## Create database and schema

```
1> CREATE DATABASE rge_app_poc
2> go
1> use rge_db_poc;
2> go
Msg 911, Level 16, State 1, Server LYN-L129, Line 1
Database 'rge_db_poc' does not exist. Make sure that the name is entered correctly.
1> use rge_app_poc;
2> go
Changed database context to 'rge_app_poc'.
1> CREATE SCHEMA test_schema;
2> GO
1> SELECT * FROM rge_app_poc.schemas;
2> GO
Msg 208, Level 16, State 1, Server LYN-L129, Line 1
Invalid object name 'rge_app_poc.schemas'.
1> SELECT * FROM sys.schemas;
2> GO
name                                                                                                                             schema_id   principal_id
-------------------------------------------------------------------------------------------------------------------------------- ----------- ------------
dbo                                                                                                                                        1            1
guest                                                                                                                                      2            2
INFORMATION_SCHEMA                                                                                                                         3            3
sys                                                                                                                                        4            4
test_schema                                                                                                                                5            1
db_owner                                                                                                                               16384        16384
db_accessadmin                                                                                                                         16385        16385
db_securityadmin                                                                                                                       16386        16386
db_ddladmin                                                                                                                            16387        16387
db_backupoperator                                                                                                                      16389        16389
db_datareader                                                                                                                          16390        16390
db_datawriter                                                                                                                          16391        16391
db_denydatareader                                                                                                                      16392        16392
db_denydatawriter                                                                                                                      16393        16393

(14 rows affected)
```


