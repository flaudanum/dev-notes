# Apache HADOOP troubleshooting

## WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!

<p style="background-color: rgb(220,220,220)">Solution:</p>

Remove SSH keys for localhost and create a new one.

## Connection refused

```
$ hdfs dfs -ls
ls: Call From new-host-4.home/192.168.1.6 to localhost:9000 failed on connection exception: java.net.ConnectException: Connection refused; For more details see:  http://wiki.apache.org/hadoop/ConnectionRefused
```

<p style="background-color: rgb(220,220,220)">Solution:</p>

Format the namenode:

```
$ hdfs namenode -format
```

