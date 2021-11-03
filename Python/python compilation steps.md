```
sudo su -
mkdir /opt/apps
cd /opt/apps
curl -O https://www.python.org/ftp/python/3.9.5/Python-3.9.5.tar.xz
tar -xf Python-3.9.5.tar.xz
cd Python-3.9.5
apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev
```

Le package `build-essential` installe le comilateur C `gcc`.

Libraires optionnelles à intégrer:

```
apt-get install libbz2-dev liblzma-dev libsqlite3-dev
```

Setup de la compilation avec création d'un Makefile:

```
./configure
```

Lancer la compilation avec l'option `altinstall` du Makefile qui aura pour effet
de ne pas créer de conflit avec une installation de Python 3 en tant que package de la distribution.

```
make altinstall
exit
```

Mise à jour de PyPI et des outils de packaging:

```
sudo python3.9 -m pip install -U pip setuptools
```
