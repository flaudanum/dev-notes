# Node Package manager notes

- [Node Package manager notes](#node-package-manager-notes)
  - [Start a Node project](#start-a-node-project)
  - [Install / update packages](#install--update-packages)
    - [Global installation or update of a package](#global-installation-or-update-of-a-package)
    - [Local project install](#local-project-install)
    - [Installing types](#installing-types)
    - [Update npm](#update-npm)
    - [Upgrade every package](#upgrade-every-package)
  - [Getting the version of an installed package](#getting-the-version-of-an-installed-package)
  - [Information about version of an available package](#information-about-version-of-an-available-package)
  - [Running scripts in project dependencies](#running-scripts-in-project-dependencies)
  - [Common issues](#common-issues)
    - [Error: Unexpected end of JSON input](#error-unexpected-end-of-json-input)

## Start a Node project

Start a Node.js project in a directory which is not necessarily empty by changing the project path
and type:
```
$ npm init
```

## Install / update packages

### Global installation or update of a package

```bash
npm install -g <package name>@<version specification>
npm install -g @<scope>/<package name>@<version specification>
```

*Examples*:

```bash
npm install -g bootstrap@latest  # latest version
npm install -g bootstrap@3.3.7  # specific version
npm install -g @angular/cli@latest  # the installed package is part of a specified scope
```

### Local project install

```npm
npm install [--save|--save-dev] package_name
```

If the package carries an executable, it can be run with the command `npx`.

### Installing types

Type definitions may be defined for a given package:

```npm
npm install @types/package_name
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

## Running scripts in project dependencies

Scripts installed with global depdendencies (`npm install -g`) are accessible
through the `PATH`. Scripts from project's dependencies are located in the
folders of dependencies in `node_modules/` and symbolic links are available in
`node_modules/.bin`. The most simple way of accessing these scripts is using
`npx`:
```
$ npx <project script>
```

## Common issues

### Error: Unexpected end of JSON input

**Problem**

While installing a package the following error messages may occur:
```
npm ERR! Unexpected end of JSON input while parsing near '..."shasum":"288a04d87ed'
```

**Solution**

Clean the cache of `npm` and reinstall:
```
$ npm cache clean --force
```
