## Bro
A project bootstrapper.

## Possibilities
- start text editor, terminal, browser, spaceship and more with single command
- support for tmux allows one to create complex terminal layouts

## Screencasts
### 1. Creating fresh project
![bro-example](https://user-images.githubusercontent.com/8296449/50532502-2cf3e400-0b6e-11e9-84af-c5f77a04e5cc.gif)

### 2. Creating a project from remote template
![bro](https://user-images.githubusercontent.com/8296449/50532391-d5a14400-0b6c-11e9-9d1f-3f57d271f017.gif)

## Table of content

<!-- vim-markdown-toc GFM -->

* [Requirement](#requirement)
  * [OS](#os)
  * [Tmux](#tmux)
* [Installation](#installation)
  * [1. Download and install bro](#1-download-and-install-bro)
  * [2. Set the directory where bro will reside](#2-set-the-directory-where-bro-will-reside)
  * [3. Set the directory where bro will store the projects](#3-set-the-directory-where-bro-will-store-the-projects)
  * [4. Activate bro](#4-activate-bro)
* [Usage](#usage)
  * [Create project](#create-project)
    * [project](#project)
    * [-t (optional)](#-t-optional)
    * [-p (optional)](#-p-optional)
  * [Work on a project](#work-on-a-project)
  * [Jump to a project directory](#jump-to-a-project-directory)
  * [List projects](#list-projects)
  * [Remove a project](#remove-a-project)
  * [Takeover an existing project](#takeover-an-existing-project)
  * [Exit from a project](#exit-from-a-project)
* [Auto Completion](#auto-completion)
  * [Auto completion for tasks](#auto-completion-for-tasks)
* [Tmux](#tmux-1)
  * [structure](#structure)
  * [window](#window)
  * [run](#run)
  * [vsplit](#vsplit)
  * [hsplit](#hsplit)
  * [pane](#pane)
  * [focus](#focus)
  * [connect](#connect)
  * [An example tmux setup is given below](#an-example-tmux-setup-is-given-below)

<!-- vim-markdown-toc -->

## Requirement
### OS
The script works only in Linux and Mac.

Mac OS needs `gsed`. Install it with `brew install gnu-sed`.
Also add following lines to `.bash_profile` if not already present.

```bash
if [ -f $HOME/.bashrc ]; then
        source $HOME/.bashrc
fi
```

### Tmux
Bro requires tmux 2 and above.

1. Mac
`brew install tmux`

2. Ubuntu
`sudo apt-get install tmux`


## Installation
Run following commands for installation.

### 1. Download and install bro

```shell
$ curl -o  /tmp/bro https://raw.githubusercontent.com/ludbek/bro/master/install && sh /tmp/bro
```
### 2. Set the directory where bro will reside

```shell
BRO_STATION? ($HOME/.bro):
```
### 3. Set the directory where bro will store the projects

```shell
WORKSTATION? ($HOME/projects):
```
### 4. Activate bro

```shell
$ source ~/.bashrc
```

## Usage
Basic project management commands.

### Create project
`bro create [-t template -p path] <project>`

#### project
Name of the project being created.

```shell
$ bro create aproject
```

#### -t (optional)
Template is a generic project structure which can be used to create multiple projects.
It could be a local directory or a remote git repository. If exists, it executes script
at `<template-name>/tasks/setup[.ext]`.

[Click here for a remote project template.](https://github.com/ludbek/bro-example)

```shell
# create project from local directory template
$ bro create -t /path/to/local/template/ aproject

# create project from remote git repo
$ bro create -t git@github.com:auser/atemplate-repo.git aproject
```

#### -p (optional)
Path where the new project will be created. By default it is created at the default directory
as specificed by environment variable `WORKSTATION`.

The path could be in following formats:

1. `-p /absolute/path/to/project`
2. `-p ~/path/to/project`
3. `-p category`
      Path relative to the default project directory.

```shell
# create project in default project directory
$ bro create aproject

# create project at home directory
$ bro create -p ~/ aproject

# categorize projects
# creates project at $WORKSTATION/web/blog
$ bro create -p web blog

# creates project at $WORKSTATION/web/gifhunter
$ bro create -p web gifhunter
```

### Work on a project
`$ bro start <project>` [...params]

Takes user to the project directory. It executes the hook at `<project-dir>/tasks/init[.ext]` if it exists (More on this later).
In then context of above example, to work on the `blog` project, all I have to do is hit following command.

`$ bro start blog`

We can pass parameters to the `<project-dir>/tasks/init[.ext]`. It gives us an ability to differ the way we start a project.
`$ bro start blog test`
`$ bro start blog open-aws-console`

### Jump to a project directory
`$bro cd <project>`

It takes us to the project directory. Useful in situations where one wishes to jump to the project directory without
invoking `init` hook.

### List projects
`$ bro list`

Lists available projects.

### Remove a project
`$ bro remove <project>`

Removes the project reference from bro's project index.
Once the project has been removed `bro` will no longer handle it.
Removing project won't remove the project directory.

### Takeover an existing project
`$ bro takeover <project_path> [project_name]`

If there are projects which `bro` did not create, one can easily hand it to `bro` with this command.

```shell
# suppose there is an awesome project at ~/path/to/awesome-project
# to let 'bro' handle it, execute following command
$ bro takeover ~/path/to/awesome-project awesome-project
```

To takeover projects in remote repo, use `create` command.

`$ bro create -t git@github.com:auser/awesome-project.git awesome-project`

### Exit from a project
One can quit current tmux session with `bro exit` command.


## Auto Completion
All the commands are auto completed by default.

### Auto completion for tasks
You can have some tasks to be executed inside `<project_root>/tasks/init.sh`.
Any function within init.sh is automatically displayed as auto completion option.

For example, you want to run docker containers using docker-compose with the command
`bro start <your_project> start-docker` so that you don't have to manually run the command.
Then the `init.sh` could be:

```shell
# $1 is always the project name
# $2 is what you provide after project name in the bro start command
action=$2

start-docker () {
    docker-compose up
}

# or you want to assemble your android project
assemble () {
    cd android
    ./gradlew assembleDebug
}

$action

```
In this case, `start-docker` and `assemble` will appear as auto completion options while starting project.

## Tmux
`bro` provides essential apis for intereacting with `tmux`.
`bro` requires `tmux` 2 or greater.
These apis are available only inside the task files.

### structure
`$ structure <project>`

Starts new tmux session.

### window
`$ window <name>`

Creates new `tmux` window.

### run
`$ run "<shell command>"`

Sends given shell command to current window or pane.

### vsplit
`$ vsplit`

Vertically splits current window and selects the left pane.

### hsplit
`$ hsplit`

Horizontally splits the current window and selects the top pane.

### pane
`pane <right|down>`

Selects the right pane or the pane below the current pane.

### focus
`focus <window>`

Selects a window with given name.

### connect
`connect <project>`

Attach to a tmux session with given project name.

### An example tmux setup is given below
These apis are available only at `<project-name>/tasks/init` script.
It must be a bash script.

```shell
#!/bin/sh

structure $project
  window editor
    run "nvim"
  window shell
    run "python manage.py shell"
  window terminal
  window builds
    vsplit
      run "python manage.py runserver"
    pane right
      hsplit
        run "webpack --watch"
      pane down
        run "cd semantic"
        run "gulp watch"

focus editor
connect $project
```
