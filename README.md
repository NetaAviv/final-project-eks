
# Neta's Project: WordPress Deployment on Kubernetes

This repository contains Kubernetes manifests for deploying a WordPress application with a MariaDB database on an AWS EKS cluster.

## Prerequisites
Ensure the following are installed and configured:
- Kubernetes (Minikube or AWS EKS)
- kubectl
- helm
- AWS CLI (to connect to AWS EKS)
- Git


## 1. Clone the Repository
Clone the repository to your local machine:

```bash
git clone https://github.com/NetaAviv/final-project-eks.git
cd final-project-eks
```


## 2. Create a storage class if needed

```bash
kubectl apply -f storage-class.yaml -n [enter your namespace here]
```

## 3. Deploy MariaDB

```bash
kubectl apply -f app/mysql -n [enter your namespace here]
```

## 4. Before deploying the wordpress Application

### You need to choose between two options:
 - Ingress
 - LB

I recommand using ingress so that we could easily access our grafana page in the future using Path-based routing


### Ingress= Set Up Ingress
```bash
helm install [name of your new ingress] ingress-nginx/ingress-nginx --namespace [enter your namespace here] --set controller.ingressClassResource.name=[name your ingress class] -f values.yaml
kubectl apply -f ingress.yaml -n [enter your namespace here]
```
### LB
```bash
echo "  type: LoadBalancer" >> app/wordpress/wordpress-service.yaml
```
to view the lb: 
```bash
kubectl get service | grep wordpress-service
```

## 5. Deploy Wordpress

```bash
kubectl apply -f app/wordpress -n [enter your namespace here]
```

## 6. Verify Deployment


Check the status of your resources:

```bash
kubectl get pods -n [enter your namespace here]
kubectl get services -n [enter your namespace here]
```


## 7. Install helm for grafana and prometheus:


```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```
```bash
helm install [RELEASE_NAME] prometheus-community/kube-prometheus-stack
```


## 8. Accessing the WordPress Application- if you used ingress

Retrieve the external URL of your application:

```bash
kubectl get ingress -n [enter your namespace here]
```

Look for the `HOSTS` column, which will show you a URL

In your browser:

- **To view WordPress**: http://host-url/
- **To view Grafana**: http://host-url/grafana



## 9. Cleanup
To delete all resources:

```bash
kubectl delete namespace [enter your namespace here]
```

---
