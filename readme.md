# About
A companion for managing projects.

## Features
- create project from predefined templates (local or remote)
- support for tasks
- tmux integration

## Installtion
Clone this repo and execute the `install` script.

```shell
$ git clone https://github.com/ludbek/bro.git
$ cd bro
$ ./install
$ WORKSTATION? ($HOME/projects):     # directory where Bro will store projects
$ BRO_STATION? ($HOME/.bro):         # directory where Bro file will reside
$ source ~/.bashrc
```

## Create project
`bro create [-t template -p path] <project>`

### project
Name of the project being created.

```shell
$ bro create aproject
```

### -t (optional)
Template is a generic project structure which can be used to create multiple projects.
It could be a local directory or a remote git repository.
A template can have a shell script named `.brotasks` or a directory named `.brotasks` which houses other 
shell scripts. Create command executes the `setup` task in `.brotasks` (More on this later).

```shell
# create project from local directory template
$ bro create -t /path/to/local/template/ aproject

# create project from remote git repo
$ bro create -t git@github.com:auser/atemplate-repo.git aproject
```

### -p (optional)
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

## Work on a project
`$ bro <project>`

Takes user to the project directory. It executs the `default` task in `.brotasks` (More on this later).
In above example, to work on the `blog` project all I have to do is hit following command.

`$ bro blog`


## List projects
`$ bro list`

Lists available projects.

## Remove a project
`$ bro remove <project>`

Removes the project reference from bro's project index.
Once the project has been removed `bro` will no longer handle it.
Removing project won't remove the project directory.

## Takeover an existing project
`$ bro takeover <project_path> <project_name>`

If there are projects which `bro` did not create, one can easily hand it to `bro` with this command.

```shell
# suppose there is an awesome project at ~/path/to/awesome-project
# to let 'bro' handle it, execute following command
$ bro takeover ~/path/to/awesome-project awesome-project
```

To takeover projects in remote repo, use `create` command.
`$ bro create -t git@github.com:auser/awesome-project.git awesome-project`

## Tasks
...

### Special tasks
#### Setup
...

#### Default
...

## Tmux
...
