# Virtual environments with Pipenv

Dependencies are expressed in the file `Pipfile`. This file is expanded as file `Pipfile.lock` which depicts in details the environment and is actually used for the installation of dependencies.

- [Virtual environments with Pipenv](#Virtual-environments-with-Pipenv)
  - [Adding dependency and keeping environment up-to-date](#Adding-dependency-and-keeping-environment-up-to-date)
  - [Installation of dependencies](#Installation-of-dependencies)
  - [Install a dependency from VCS](#Install-a-dependency-from-VCS)

## Adding dependency and keeping environment up-to-date

A new dependency is added with the instruction:

```bash
$ pipenv install package-name
```

If the dependency is only required for development purposes

```bash
$ pipenv install --dev dev-package-name
```

Sometimes it can be useful to update the file `Pipfile.lock`. For instance if the file `Pipfile` was manually modified:

```bash
$ pipenv lock
```

For removing a dependency:

```bash
$ pipenv uninstall package-name
```

For removing every dependency:

```bash
$ pipenv uninstall --all
```

Display the graph of dependencies:

```bash
$ pipenv graph
```

## Installation of dependencies

Install a **production** environment from file `Pipfile.lock`:

```bash
pipenv install
```

Install a **development** environment from file `Pipfile.lock`:

```bash
pipenv install --dev
```

## Install a dependency from VCS

Suppose one wants to install a package from a *GitLab* server where:
* URL domain is *gitlab.my-domain.com*
* user is *flaudanum*
* project name is *my_project*
* the selected branch of the Git remote repository is `main_dev`
* the package name (as stated in file `setup.py`) is *my-project*
then the installation can be made with the following command:

```bash
$ pipenv install -e git+http://gitlab.my-domain.com/flaudanum/my_project.git@main_dev#egg=my-project
```
