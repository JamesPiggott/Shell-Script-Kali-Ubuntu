# Installing Google Cloud SDK

Procedure for setting up Google Cloud SDK (Gcloud), Kubectl, Helm etc

### Download and install Gcloud
```
wget https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-313.0.0-linux-x86_64.tar.gz
tar -xvzf google-cloud-sdk-313.0.0-linux-x86_64.tar.gz 
./google-cloud-sdk/install.sh
./google-cloud-sdk/bin/gcloud init
source ~/.bashrc
```

### Install kubectl
```
gcloud components install kubectl
```

### List current GCP projects amd set gcloud to that project
```
gcloud projects list
gcloud config set project [project_id]
```

Navigate to the GKE page of your project so the Kubernetes Engine API will switch on. That will take several minutes

### Enable APIs
```
gcloud services enable cloudresourcemanager.googleapis.com pubsub.googleapis.com
```

### Set timezone and create a GKE cluster
```
gcloud config set compute/zone europe-west3-c

gcloud container clusters create application-cluster --num-nodes 3 --enable-network-policy --zone=europe-west3-c --release-channel=rapid --cluster-version=1.20.5-gke.1300
```

### List zone of the GKE cluster
```
gcloud container clusters list
```

### Create a GCE persistent disk
```
gcloud compute disks create --size=10GiB --zone=europe-west3-c postgres-db
```

## Set up Google Cloud Repository

### Install docker
```
sudo apt -y install docker.io
sudo usermod -a -G docker ${USER}
```

### Ensure billing is activated, if you have only one billing account this is done automatically

### Enable Container Registry API with the following [link](https://console.cloud.google.com/apis/library/containerregistry.googleapis.com?_ga=2.244700289.523870026.1602227073-601722703.1601999851)
```
gcloud services enable containerregistry.googleapis.com
```

#### Get project ID and number as follows
```
PROJECT=$(gcloud config get-value project)
echo $PROJECT && gcloud projects list --filter="$PROJECT" --format="value(PROJECT_NUMBER)"
```

#### Check if you have role 'roles/containerregistry.ServiceAgent', which you should after Oct 5th 2020
```
gcloud projects get-iam-policy [PROJECT-ID]  \
--flatten="bindings[].members" \
--format='table(bindings.role)' \
--filter="bindings.members:service-[PROJECT-NUMBER]@containerregistry.iam.gserviceaccount.com"
```

#### If so, continue or else Google is your friend
```
gcloud auth configure-docker
```

### Build a little test image
```
docker build -t [test-project] .
docker tag [test-project] gcr.io/[PROJECT-ID]/[test-project]:tag1
docker push gcr.io/[PROJECT-ID]/[test-project]:tag1
docker pull gcr.io/[PROJECT-ID]/[test-project]:tag1
```

#### Head out to your project container registry to see the actual image
```
gcloud container images list

gcloud container images delete gcr.io/[PROJECT-ID]/quickstart-image:tag1 --force-delete-tags
```

## Install Helm
```
sudo snap install helm --classic
```

## Install NGINX with Helm
```
helm repo add stable https://charts.helm.sh/stable
kubectl create ns nginx
helm install nginx stable/nginx-ingress --namespace nginx --set rbac.create=true --set controller.publishService.enabled=true
kubectl get svc -n nginx
```

Now wait for the External-IP to become available for the NGINX controller. 

## Install cert-manager and use Lets Encrypt
```
kubectl apply --validate=false -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.12/deploy/manifests/00-crds.yaml
kubectl create namespace cert-manager
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager --namespace cert-manager --version v0.12.0 jetstack/cert-manager
```

Start certificate issuer
```
kubectl apply -f issuer.yaml
```

Finally, start NGINX ingress

```
kubectl create -f ingress-customerportal.yaml
```

Check if a certificate has been issued

```
kubectl get certificaterequests.cert-manager.io
```

## Install Kasten K10
```
helm repo add kasten https://charts.kasten.io/
kubectl create namespace kasten-io
helm install k10 kasten/k10 --namespace=kasten-io
kubectl get pods --namespace kasten-io
kubectl --namespace kasten-io port-forward service/gateway 8080:8000
http://127.0.0.1:8080/k10/#/.
```

