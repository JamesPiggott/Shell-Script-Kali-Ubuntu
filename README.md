# Ubuntu Installation Guide

Use the following script to set up Ubuntu in one go. It will install Chrome, Git, more editors, set up programming environments and prepare your machine for the cloud.

## Update the system
```
sudo apt-get update && sudo apt-get -y upgrade && sudo apt-get dist-upgrade && sudo apt autoremove
```

## Install Git
```
sudo apt-get -y install git && sudo apt-get -y install git-lfs
```

### Generate key and add public key to GitHub
``` 
ssh-keygen
eval `ssh-agent`
ssh-add ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub
```

## Install Chrome browser
```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && sudo dpkg -i google-chrome-stable_current_amd64.deb
```

## Set Python environment to version 3
```
sudo apt -y install python3-pip
sudo pip3 install virtualenvwrapper
```

### Install HDF5
```
sudo apt -y install libhdf5-dev
```

### Set the Python3 environment variables
```
sudo echo "export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3" >> ~/.bashrc
sudo echo "export WORKON_HOME=$HOME/.virtualenvs" >> ~/.bashrc
sudo echo "export PROJECT_HOME=$HOME" >> ~/.bashrc
sudo echo "source /usr/local/bin/virtualenvwrapper.sh" >> ~/.bashrc
```

### Create an alias for Python3.10 (the latest version of Python installed), perform these commands in the terminal
```
alias python='/usr/bin/python3.10'
. ~/.bashrc
python --version
```

## Install Terminator
```
sudo add-apt-repository -y ppa:gnome-terminator
sudo apt-get -y update
sudo apt-get -y install terminator
```

## Install Visual Studio Code
```
sudo apt -y install software-properties-common apt-transport-https wget
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
sudo add-apt-repository -y "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
sudo apt -y update
sudo apt -y install code
```

## Install Java
```
sudo apt -y install default-jre
sudo apt -y install default-jdk
```

## Install IntelliJ
```
sudo snap install intellij-idea-community --classic
```

## Install PyCharm
```
sudo snap install pycharm-community --classic
```

## Install Go Programming Language
```
wget https://go.dev/dl/go1.18.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.18.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
source $HOME/.profile
go version
```

## Install Postgres database
```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get -y install postgresql
```

## Install docker
```
sudo apt -y install docker.io
sudo usermod -a -G docker ${USER}
```
