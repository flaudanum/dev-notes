# Secure SHell

## Basic setup

Main resources on NixCraft:

* [How To Set up SSH Keys on a Linux / Unix System](https://www.cyberciti.biz/faq/how-to-set-up-ssh-keys-on-linux-unix/)
* [OpenSSH Config File Examples](https://www.cyberciti.biz/faq/create-ssh-config-file-on-linux-unix/)
* [ssh.com](https://www.ssh.com/ssh/)

The default port for SSH is port 22. It must be open on the local and *remote machines*. Edit the default SSH configuration file (on both machines) `/etc/ssh/ssh_config` and add the line:

```conf
Port 22
```

Suppose the *remote machine* is named `server2` with IP address `192.168.1.11`. First generate a *SSH key* for a connection to that *remote machine*. The commands below generate a *RSA* key and copy the public key file to the account `flaudanum` on the remote server/host:

```
$ ssh-keygen -t rsa -b 4096 -f ~/.ssh/server2.key -C "Key to server2 - home network"
$ ssh-copy-id -i .ssh/server2.key.pub flaudanum@192.168.1.11
```

The options used with the command `ssh-keygen` gives the following properties to the key:

* `-t rsa` : Specifies the type of key to create. The possible values are `rsa1` for protocol version 1 and `dsa`, `ecdsa`, `ed25519` or `rsa` for protocol version 2.
* `-b 4096` : Specifies the number of bits in the key to create (default is 2048)
* `~/.ssh/server2.key` : Specifies the filename of the key file.
* `-C "Key to server2 - home network"` : Set a new comment.

The command `ssh-copy-id` copies the public key to the file `/home/flaudanum/.ssh/authorized_keys` on the *remote machine*.

Next the file `~/.ssh/config` must be edited. You must create it if used for the first time:

```
$ touch ~/.ssh/config
$ chmod 600 ~/.ssh/config
```

The following lines must be added to `~/.ssh/config`:

```conf
Host server2
     HostName 192.168.1.11
     User flaudanum
     Port 22
     IdentityFile /home/flaudanum/.ssh/server2.key
```

## Run commands on a remote machine

```
ssh user@server command arg1 arg2 ...
```
When arguments include special characters like `!` then use simple quotes around the command: `'command arg1 arg2 ...'`.

Running a command as a **sudoer** on the remote machine usually require to enter the user's password within a *pseudo-terminal*. `ssh user@server sudo command` will fail with the error message `sudo: no tty present and no askpass program specified`. In order to force the use of a pseudo terminal option `-t` is required:
```
ssh -t user@server command arg1 arg2 ...
```


## Secure copy with `scp`

The SSh protocol can copy files from a machine to a remote one. Options `-p` (preserve metadata) and `-r` (recursive copy) from `cp` are still valid.
```
$ scp examplefile user@server:/remote/location/path
```
`user@server` must have permissions on path `server:/remote/location/path`. If not the copy must be carried out in **two steps**:

First copy to a location where `user@server` has permissions:
```
$ scp examplefile user@server:/remote/location/path/with/permission
```
Next move the copied file to the final location with `ssh`:
```
$ ssh -t user@server sudo mv /remote/location/path/with/permission/examplefile /final/location/path
```
