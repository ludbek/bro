```
About
  A generic project management tool. Create, jump and bootstrap projects for 
  any language.

Installation
  Clone this repo at your home directory with the name '.bro' and add it to 
  system path.

  $ git clone git@github.com:ludbek/bro.git ~/.bro
  $ echo "source ~/.bro/activate" >> ~/.bashrc
  $ source ~/.bashrc

Commands
  Create new project
    $ bro create awesome_project
    
    Creates a directory named 'awesome_project' at default project directory which
    is at '~/projects'.
    CDs TO 'awesome_project' and bootstraps it using '.brofile'.

  Work on existing project
    $ bro workon awesome_project
    
    CDs to 'awesome_project' and bootstraps it using '.brofile' in it.

  Remove existing project
    $ bro remove awesome_project
    
    Use it with caution. Deletes the specified project.
  
  List projects
    $ bro list

Bootstraping Project
  Every new project has '.brofile' in it. Which is executed by Bro everytime the 
  project is started with 'bro workon' command. Add necessary bash command to it.
  One could put commands to activate virtual environments, launch text editor and
  others.
   
  Example .brofile contents
    Node
      nvm use v0.10.38

    Python
      source .env/bin/activate

    Or
      # use appropriate environment
      nvm use v0.10.38
      # launch a browser     
      firefox &
      # lauch a text editor 
      gvim &
      # lauch a file manager
      nemo &

Configuration
  Specify project directory
    Set the directory where Bro saves the projects.
      $ export WORKSTATION=~/path/to/project/directory/

  Specify Bro directory
    Set where Bro files will reside.
      $ export BRO_STATION=/path/to/bro-directory/
```  
