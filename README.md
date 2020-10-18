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

### Install HDF5
sudo apt -y install libhdf5-dev

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

## Installing Google Cloud SDK

### Download and install Gcloud
wget https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-313.0.0-linux-x86_64.tar.gz
tar -xvzf google-cloud-sdk-313.0.0-linux-x86_64.tar.gz 
./google-cloud-sdk/install.sh
./google-cloud-sdk/bin/gcloud init
source ~/.bashrc

### Install kubectl
gcloud components install kubectl

### List current GCP projects amd set gcloud to that project
gcloud projects list
gcloud config set project [project_id]

Navigate to the GKE page of your project so the Kubernetes Engine API will switch on. That will take several minutes

### Enable APIs
gcloud services enable cloudresourcemanager.googleapis.com pubsub.googleapis.com

### Set timezone and create a GKE cluster
gcloud  config set compute/zone europe-west3-c
gcloud container clusters create test_cluster --num-nodes 3

## Set up Google Cloud Repository
### Install docker
sudo apt -y install docker.io
sudo usermod -a -G docker ${USER}

### Ensure billing is activated, if you have only one billing account this is done automatically

### Enable Container Registry API with the following [link](https://console.cloud.google.com/apis/library/containerregistry.googleapis.com?_ga=2.244700289.523870026.1602227073-601722703.1601999851)

#### Get project ID and number as follows
PROJECT=$(gcloud config get-value project)
echo $PROJECT && gcloud projects list --filter="$PROJECT" --format="value(PROJECT_NUMBER)"

#### Check if you have role 'roles/containerregistry.ServiceAgent', which you should after Oct 5th 2020
gcloud projects get-iam-policy [PROJECT-ID]  \
--flatten="bindings[].members" \
--format='table(bindings.role)' \
--filter="bindings.members:service-[PROJECT-NUMBER]@containerregistry.iam.gserviceaccount.com"

#### If so, continue or else Google is your friend
gcloud auth configure-docker

### Build a little test image
docker build -t [test-project] .
docker tag [test-project] gcr.io/[PROJECT-ID]/[test-project]:tag1
docker push gcr.io/[PROJECT-ID]/[test-project]:tag1
docker pull gcr.io/[PROJECT-ID]/[test-project]:tag1

#### Head out to your project container registry to see the actual image
gcloud container images delete gcr.io/[PROJECT-ID]/quickstart-image:tag1 --force-delete-tags

## Pentesting software

### Install nmap
sudo apt -y install nmap

### Installing Hashcat
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