```
About
  A generic project management tool.


Installation
  Clone this repo at your home directory with the name '.bro' and add it to 
  system path.

  $ git clone git@github.com:ludbek/bro.git
  $ cd bro
  $ ./install
  $ source ~/.bashrc


Commands
  Create new project
    $ bro create awesome_project brofile_template
    
    Creates a directory named 'awesome_project' at default project directory which
    is at '~/projects'. It also places a '.brofile' inside the 'awesome_project'.
    '.brofile' is simply a shell script which is executed by Bro everytime the project
    starts. 'bro create' executes commands at 'setup' section in a '.brofile'.
   
    brofile_template is optional. If present, it is the name of one the templates at
    '.bro/templates' directory. These templates are used for creating '.brofile'
    for a project. Clone and modify the default template to meet your need. One can
    have different templates for different kinds of projects.

  Kickstart an existing project
    $ bro boot awesome_project
    
    Boots a project. It executes commands at 'boot' section in a '.brofile'.
    Use this command to start working on an existing project and launch
    necessary tools for working on it.

  Work on project which has been booted
    $ bro workon awesome_project
    
    Once the project has been kickstarted, use this command for every new terminal.
    It executes commands at 'workon' section in a '.brofile'.

  Remove existing project
    $ bro remove awesome_project
    
    Use it with caution. Deletes the specified project.
  
  List projects
    $ bro list


Brofile
  .brofile is a shell script which is executed during project creation and boot up.
  It is created from templates at 'template/' directory. One can easily extend
  'default' .brofile template.
  

Configuration
  Specify project directory
    Set the directory where Bro saves the projects.
      $ export WORKSTATION=~/path/to/project/directory/

  Specify Bro directory
    Set where Bro files will reside.
      $ export BRO_STATION=/path/to/bro-directory/
```  
