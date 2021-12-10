# WordPress Devcontainer 
Develop WordPress plugins or themes inside a devcontainer, with prebuilt database, and mailhog. With devcontainer, your team can share the same dev environment, and it doesn't mess up with your local environment. 

The devcontainer installs the latest gutenberg, theme-check, and query-monitor plugins to help with development. 

## Usage 
1. Install Docker and VSCode, install [Remote Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) and install `devcontainer` CLI from VSCode
2. clone this repo and name it as you will: `git clone https://github.com/JJPro/devcontainer-wordpress.git [<your project name>]`
3. modify .devcontainer/setup.sh as instructed in the file to specify the type of project (plugin or theme or mixed)
4. execute `devcontainer open` from the command line or execute from VSCode **Remote-Containers: Open Folder in Container...**
5. now a wordpress instance is up and running at http://localhost:8000

## Features 
- WordPress intellisense / auto-completion, for functions and hooks 
- wp-cli preinstalled 
- default shell: zsh 

## Forwarded Ports 
- 8000: WordPress
- 3306: Mariadb Database 
- 8025, 1025: MailHog 

## Passwords 
- WordPress: (defined in *Dockerfile*)
  - user: wordpress
  - password: password 
- DB: (defined in *docker-compose.yml*)
  - database: wordpress
  - user: wordpress
  - pass: wordpress

## default plugins
The following plugins are activated to ease with development: 
- gutenberg
- theme-check
- query-monitor

The list can be updated in *devcontainer.json* file in property `postCreateCommand`

## Other Information
- aliases: (defined in *.zshrc*)
  - cdp: cd to project directory 
  - cdw: cd to wordpress root directory 
- PHP version: 7.4 (can be changed in *docker-compose.yml*)
