<h1>APT - Advanced Packaging Tool</h1>

# APT commands

| Command        | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `list`         | list packages based on package names                         |
| `search`       | search in package descriptions                               |
| `show`         | show package details                                         |
| `install`      | install packages                                             |
| `remove`       | remove packages                                              |
| `autoremove`   | Remove automatically all unused packages                     |
| `update`       | update list of available packages                            |
| `upgrade`      | upgrade the system by installing/upgrading packages          |
| `full-upgrade` | upgrade the system by removing/installing/upgrading packages |
| `edit-sources` | edit the source information file                             |

Other commands

| Command     |    paramters     | Description                                                                                                             |
| ----------- | :--------------: | ----------------------------------------------------------------------------------------------------------------------- |
| `purge`     | list of packages | Removes a package with associated configuration files                                                                   |
| `depends`   |   package name   | List the package's dependencies                                                                                         |
| `autoclean` |        —         | Suppresses APT cache for deprecated packages (does not exist with **Trusty**, use ''**apt-get** `autoclean`'' instead). |
| `clean`     |        —         | Suppresses all APT cache (does not exist with **Trusty**, use ''**apt-get** `clean`'' instead).                         |
