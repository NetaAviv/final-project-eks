
# WordPress Deployment on Kubernetes

This repository contains Kubernetes manifests for deploying a WordPress application with a MariaDB database on an AWS EKS cluster.

## Prerequisites
Ensure the following are installed and configured:
- Kubernetes (Minikube or AWS EKS)
- `kubectl`
- `helm`
- AWS CLI (if using AWS EKS)
- Git


## 1. Clone the Repository
Clone the repository to your local machine:

```bash
git clone https://github.com/NetaAviv/final-project-eks.git
cd final-project-eks
```


## 2. Create a storage class if needed

```bash
kubectl apply -f storage-class.yaml -n neta-aviv-new
```


## 3. Create PVCs

```bash
kubectl apply -f mysql-pvc-neta.yaml -n neta-aviv-new
kubectl apply -f wordpress-pvc.yaml -n neta-aviv-new
```


### 4. Deploy MariaDB

```bash
kubectl apply -f mysql-statefulset.yaml -n neta-aviv-new
```


### 5. Deploy WordPress

```bash
kubectl apply -f wordpress-deployment.yaml -n neta-aviv-new
```


## 6. Deploy the Application

### We have two options:
 - Ingress
 - LB

I recommand using ingress so that we could easily access our grafana page in the future using Path-based routing


### Ingress= Set Up Ingress (Ensure Nginx Ingress is Installed)
```bash
kubectl apply -f wordpress-service.yaml -n neta-aviv-new
kubectl apply -f ingress.yaml -n neta-aviv-new
```
### LB
```bash
echo "  type: LoadBalancer" >> wordpress-service.yaml
kubectl apply -f wordpress-service.yaml -n neta-aviv-new
```
to view the lb: 
```bash
kubectl get service | grep wordpress-service
```


### 7. Verify Deployment

Check the status of your resources:

```bash
kubectl get pods -n neta-aviv-new
kubectl get services -n neta-aviv-new
```


## 8. Accessing the WordPress Application- if you used ingress

Retrieve the external URL of your application:

```bash
kubectl get ingress -n neta-aviv-new
```

Look for the `HOSTS` column, which will show you a URL

In your browser:

- **To view WordPress**: http://host-url/
- **To view Grafana**: http://host-url/grafana


## 9. Cleanup
To delete all resources:

```bash
kubectl delete namespace neta-aviv-new
```

---
