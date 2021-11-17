<h1>Pyenv</h1>

## Installation

Install `pyenv` CLI with [pyenv-installer](https://github.com/pyenv/pyenv-installer):
```
sudo curl https://pyenv.run | bash
```
It is installed by default in `~/.pyenv/bin`


Add the `pyenv` CLI to the `PATH` in `~/.bashrc` or `~/.profile`. For instance under _Ubunu / Devian_:
```bash
# the sed invocation inserts the lines at the start of the file
# after any initial comment lines
sed -Ei -e '/^([^#]|$)/ {a \
export PYENV_ROOT="$HOME/.pyenv"
a \
export PATH="$PYENV_ROOT/bin:$PATH"
a \
' -e ':a' -e '$!{n;ba};}' ~/.profile
echo 'eval "$(pyenv init --path)"' >>~/.profile

echo 'eval "$(pyenv init -)"' >> ~/.bashrc
```

See [documentation](https://github.com/pyenv/pyenv#basic-github-checkout) for other platforms.

## Usage

Install a Python version. <span style="color: #A00000; font-weight: 700">Beware</span>: `pyenv` cannot register and use an 
existing Python interpreter.
```
$ pyenv install 3.6.15
Downloading Python-3.6.15.tar.xz...
-> https://www.python.org/ftp/python/3.6.15/Python-3.6.15.tar.xz
Installing Python-3.6.15...
Installed Python-3.6.15 to /home/flaudarin/.pyenv/versions/3.6.15
```

List available versions. Here system means system versions when `pyenv` is not active.
```
$ pyenv versions
* system (set by /home/flaudarin/.pyenv/version)
  3.6.15
```

It is possible to set the Python version to use from the variable environment `PYENV_VERSION`:
```
$ export PYENV_VERSION="3.6.15"
$ pyenv shell
3.6.15
$ python --version
Python 3.6.15
```

Or directly with the command `pyenv shell`
```
$ pyenv shell 3.6.15
$ python --version
Python 3.6.15
```

Deactivate `pyenv` by setting back the _system version of python_:
```
$ pyenv shell system
$ python
pyenv: python: command not found
```


## Update

```
pyenv update
```