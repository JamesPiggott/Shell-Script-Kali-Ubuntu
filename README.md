# Ubuntu Installation Guide

Use the following script to set up Ubuntu in one go. It will install Chrome, Git, more editors, set up programming environments and prepare your machine for the cloud.

## Update the system
```
sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade
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
```wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && sudo dpkg -i google-chrome-stable_current_amd64.deb```

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

### Create an alias for Python3.8 (the latest version of Python installed), perform these commands in the terminal
```
alias python='/usr/bin/python3.8'
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

# Install Maltego
```
wget https://maltego-downloads.s3.us-east-2.amazonaws.com/linux/Maltego.v4.2.16.13775.deb
sudo dpkg -i Maltego.v4.2.16.13775.deb
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
wget https://golang.org/dl/go1.16.2.linux-amd64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.16.2.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
source $HOME/.profile
```


```

## Pentesting software

### Install nmap
```
sudo apt -y install nmap
```

### Installing Hashcat
```
sudo apt -y install libssl-dev libcurl4-openssl-dev libpcap0.8-dev zlib1g-dev

sudo git clone https://github.com/ZerBea/hcxdumptool.git
sudo git clone https://github.com/ZerBea/hcxtools.git
sudo git clone https://github.com/hashcat/hashcat.git

cd hcxdumptool
sudo make
sudo make install
cd ..
cd hcxtools/
sudo make
sudo make install
cd ..
cd hashcat
sudo make
sudo make install
cd ..
```