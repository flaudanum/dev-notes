- [Virtual environments with `venv`](#Virtual-environments-with-venv)
  - [Create a new virtual environment](#Create-a-new-virtual-environment)
  - [Activate / Deactivate a virtual environment](#Activate--Deactivate-a-virtual-environment)
  - [Remove all installed packages](#Remove-all-installed-packages)
  - [Removing a package and its dependencies](#Removing-a-package-and-its-dependencies)
  - [Install a dependency from a Git repository](#Install-a-dependency-from-a-Git-repository)
- [Virtual environments with `virtualenvwrapper`](#Virtual-environments-with-virtualenvwrapper)
  - [Installation](#Installation)
  - [Usage](#Usage)

# Virtual environments with `venv`

The module `venv` is part of the standard library of Python 3. However on Linux **Ubuntu** it is required to install a dedicated package:

```bash
$ sudo apt install python3.7-venv
```

If the use of `venv` module fails (module `_ctypes` is missing) then install the library `ffi-dev` and other libraries:

```bash
$ sudo apt install libffi-dev
$ sudo apt install libssl-dev libncurses5-dev libsqlite3-dev libreadline-dev libtk8.5 libgdm-dev libdb4o-cil-dev libpcap-dev libreadline-gplv2-dev libncursesw5-dev tk-dev libgdbm-dev libc6-dev libbz2-dev
```

and build Python 3 from sources. Download and decompress sources, change to the directory and start the build process:

```bash
$ sudo tar -xzvf Python-3.7.3.tgz
$ cd Python-3.7.3/
$ sudo ./configure
$ sudo make
$ sudo -H make install
```

## Create a new virtual environment

Change to the working directory where the *virtual environment* will be activated. Next create a new virtual environment inside the directory:

```bash
$ cd my-working-directory
$ python3 -m venv my_virtual_env
```

A new directory called `my_virtual_env` is created.

## Activate / Deactivate a virtual environment

From the working directory where the *virtual environment* is installed, activate the environment with the command (Linux):

```bash
$  source my_virtual_env/bin/activate
(my_virtual_env) $
```

For deactivating the virtual environment:

```bash
(my_virtual_env) $ deactivate
$
```

Scripts `activate.bat` and `deactivate.bat` have been created on Windows:

```cmd
$ my_virtual_env/bin/activate.bat
```

## Remove all installed packages

For removing all packages installed with pip (from [StackOverflow](https://stackoverflow.com/questions/11248073/what-is-the-easiest-way-to-remove-all-packages-installed-by-pip)):
```bash
$ pip freeze | xargs pip uninstall -y
```

An alternative solution in two steps is:
```bash
$ pip freeze > requirements.txt
$ pip uninstall -r requirements.txt -y
```

## Removing a package and its dependencies

When removing a package with `pip` the dependencies of this package that are not used by any other package are not removed. This issue is addressed by the util [pip-autoremove](https://github.com/invl/pip-autoremove) that is available on **PyPi**.

## Install a dependency from a Git repository

Installing from a **remote** repository (e.g. *GitHub*, *GitLab*) with HTTP:
```
$ pip install -e git+https://path/to/the/remote/repository.git@the_selected_branch
```
The option `-e` may be added so that the dependency is editable.

Installing from a **local** repository require the prefix `file://` which indicates the local file system::
```
$ pip install -e git+https://path/to/the/local/repository.git@the_selected_branch
```

# Virtual environments with `virtualenvwrapper`

## Installation

First install `virtualenvwrapper` from *PyPi*

```bash
$ pip install virtualenvwrapper
```

Next get the location of the script `virtualenvwrapper.sh`:

```bash
$ which virtualenvwrapper.sh
/usr/local/bin/virtualenvwrapper.sh
```

Eventually edit file `~/.bashrc` or `~/.profile` and add the following instructions:

```bash
export WORKON_HOME=$HOME/.virtualenvs   # Optional
export PROJECT_HOME=$HOME/projects      # Optional
source /usr/local/bin/virtualenvwrapper.sh
```

## Usage

**Create** a new virtual environment:

```bash
$ mkvirtualenv my-env
(my_env) $
```

**Deactivate** the current virtual environment:

```bash
(my_env) $ deactivate
$
```

**Activate** an existing environment:

```bash
$ workon my-existing-env
(my-existing-env) $
```

**Remove** an existing environment:

```bash
$ rmvirtualenv my-existing-env
```

**List** existing environments:

```bash
$ lsvirtualenv
```
