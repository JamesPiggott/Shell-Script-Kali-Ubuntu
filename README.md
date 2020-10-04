# Ubuntu Installation Guide

Use the following script to set up Ubuntu in one go. It will install Chrome, Git, more editors, set up programming environments and prepare your machine for the cloud.

## Update the system
sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade

## Install Git
sudo apt-get -y install git && sudo apt-get -y install git-lfs

## Install Chrome browser
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && sudo dpkg -i google-chrome-stable_current_amd64.deb

## Set Python environment to version 3
sudo apt -y install python3-pip
sudo pip3 install virtualenvwrapper

### Set the Python3 environment variables
sudo echo "export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3" >> ~/.bashrc
sudo echo "export WORKON_HOME=$HOME/.virtualenvs" >> ~/.bashrc
sudo echo "export PROJECT_HOME=$HOME" >> ~/.bashrc
sudo echo "source /usr/local/bin/virtualenvwrapper.sh" >> ~/.bashrc

### Create an alias for Python3.8 (the latest version of Python installed), perform these commands in the terminal
alias python='/usr/bin/python3.8'
. ~/.bashrc
python --version

## Install Terminator
sudo add-apt-repository -y ppa:gnome-terminator
sudo apt-get -y update
sudo apt-get -y install terminator

## Install Visual Studio Code
sudo apt -y install software-properties-common apt-transport-https wget
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
sudo add-apt-repository -y "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
sudo apt -y update
sudo apt -y install code

## Install Java
sudo apt -y install default-jre
sudo apt -y install default-jdk

## Install IntelliJ
sudo snap install intellij-idea-community --classic
