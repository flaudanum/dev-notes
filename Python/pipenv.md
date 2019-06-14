# Virtual environments with Pipenv

Dependencies are expressed in the file `Pipfile`. This file is expanded as file `Pipfile.lock` which depicts in details the environment and is actually used for the installation of dependencies.

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
