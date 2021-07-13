<style>
body {
  width: 50%;
  min-width: 750px;
  margin: auto;
}
h1, h2, h3 {
  color: #40b149;
  background-color: #FAFAFA;
}
</style>
<h1>Anaconda</h1>

<h2>Table of Content</h2>

- [Anaconda CLI - conda](#anaconda-cli---conda)
  - [Environments](#environments)
    - [Creating a virtual environment](#creating-a-virtual-environment)
    - [Removing an environment](#removing-an-environment)
    - [Listing environments](#listing-environments)
    - [Activate an environment](#activate-an-environment)
    - [Installing / removing dependencies](#installing--removing-dependencies)
    - [Updating dependencies](#updating-dependencies)
    - [Updating conda](#updating-conda)
    - [Listing dependencies](#listing-dependencies)
    - [Export / Import an environment](#export--import-an-environment)
    - [File environment.yml with git](#file-environmentyml-with-git)
  - [Python interpreter](#python-interpreter)
    - [Available version of Python](#available-version-of-python)
    - [Creating an environment with specific python version](#creating-an-environment-with-specific-python-version)

# Anaconda CLI - conda

## Environments

### Creating a virtual environment

Creating a new blank environment:

```
conda create --name <envname>
```

Creating a new environment with dependencies specified in a `requirements.txt`:

```
conda create --name <envname> --file requirements.txt
```

### Removing an environment

```
conda env remove -n <name of the env to remove>
```

### Listing environments

Getting information about the active environment:

```
conda info
```

Getting the list of all environments:

```
conda info --envs
```

### Activate an environment

Listing environments:

```
(base)> conda info --envs
# conda environments:
#
base                  *  C:\Users\frederic.Laudanum\Anaconda3
my-project               C:\Users\frederic.Laudanum\Anaconda3\envs\my-project
demo-api                 C:\Users\frederic.Laudanum\Anaconda3\envs\demo-api
```

`base` is the default global environment. For activating the environment `my-project`:

```
(base)> conda activate my-project
(my-project)>
```

### Installing / removing dependencies

The dependencies to install can be specified as CLI arguments:

```
conda install <pacakge0>[=,>=,<=,>,<]<version> ...
```

or from a text file:

```
conda install --file requirements.txt
```

Uninstall dependencies:

```
conda remove <pacakge0>[=,>=,<=,>,<]<version> ...
```

### Updating dependencies

Update dependencies:

```
conda install --update-deps <dependency-0> <dependency-1> ...
```

Update all packages:

```
conda install --update-all, --all
```

Updating specific packages:

```
conda update <dependency-0> <dependency-1> ...
```

### Updating conda

```
conda update -n base -c defaults conda
```

### Listing dependencies

This command lists all the dependencies installed in the active _conda environment_ or by default the globally installed dependencies.

```
conda list
```

For listing information about specific dependencies:

```
conda list <dependency-0> <dependency-1> ...
```

This command is similar to `pip list`. For listing dependencies for a `requirements.txt` in the same way as `pip freeze`:

```
conda list --export
```

### Export / Import an environment

Export your active environment to a file `environment.yml` (standard name):

```
conda env export > environment.yml
```

**Note** : This file handles both the environment's pip packages and conda packages.

Then for importation:

```
conda env create -f environment.yml
```

### File environment.yml with git

Example of environment file `environment.yml` with a GitHub dependency:

```yaml
name: example-environment

channels:
  - conda-forge
  - defaults

dependencies:
  - python=3.4
  - numpy
  - toolz
  - matplotlib
  - dill
  - pandas
  - partd
  - bokeh
  - pip:
    - git+https://github.com/user/project.git@branch_name#egg=package_name[extra_name]
```

## Python interpreter

### Available version of Python

```
conda search -f python
```

### Creating an environment with specific python version

```
conda create -n "myenv" python=3.3.0 ipython
```

The conda environments are prepended to the PATH variable, so when trying to run the executable "ipython", the OS will not find "ipython" in the our activated environment (since it doesn't exist there), but it will continue searching for it, and eventually find it wherever you have it installed.
