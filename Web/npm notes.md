# Node Package manager notes

- [Node Package manager notes](#Node-Package-manager-notes)
  - [Install / update packages](#Install--update-packages)
    - [Update npm](#Update-npm)
    - [Upgrade every package](#Upgrade-every-package)
  - [Getting the version of an installed package](#Getting-the-version-of-an-installed-package)
  - [Information about version of an available package](#Information-about-version-of-an-available-package)

## Install / update packages

Global installation or update of a package

```bash
> npm install -g <package name>@<version specification>
> npm install -g @<scope>/<package name>@<version specification>
```

*Examples*:

```bash
> npm install -g bootstrap@latest  # latest version
> npm install -g bootstrap@3.3.7  # specific version
> npm install -g @angular/cli@latest  # the installed package is part of a specified scope
```

### Update npm

Once installed, **npm** can be updated like any package:

```bash
> npm install -g npm@latest
```

### Upgrade every package

To update to a new major version all the packages, install the `npm-check-updates` package globally:

```bash
npm install -g npm-check-updates
```

then run it:

```bash
ncu -u
```

this will upgrade all the version hints in the *package.json* file, to dependencies and devDependencies, so `npm` can install the new major version.

You are now ready to run the update:

```bash
npm update
```

If you just downloaded the project without the node_modules dependencies and you want to install the shiny new versions first, just run:

```bash
npm install
```


## Getting the version of an installed package

Use command `npm ls` or `npm list` for getting the version of an installed package. With option `-g` the version of the *global* install is provided and without this is the local version

```bash
> npm ls <package name>
> npm ls -g <package name>
```

The depth of the dependency tree can be limited with the option `--depth`. The following command list the local packages without depth.

```bash
> npm list --depth 0
```

## Information about version of an available package

For getting information about the available versions of a package, use commands `npm show` or `npm view`:

```bash
> npm view <package name>
> npm show <package name>
```