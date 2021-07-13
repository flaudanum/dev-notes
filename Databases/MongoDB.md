<h1>MongoDB</h1>

- [Installation - Fedora](#installation---fedora)
- [Configure SELinux](#configure-selinux)
- [Start MongoDB](#start-mongodb)
- [Local databse URI](#local-databse-uri)
- [Basic operations with MongdDB Shell](#basic-operations-with-mongddb-shell)

# Installation - Fedora

Download the `.rpm` packages (server, client, shell, utilities) then run:

```
# dnf install mongodb-org-server-*.x86_64.rpm mongodb-org-mongos-*.x86_64.rpm mongodb-org-shell-*.x86_64.rpm mongodb-org-tools-*.x86_64.rpm


```

# Configure SELinux

If _SELinux_ is in enforcing mode, you must customize your _SELinux policy_ for MongoDB. This process comes from the [MongoDB official documentation](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/).

First check that the utility `checkpolicy` is installed

```
# dnf install checkpolicy
```

Create a custom policy file mongodb_cgroup_memory.te:

```
cat > mongodb_cgroup_memory.te <<EOF
module mongodb_cgroup_memory 1.0;

require {
    type cgroup_t;
    type mongod_t;
    class dir search;
    class file { getattr open read };
}

#============= mongod_t ==============
allow mongod_t cgroup_t:dir search;
allow mongod_t cgroup_t:file { getattr open read };
EOF
```

Once created, compile and load the custom policy module by running these three commands:

```
checkmodule -M -m -o mongodb_cgroup_memory.mod mongodb_cgroup_memory.te
semodule_package -o mongodb_cgroup_memory.pp -m mongodb_cgroup_memory.mod
sudo semodule -i mongodb_cgroup_memory.pp
```

The MongoDB process is now able to access the correct files with SELinux set to enforcing.

_Remark_: The current SELinux Policy does not allow the MongoDB process to open and read /proc/net/netstat for Diagnostic Parameters (FTDC). As such, the audit log may include numerous messages regarding lack of access to this path. <span style="color:#007000">This should be fixed soon in the latest stable version.</span>

# Start MongoDB

Starting with `systemctl`:

```
sudo systemctl start mongod
```

```
$ systemctl status mongod
● mongod.service - MongoDB Database Server
     Loaded: loaded (/usr/lib/systemd/system/mongod.service; enabled; vendor preset: disabled)
     Active: active (running) since Sun 2020-07-12 17:51:32 CEST; 13s ago
       Docs: https://docs.mongodb.org/manual
    Process: 68509 ExecStartPre=/usr/bin/mkdir -p /var/run/mongodb (code=exited, status=0/SUCCESS)
    Process: 68510 ExecStartPre=/usr/bin/chown mongod:mongod /var/run/mongodb (code=exited, status=0/SUCCESS)
    Process: 68511 ExecStartPre=/usr/bin/chmod 0755 /var/run/mongodb (code=exited, status=0/SUCCESS)
    Process: 68512 ExecStart=/usr/bin/mongod $OPTIONS (code=exited, status=0/SUCCESS)
   Main PID: 68515 (mongod)
     Memory: 72.0M
        CPU: 523ms
     CGroup: /system.slice/mongod.service
             └─68515 /usr/bin/mongod -f /etc/mongod.conf

Jul 12 17:51:31 localhost.localdomain systemd[1]: Starting MongoDB Database Server...
Jul 12 17:51:31 localhost.localdomain mongod[68512]: about to fork child process, waiting until server is ready for connections.
Jul 12 17:51:31 localhost.localdomain mongod[68514]: forked process: 68515
Jul 12 17:51:32 localhost.localdomain mongod[68512]: child process started successfully, parent exiting
Jul 12 17:51:32 localhost.localdomain systemd[1]: Started MongoDB Database Server.
```

The output above shows that the service is enabled but you can ensure that MongoDB will start following a system reboot by issuing the following command:

```
sudo systemctl enable mongod
```

# Local databse URI

The default local database URI is `mongodb://localhost:27017`. More details about URI formats in the [official documentation](https://docs.mongodb.com/manual/reference/connection-string/).

# Basic operations with MongdDB Shell

Connect with the MongdDB Shell CLI:

```
$ mongo
MongoDB shell version v4.2.8
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("4a6e1a2e-a2ad-4b38-a894-55ca539b034b") }
MongoDB server version: 4.2.8
...
>
```

Create a new database `new-db` and check that it is the active database:

```
> use new-db
switched to db new-db
> db
new-db
```

When list existing databases, the newly created database does not exist. The reason is that an empty database is not listed

```
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
```

Now add a new collection `notes` with a document:

```
> db.notes.insert({"title": "My MongoDB notes"})
WriteResult({ "nInserted" : 1 })
> show dbs
admin        0.000GB
config       0.000GB
new-db       0.000GB
local        0.000GB
> show collections
notes
```

