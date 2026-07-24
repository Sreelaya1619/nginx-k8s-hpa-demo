# Kubernetes Nginx Autoscaling (HPA) Demo

This repository demonstrates how to deploy an Nginx web server on Kubernetes and configure a **Horizontal Pod Autoscaler (HPA)** to dynamically scale replicas up and down based on CPU utilization.

## 📁 Repository Structure
```text
.
├── deployment.yaml   # Nginx Deployment configuration
├── service.yaml      # ClusterIP / NodePort Service
└── hpa.yaml          # Horizontal Pod Autoscaler configuration
```

## 🚀 How It Works

1. **Deployment & Service:** Spins up baseline Nginx pods exposed via a Kubernetes Service.
2. **Autoscaling Target:** HPA monitors average CPU usage (Target: 50%).
3. **Load Testing:** Simulated high HTTP request load triggers auto-scaling from 2 up to 10 replicas.
4. **Cooldown:** Automatically scales back down to 2 replicas when traffic drops to 0%.

## 🧪 Testing Autoscaling

To generate CPU load and test the autoscaler:

```bash
# 1. Run a load test generator using ApacheBench
kubectl run load-generator --rm -i --tty --image=alpine --restart=Never -- sh -c "apk add --no-cache apache2-utils && ab -n 500000 -c 20 http://nginx-service/"

# 2. Watch HPA scale in a separate terminal window
kubectl get hpa nginx-hpa --watch
```
