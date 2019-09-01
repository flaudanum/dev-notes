# Hadoop

<!-- vscode-markdown-toc -->
* 1. [Configuration](#Configuration)
	* 1.1. [Dependency to JAVA](#DependencytoJAVA)
	* 1.2. [Environment variables](#Environmentvariables)
	* 1.3. [Hadoop configutation](#Hadoopconfigutation)
	* 1.4. [Bash setup](#Bashsetup)
	* 1.5. [Formatting the file system](#Formattingthefilesystem)
	* 1.6. [Starting the NameNode daemon DataNode daemon](#StartingtheNameNodedaemonDataNodedaemon)
	* 1.7. [Web interface](#Webinterface)
* 2. [HDFS basic operations](#HDFSbasicoperations)
	* 2.1. [Make a new directory](#Makeanewdirectory)
	* 2.2. [Send data to the HDFS](#SenddatatotheHDFS)
	* 2.3. [Misc commands for HDFS operations](#MisccommandsforHDFSoperations)
* 3. [Run an Mapreduce job](#RunanMapreducejob)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc --># Apache Hadoop



##  1. <a name='Configuration'></a>Configuration

###  1.1. <a name='DependencytoJAVA'></a>Dependency to JAVA

Check java version:
```
$ java --version
openjdk 11.0.3 2019-04-16
OpenJDK Runtime Environment 18.9 (build 11.0.3+7)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.3+7, mixed mode, sharing)
```

Find the location of java implementation

```
$ which java
/usr/bin/java

$ ll /usr/bin/java
lrwxrwxrwx. 1 root root 22 May  5 01:08 /usr/bin/java -> /etc/alternatives/java

$ ll /etc/alternatives/java
lrwxrwxrwx. 1 root root 60 May  5 01:08 /etc/alternatives/java -> /usr/lib/jvm/java-11-openjdk-11.0.3.7-1.fc30.x86_64/bin/java
```

The root of the Java installation is `/usr/lib/jvm/java-11-openjdk-11.0.3.7-1.fc30.x86_64/`

In the Hadoop installation directory edit file `etc/hadoop/hadoop-env.sh` and set the location of the Java installation

```sh
# The java implementation to use. By default, this environment
# variable is REQUIRED on ALL platforms except OS X!
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.3.7-1.fc30.x86_64/
```

###  1.2. <a name='Environmentvariables'></a>Environment variables

| Name   | Description |
|--------|-------------|
| HADOOP_HOME | Path to the root directory of the Hadoop installation |


###  1.3. <a name='Hadoopconfigutation'></a>Hadoop configutation

Documentation is available [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html).

As described in the official documentation, edit configuration files:

- `<HADOOP_HOME>/etc/hadoop/core-site.xml`
- `<HADOOP_HOME>/etc/hadoop/hdfs-site.xml`

### Setup SSH connection to `localhost`

A SSH connection to localhost without passphrase is required. For creating a new SSH key:

```bash
$ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
$ chmod 0600 ~/.ssh/authorized_keys
```

The SSH connection uses port 22 by default. This port might be closed. For opening the port edit file `/etc/ssh/sshd_config` with priviledge then uncomment or add the acces to port 22:

```conf
# If you want to change the port on a SELinux system, you have to tell
# SELinux about this change.
# semanage port -a -t ssh_port_t -p tcp #PORTNUMBER
#
Port 22
```

Eventually restart the SSH service:

```
$ systemctl restart sshd
```

###  1.4. <a name='Bashsetup'></a>Bash setup

Add the following lines:

```bash
# Hadoop
export PATH=$PATH:$APPS/hadoop-3.2.0/bin
export PATH=$PATH:$APPS/hadoop-3.2.0/sbin
export HADOOP_HOME=$APPS/hadoop-3.2.0
```

###  1.5. <a name='Formattingthefilesystem'></a>Formatting the file system

The *Hadoop file system* (HDFS) is formatted with the command `hdfs` in `<HADOOP_HOME>bin`:

```
hdfs namenode -format
```

###  1.6. <a name='StartingtheNameNodedaemonDataNodedaemon'></a>Starting the NameNode daemon DataNode daemon

Use script `start-dfs.sh` from in `<HADOOP_HOME>sbin`:

```
$ start-dfs.sh
Starting namenodes on [localhost]
Starting datanodes
Starting secondary namenodes [new-host]
```

The hadoop daemon log output is written to the $HADOOP_LOG_DIR directory (defaults to $HADOOP_HOME/logs).

###  1.7. <a name='Webinterface'></a>Web interface

Browse the web interface for the *NameNode*; by default it is available at `http://localhost:50070/`. This URL can be configured in `<HADOOP_HOME>/etc/hadoop/hdfs-site.xml` with a `property` element:

```XML
<property>
  <name>dfs.namenode.http-address</name>
  <value>0.0.0.0:50070</value>
</property>
```

##  2. <a name='HDFSbasicoperations'></a>HDFS basic operations

###  2.1. <a name='Makeanewdirectory'></a>Make a new directory

```
$ hdfs dfs -mkdir <path of the directory>
```

The standard HDFS directories required to execute MapReduce jobs are:

```
$ hdfs dfs -mkdir /user
$ hdfs dfs -mkdir /user/flaudanum
$ hdfs dfs -mkdir /user/flaudanum/input
```

###  2.2. <a name='SenddatatotheHDFS'></a>Send data to the HDFS

```
hdfs dfs -put path/to/data /user/flaudanum/input
```

The data `path/to/data` can be either a file or a folder. The data can be removed from the hadoop file system the command `hdfs dfs -rm` for files and `hdfs dfs -rm -r` for removing a folder.

###  2.3. <a name='MisccommandsforHDFSoperations'></a>Misc commands for HDFS operations

The command are provided as options of the HDFS CLI with command `hdfs dfs` or `hadoop fs`. Reference to the commands are available with the `-help` option.

| Command   | Description | Example  |
|-----------|-------------|----------|
| `-ls`     | List path. The minimal path is `/` for there is no concept of current path.  | `hdfs dfs -ls /user` |
| `-cat`    | Concatenate files and print on the standard output | `hadoop fs -cat /user/flaudanum/output/part-r-00000` |

Here are some websites providing some reference material:

* [data-flair.training](https://data-flair.training/blogs/top-hadoop-hdfs-commands-tutorial/)
* [tutorialspoint.com](https://www.tutorialspoint.com/hadoop/hadoop_command_reference.htm)

##  3. <a name='RunanMapreducejob'></a>Run an Mapreduce job

Prepare a jar file file from Hadoop libraries with Maven.

```
hadoop jar MapReduce.jar MaxTemperature /hdfs/path/of/input /hdfs/path/of/output
```
