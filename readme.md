# About
A generic project management tool. It takes you to project directory, sets environment, launches tools(IDE, browser, filemanger) and more.

# Requirements
Linux or OSX.

# Installation
Clone this repo and execute the 'install' script.

```shell
$ git clone git@github.com:ludbek/bro.git
$ cd bro
$ ./install
$ Project directory? ($HOME/projects):     # directory where Bro will store projects
$ Bro directory? ($HOME/.bro):             # directory where Bro file will reside
$ source ~/.bashrc
```

# Commands
## Create new project
`$ bro create [-t template -p path -r repo] project`

The options are optional.

### -t
Name of template file at `$BRO_STATION/templates/` directory which will be used for setting up the project. Checkout the templates at `templates` directory for deeper insight.

### -p
Path where the new project will be created. By default it is created at the default directory
as specificed by environment variable `WORKSTATION`.

The path could be in following formats:

1. `-p /absolute/path/to/project`
2. `-p ~/path/to/project`
3. `-p category`
    Path relative to the default project directory.

### -r
URL of the remote git repo which will be used as the new project's blueprint.

## Kickstart an existing project
`$ bro boot project`

Boots a project. It executes commands at `boot` section in the `.brofile`.
Use this command to launch tools necessary for the project. e.g. emulators, text editor, browser, etc.

## Work on project which has been booted
`$ bro workon project`

Once the project has been booted, use this command for every new terminal. Useful for setting environment variables. It executes commands at `workon` section in the `.brofile` at a project directory.

## Remove existing project
`$ bro remove project`

~~Use it with caution.~~Unlinks the project. Bro won't remove the project directory. One has to manually remove it.

## List projects
`$ bro list`


# Brofile
`.brofile` is a shell script which is executed during project creation and boot up.
It is created from templates at `templates/` directory. One can easily extend
`default` brofile template.


# Configuration
## Specify project directory
Set the directory where Bro saves the projects.
`$ export WORKSTATION=~/path/to/projects/directory/`

## Specify Bro directory
Set where Bro files will reside.
`$ export BRO_STATION=/path/to/bro-directory/`


# TODO
- Autocomplete commands and stuff.
- ~~Create project from a git repo.~~
- ~~Specify project path while creating a new one.~~
