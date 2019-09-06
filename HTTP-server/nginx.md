# NGINX

- [NGINX](#nginx)
  - [Installation](#installation)
  - [Lower SELinux restrictions](#lower-selinux-restrictions)
  - [Open the firewall for port 80](#open-the-firewall-for-port-80)
  - [Tutorial - Configuration](#tutorial---configuration)

## Installation

On **Linux Fedora**, use `dnf install`:
```
$ sudo dnf install nginx
```

Next enable NGINX service permanently:
```
$ sudo systemctl enable nginx.service
```

Start NGINX service:
```
$ sudo systemctl start nginx.service
```

Check the service is running:
```
$ systemctl list-units | grep 'nginx'
nginx.service     loaded active running   The nginx HTTP and reverse proxy server 
```

## Lower SELinux restrictions

SELinux is enabled by default on modern RHEL and CentOS servers. Each operating system object (process, file descriptor, file, etc.) is labeled with an **SELinux context** that defines the permissions and operations the object can perform. On Fedora, NGINX is labeled with the **httpd_t context**.  Context httpd_t must be added to the list of permissive domains (object type):
```
$ sudo semanage permissive -a httpd_t
```
Option `-a` adds a record of the permissive object type. This permission may be removed with `semanage permissive -d`.

Reference: [nginx.com/blog](https://www.nginx.com/blog/using-nginx-plus-with-selinux/).

**To be confirmed** directive `user` may be set in `nginx.conf` for specifying a user associated the *worker processes*.

## Open the firewall for port 80

If a server listens to the default port 80, it may be locally accessed with the URL `http://localhost/` (equivalent to `http://localhost:80/`). First check that the server is actually listening to port 80:
```
# netstat -tulpn | grep nginx
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      1100/nginx: master  
tcp6       0      0 :::80                   :::*                    LISTEN      1100/nginx: master  
```

Now if any external connection to the server is timed out when connecting on port 80 then it is very likely that it is rejected by the firewall. As a consequence a connection with a port higher than 1024 (*well known* ports) should work.

Making the firewall **firewallD** allow external connections with port 80 requires to add the service **http** to authorized services in the appropriate *zone*. Network interfaces are assigned a zone to dictate the behavior that the firewall should allow:
```
# firewall-cmd --get-active-zones
FedoraWorkstation
  interfaces: enp0s3
```
Here the only active zone is `FedoraWorkstation` with a network interface `enp0s3`. Next add the service **http**:
```
# firewall-cmd --zone=FedoraWorkstation --add-service=http
success
```
Now check that external connection to default port 80 is not blocked by the firewall anymore. If so, add the rule permanently:
```
# firewall-cmd --permanent --zone=FedoraWorkstation --add-service=http
success
```
Service `http` has been added permanently:
```
# firewall-cmd --zone=FedoraWorkstation --list-services --permanent
dhcpv6-client http mdns samba-client ssh
```
For more information see the [tutorial blog on digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7).


## Tutorial - Configuration

Edit the configuration file `/etc/nginx/nginx.conf`. When making modifications for the first time make a backup copy first:
```
$ cp -p /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup-original
```

See the [Beginners' guide on nginx.com](https://nginx.org/en/docs/beginners_guide.html).

Suppose we want to serve a static website which local path on the server is `/data/www/m-web-site` and index page is `index.html`. Suppose the server has no recorded domain in the DNS so that it is accessed by the IP address `192.168.0.223` (*e.g.* get it with `ifconfig`), see the [documentation about server names](https://nginx.org/en/docs/http/server_names.html) for more information.. The server listens to the port 4200.

```conf
    server {
        # Server is listening to port 4200
        listen *:4200;
        
        # The server is accessed with its IP address
        server_name 192.168.0.223;
        
        location / {
            # Local path of the web site
            root /data/www/m-web-site;
        }
        
        # Index web page
        index index.html;
    }
```

The following snippet may be introduced in the directive `http` for using the connection to a virtual server listening to the default port 80 as a *proxy* for connecting two other virtual servers. All the virtual servers are hosted by the local machine which IP address is provided by `$host`.
```conf
    # Server #0
    server {
        # Listening to port 80 as default port
        listen 80 default_server;
        listen [::]:80 default_server;
        server_name $host;
        # Local path /data/www/ contains a file `index.html`
        root /data/www/;

        # Proxy to Server #1
        location /url-to-server-1/ {
            return http://$host:5000;
        }

        # Proxy to Server #2
        location /url-to-server-2/ {
            return http://$host:6911;
        }

    }

    # Virtual server #1
    server {
        # Listening to port 5000
        listen 5000;
        listen [::]:5000;
        server_name $host;
        root /data/www/site_1/;
    }

    # Virtual server #2
    server {
        # Listening to port 6911
        listen 6911;
        listen [::]:6911;
        server_name $host;
        root /data/www/site_2/;
    }
```