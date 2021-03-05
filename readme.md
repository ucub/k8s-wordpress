# Wordpress Kubernetes

## How to Deploy
#### 1. Clone
Clone this repo.
#### 2. Create password using base64 because using secret option on kubernetes
Create password Using 
```sh
echo -n 'admin' | base64
```
#### 3. Edit on secret.yaml
edit this field ```password: ```
#### 4. Deploy secret
Deploy first the secret.yaml
```sh
kubectl create -f secret.yaml
```
check with ```kubectl get secret```
#### 5. Create persistent volume
This script to bind/mount $PATH of you use to datastore on host like ```/mnt/wordpress``` and ```/mnt/mysql```

Deploy PV mysql first
```sh
kubectl create -f pv-mysql.yaml
```
and then deploy pv-wordpress.yaml
```sh
kubectl create -f pv-wordpress.yaml
```

to check deployment of persistent volume you can check using :
```sh
kubectl get pv
```
#### 6. Deploy persistent volume claim to get the volume.
For mysql :
```sh
kubectl create -f pvc-mysql.yaml
```
for wordpress :
```sh
kubectl create -f pvc-wordpress.yaml
```
check using ```kubectl get pvc```
#### 7 After that deploy the mysql first
```sh
kubectl create -f mysql-deployment.yaml
```
####  8. Deploy wordpress
```sh
kubectl create -f wordpress-deployment.yaml
```
#### 9. after the ceployment, check the deployment
```sh
kubectl get all -A
```
#### 10. After that, deploy the service to expose a POD of wordpress
```sh
kubectl create -f wordpress-service.yaml
```

Check running condition of all using ```watch kubectl get all -A```
to check the error using :
- PV
```kubectl describe pv```
- PVC
```kubectl describe pvc```
- POD
```kubectl get pods``` after that ```kubectl describe {name_pods}```