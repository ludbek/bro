# About
A generic project manager and task executer.
It creates, lists and removes projects.
It also executes tasks inside a project.

# Updates
- v2.0.0 allows tasks. The task execution syntax has change. Its now `bro [project] task [params]` instead of `bro task project`

# Requirements
Linux or OSX.

# Installation
Clone this repo and execute the `install` script.

```shell
$ git clone https://github.com/ludbek/bro.git
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

## Remove existing project
`$ bro remove project`

~~Use it with caution.~~Unlinks the project. Bro won't remove the project directory. One has to manually remove it.

## List projects
`$ bro list`


# Task Execution
`bro [project] <task> [params]`

Task is simply a switch case in `.brofile`.

- project(optional) : Name of the project. The project name is not required if one is at the project's root directory.
- task : A task inside the project.
- params(optional) : Parameters passed to the project.


## Default Tasks
### boot
`$ bro project boot`

Boots a project. It executes task at `boot` section in the `.brofile`.
Use this command to launch tools necessary for the project. e.g. emulators, text editor, browser, etc.

### workon
`$ bro project workon`

Execute this task for every new terminal. Useful for setting environment variables. It executes commands at `workon` section in the `.brofile` at a project directory.

# Brofile
`.brofile` is a shell script which is executed during project creation and task execution.
It is created from templates at `templates/` directory. One can easily extend
`default` brofile template.


# Configuration
The values are set during installation.
## Specify project directory
Set the directory where Bro saves the projects.
`$ export WORKSTATION=~/path/to/projects/directory/`

## Specify Bro directory
Set where Bro files will reside.
`$ export BRO_STATION=/path/to/bro-directory/`


# TODO
- Autocomplete commands and stuff.
- ~~Support custom commands(tasks).~~
- ~~Create project from a git repo.~~
- ~~Specify project path while creating a new one.~~
