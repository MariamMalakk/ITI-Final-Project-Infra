# ITI-Final-Project-Infra
### Objectives:
![MicrosoftTeams-image (1)](https://user-images.githubusercontent.com/118521640/221272282-fefc348f-0c6c-4aed-97ac-1bb7ac60ea1f.png)

This project is to build and deploy a web app using jenkins on a private cluster via private vm for more security. Infrastructure is created by terraform.
#### It consists of:
- VPC
- two private subnets (management-subnet & restricted-subnet)
- private VM to connect to cluster
- priviae cluster to deploy our app
- NAT Gateway 
___________________________________________________________________________________________________________________________
### Prerequisites:
  - linux
  - Terraform 
  - GCP
  - Docker
  - VScode
 ___________________________________________________________________________________________________________________________
 ### Login to your project on GCP:
 ```bash 
 gcloud auth login
 ```
____________________________________________________________________________________________________________________________
### Build GCP infrastructure:
- By using terraform files 
- Apply this commands on VScode
```bash
  cd terraform
  terraform init
  terraform plan
  terraform apply
```
_____________________________________________________________________________________________________________________________
### Connect to GCP private Cluster:
#### Note: kubectl is already installed as startup script in terraform, check instance.tf file.
- SSH to private VM 
- Then connect to the cluster from instance-1
```bash
gcloud container clusters get-credentials priv-gke-cluster --zone asia-east1-b --project <project-id>
```
![3-connect to cluster](https://user-images.githubusercontent.com/118521640/221274053-22cf7774-f41e-4570-9a62-fae5ed9c1edd.png)

____________________________________________________________________________________________________________________________
### Deploy jenkins on GKE cluster :
#### Note: you can use jenkins deployment files, check jenkins-files directory
```bash
kubectl create ns devops-tools
kubectl apply -f <file-name.yml> -n devops-tools
```
#### Get service ip to access jenkins on browser:
```bash
kubectl get svc -n devops-tools
```
![1-creating jenkins pod](https://user-images.githubusercontent.com/118521640/221276938-b86bf8e1-1569-4c43-937c-7e0d41c60b7a.png)
![2-login to jenkins](https://user-images.githubusercontent.com/118521640/221279555-82439687-7d5c-41ef-9cff-fc64321871b3.png)
#### You can get password from logs:
```bash
kubectl logs <pod-name> -n devops-tools
```
_________________________________________________________________________________________________________________________________
### Deploy jenkins-slave:
#### Note: you can use jenkins slave deplyment files, check jenkins-files/slave directory
![6- slave deployment](https://user-images.githubusercontent.com/118521640/221280737-1c02b8db-07f4-4b02-a5b6-d4a955363449.png)
_________________________________________________________________________________________________________________________________
#### Configure slave on terminal and jenkins ui:
##### Run this commands on terminal:
```bash
kubectl exec -it <slave-pod-name> -n devops-tools -- bash
passwd jenkins
service ssh start
chmod 666 /var/run/docker.sock 
apt install gettext
```
_________________________________________________________________________________________________________________________________
#### Now change to jenkins user and connect to cluster with it:
```bash
su jenkins
gcloud container clusters get-credentials priv-gke-cluster --zone asia-east1-b --project <project-id>
````
_________________________________________________________________________________________________________________________________
