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


## Kill tmux session
`$ bro exit`

Kills current tmux session.

## Tasks
Bro supports task execution. The tasks are housed in a `.brotasks` file or `.brotasks` directory.
If `.brotasks` is a directory, it can have multiple shell scripts.
Each script can have its own set of switch statements. Look at the `templates` directory of this repo for basic `.brotasks` script.
The syntax for task execution is as follows:

`$ bro [project] <task> [params]`

- project(optional) : Name of the project. The project name is not required if one is at the project's root directory.
- task : A task inside the project.
- params(optional) : Parameters passed to the project.

A simple `.brotasks` is given below:

```shell
#!/bin/sh

# $1, is the name of the project in which this file resides
# $2, is the task to be executed

case $2 in
	greet)
		echo "Hello world !!!"
	;;
esac
```

The tasks in above file can be executed in following ways.

```shell
# execute tasks from outside a project
$ bro aproject greet
=> Hello world !!!

# execte tasks from inside a project
$ bro aproject
$ bro greet
=> Hello world !!!
```

### Special tasks
There are two tasks which are special to `bro`. These tasks are optional and are executed only if available.

#### setup
This task is executed while creating a project with `create` command and must reside in a project template.
An example for setting up a Django and Mithril project is as follows:

```shell
#!/bin/sh

# $1, is the name of the project in which this file resides
# $2, is the task to be executed

case $2 in
	setup)
		# setup django
		virtualenv --python=python3 .env
		source .env/bin/activate
		pip install django
		django-admin startproject $1 .

		# setup mithril
		nvim use 5.6.0
		npm install --save mithril
		npm install --save-dev webpack

		echo "Project setup successful"
		;;
	greet)
		echo "Hello world !!!"
		;;
esac
```

#### default
This task is executed everytime a user requests `bro` to workon a project.
`bro aproject` executes this task.
An example task which activates python virtual environment and chooses node version is as follows.

```shell
#!/bin/sh

# $1, is the name of the project in which this file resides
# $2, is the task to be executed

case $2 in
	setup)
		# setup django
		virtualenv --python=python3 .env
		source .env/bin/activate
		pip install django
		django-admin startproject $1 .

		# setup mithril
		nvim use 5.6.0
		npm install --save mithril
		npm install --save-dev webpack

		echo "Project setup successful"
		;;
	default)
		source .env/bin/activate
		nvm use 5.6.0

		echo "Happy hacking"
		;;
	greet)
		echo "Hello world !!!"
		;;
esac
```


## Tmux
`bro` provides essential apis for intereacting with `tmux`.
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

```shell
#!/bin/sh

# $1, is the name of the project in which this file resides
# $2, is the task to be executed

case $2 in
	tmux)
		project=$1

		structure $project
			window editor
				run "bro env"
				run "nvim"
			window shell
				run "bro env"
				run "python manage.py shell"
			window terminal
				run "bro env"
			window builds
				vsplit
					run "bro env"
					run "python manage.py runserver"
				pane right
					hsplit
						run "bro env"
						run "webpack --watch"
					pane down
						run "bro env"
						run "cd semantic"
						run "gulp watch"

		focus editor
		connect $project
		;;
	env)
		source .env/bin/activate
		nvm use v5.6.0	
		;;
esac
```
