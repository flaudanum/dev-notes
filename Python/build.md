<h1>Build Python from source</h1>

# Linux OS

The documentation provides the following steps:

```
./configure
make
make install
```

## Integrate SSL

SSL is mandatory for downloading dependencies with PyPI repository. See [this page](https://techglimpse.com/install-python-openssl-support-tutorial/) for the detailed process.

The main point is to uncomment and set the location of OpenSSL in the `Modules/Setup`:

```
# Socket module helper for SSL support; you must comment out the other
# socket line above, and possibly edit the SSL variable:
SSL=/usr/bin/openssl
#_ssl _ssl.c \
#	-DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \
#	-L$(SSL)/lib -lssl -lcrypto
```

## Optinal modules

When running the first `make` command, some optional libraries may be missing. This can be solved by installing some libraries. E.g. (Fedora/Red Hat):

```
sudo dnf install openssl-devel.x86_64
sudo dnf install python3-tkinter.x86_64
sudo dnf install bzip2-devel.x86_64
sudo dnf install readline-devel.x86_64
sudo dnf install tk-devel.x86_64
sudo dnf install sqlite-devel
```
