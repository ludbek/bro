# About
A generic project management tool.

# Installation
Clone this repo and execute the 'install' script.

```shell
$ git clone git@github.com:ludbek/bro.git
$ cd bro
$ ./install
$ source ~/.bashrc
```

# Commands
## Create new project
`$ bro create [-t template -p path -r repo] project`

### -t
Name of template file at `templates` directory which will be used for setting up the project.

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

Boots a project. It executes commands at 'boot' section in a '.brofile'.
Use this command to start working on an existing project and launch
necessary tools for working on it.

## Work on project which has been booted
`$ bro workon project`

Once the project has been kickstarted, use this command for every new terminal.
It executes commands at 'workon' section in a `.brofile`.

## Remove existing project
`$ bro remove project`

Use it with caution. Deletes the specified project.

## List projects
`$ bro list`


# Brofile
`.brofile` is a shell script which is executed during project creation and boot up.
It is created from templates at 'template/' directory. One can easily extend
'default' .brofile template.


# Configuration
## Specify project directory
Set the directory where Bro saves the projects.
`$ export WORKSTATION=~/path/to/project/directory/`

## Specify Bro directory
Set where Bro files will reside.
`$ export BRO_STATION=/path/to/bro-directory/`


# TODO
- Autocomplete commands and stuff.
- ~~Create project from a git repo. ~~
- ~~ Specify project path while creating a new one. ~~
