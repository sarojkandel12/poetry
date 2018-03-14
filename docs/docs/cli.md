# Commands

You've already learned how to use the command-line interface to do some things.
This chapter documents all the available commands.

To get help from the command-line, simply call `poetry` or `poetry list` to see the complete list of commands,
then `--help` combined with any of those can give you more information.

As `Poetry` uses [cleo](https://github.com/sdispater/cleo) you can call commands by short name if it's not ambiguous.

```bash
poetry up
```

calls `poetry update`.


## Global options

* `--verbose (-v|vv|vvv)`: Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug.
* `--help (-h)` : Display help information.
* `--quiet (-q)` : Do not output any message.
* `--ansi`: Force ANSI output.
* `--no-ansi`: Disable ANSI output.
* `--version (-V)`: Display this application version.


## new

This command will help you kickstart your new Python project by creating
a directory structure suitable for most projects.

```bash
poetry new my-package
```

will create a folder as follows:

```text
my-package
├── pyproject.toml
├── README.rst
├── my_package
│   └── __init__.py
└── tests
    ├── __init__.py
    └── test_my_package
```

If you want to name your project differently than the folder, you can pass
the `--name` option:

```bash
poetry new my-folder --name my-package
```

## install

The `install` command reads the `pyproject.toml` file from the current directory, resolves the dependencies,
and installs them.

```bash
poetry install
```

If there is a `pyproject.lock` file in the current directory,
it will use the exact versions from there instead of resolving them.
This ensures that everyone using the library will get the same versions of the dependencies.

If there is no `pyproject.lock` file, Poetry will create one after dependency resolution.

You can specify to the command that you do not want the development dependencies installed by passing
the `--no-dev` option.

```bash
poetry install --no-dev
```

You can also specify the extras you want installed
by passing the `--E|--extras` option (See [Extras](#extras) for more info)

```bash
poetry install --extras "mysql pgsql"
poetry install -E mysql -E pgsql
```

### Options

* `--no-dev`: Do not install dev dependencies.
* `--extras (-E)`: Features to install (multiple values allowed).

## update

In order to get the latest versions of the dependencies and to update the `pyproject.lock` file,
you should use the `update` command.

```bash
poetry update
```

This will resolve all dependencies of the project and write the exact versions into `pyproject.lock`.

If you just want to update a few packages and not all, you can list them as such:

```bash
poetry update requests toml
```

### Options

* `--dry-run` : Outputs the operations but will not execute anything (implicitly enables --verbose).

## add

The `add` command adds required packages to your `pyproject.toml` and installs them.

If you do not specify a version constraint,
poetry will choose a suitable one based on the available package versions.

```bash
poetry add requests pendulum
```

### Options

* `--dev (-D)`: Add package as development dependency.
* `--optional` : Add as an optional dependency.
* `--dry-run` : Outputs the operations but will not execute anything (implicitly enables --verbose).


## remove

The `remove` command removes a package from the current
list of installed packages

```bash
poetry remove pendulum
```

### Options

* `--dev (-D)`: Removes a package from the development dependencies.
* `--dry-run` : Outputs the operations but will not execute anything (implicitly enables --verbose).


## show

To list all of the available packages, you can use the `show` command.

```bash
poetry show
```

If you want to see the details of a certain package, you can pass the package name.

```bash
poetry show pendulum

name        : pendulum
version     : 1.4.2
description : Python datetimes made easy

dependencies:
 - python-dateutil >=2.6.1
 - tzlocal >=1.4
 - pytzdata >=2017.2.2
```

### Options

* `--tree`: List the dependencies as a tree.
* `--latest (-l)`: Show the latest version.
* `--outdated (-o)`: Show the latest version but only for packages that are outdated.


## build

The `build` command builds the source and wheels archives.

```bash
poetry build
```

Note that, at the moment, only pure python wheels are supported.

### Options

* `--format (-F)`: Limit the format to either wheel or sdist.

## publish

This command builds (if not already built) and publishes the package to the remote repository.

It will automatically register the package before uploading if this is the first time it is submitted.

```bash
poetry publish
```

### Options

* `--repository (-r)`: The repository to register the package to (default: `pypi`).
Should match a repository name set by the [`config`](#config) command.


## config

The `config` command allows you to edit poetry config settings and repositories.

```bash
poetry config --list
```

### Usage

````bash
poetry config [options] [setting-key] [setting-value1] ... [setting-valueN]
````

`setting-key` is a configuration option name and `setting-value1` is a configuration value.

### Modifying repositories

In addition to modifying the config section,
the config command also supports making changes to the repositories section by using it the following way:

```bash
poetry config repositories.foo https://foo.bar/simple/
```

This will set the url for repository `foo` to `https://foo.bar/simple/`.

If you want to store your credentials for a specific repository, you can do so easily:

```bash
poetry config http-basic.foo username password
```

If you do not specify the password you will be prompted to write it.

### Options

* `--unset`: Remove the configuration element named by `setting-key`.
* `--list`: Show the list of current config variables.

## search

This command searches for packages on a remote index.

```bash
poetry search requests pendulum
```

### Options

* `--only-name (-N)`: Search only in name.

## lock

This command locks (without installing) the dependencies specified in `pyproject.toml`.

```bash
poetry lock
```