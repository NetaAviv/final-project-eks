
# WordPress Deployment on Kubernetes

This repository contains Kubernetes manifests for deploying a WordPress application with a MariaDB database on an AWS EKS cluster.

## Prerequisites
Ensure the following are installed and configured:
- Kubernetes (Minikube or AWS EKS)
- `kubectl`
- `helm`
- AWS CLI (if using AWS EKS)
- Git

## Clone the Repository
Clone the repository to your local machine:

```bash
git clone https://github.com/NetaAviv/final-project-eks.git
cd final-project-eks
```

## Deploy the Application

### 1. Apply Persistent Volumes and Claims
```bash
kubectl apply -f mysql-pvc-neta.yaml -n neta-aviv-new
kubectl apply -f wordpress-pvc.yaml -n neta-aviv-new
```

### 2. Deploy MariaDB
```bash
kubectl apply -f mariadb-deployment.yaml -n neta-aviv-new
```

### 3. Deploy WordPress
```bash
kubectl apply -f wordpress-deployment.yaml -n neta-aviv-new
```

### 4. Set Up Ingress (Ensure Nginx Ingress is Installed)
```bash
kubectl apply -f ingress.yaml -n neta-aviv-new
```

### 5. Verify Deployment
Check the status of your resources:

```bash
kubectl get pods -n neta-aviv-new
kubectl get services -n neta-aviv-new
kubectl get ingress -n neta-aviv-new
```

## Accessing the WordPress Application

Retrieve the external URL of your application:

```bash
kubectl get ingress -n neta-aviv-new
```

Look for the `HOSTS` column, which will show the URL, such as:

- **WordPress**: [http://<host-url>/](http://<host-url>/)
- **Grafana**: [http://<host-url>/grafana](http://<host-url>/grafana)

## Cleanup
To delete all resources:

```bash
kubectl delete namespace neta-aviv-new
```

---

## Contributing
Feel free to open issues or submit pull requests for improvements!

## License
This project is open-source and available under the MIT License.
