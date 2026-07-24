# Kubernetes Nginx Scaling & HPA Demo

This repository documents the step-by-step implementation of scaling an Nginx web application on Kubernetes—progressing from a single individual Pod to a multi-replica Deployment, and finally implementing dynamic auto-scaling using a **Horizontal Pod Autoscaler (HPA)**.

---

## 📌 Implementation Milestones

### 1. Single Nginx Pod
* Initial deployment running a standalone single pod to verify baseline configuration and service routing.

### 2. Multi-Replica Deployment
* Scaled up to a fixed **3-replica Deployment** to establish high availability and load distribution across multiple instances.

### 3. Dynamic Horizontal Auto-scaling (HPA)
* Configured an HPA targeted at **50% CPU utilization**:
  * **Min Replicas:** `2`
  * **Max Replicas:** `10`

---

## 📁 Repository Structure

```text
.
├── deployment.yaml   # Nginx Deployment (baseline 3 replicas)
├── service.yaml      # Kubernetes Service (ClusterIP / NodePort)
└── hpa.yaml          # Horizontal Pod Autoscaler (2 to 10 replicas)
```

---

## 🧪 Testing Autoscaling

To generate CPU load and test the HPA scale-up/scale-down behavior:

```bash
# 1. Run continuous traffic generator using ApacheBench
kubectl run load-generator --rm -i --tty --image=alpine --restart=Never -- sh -c "apk add --no-cache apache2-utils && ab -n 500000 -c 20 http://nginx-service/"

# 2. Monitor autoscaling in real time in a second terminal
kubectl get hpa nginx-hpa --watch
```

### Expected Scaling Behavior:
1. **Idle State:** Baseline runs at `2` replicas (~0–8% CPU).
2. **Traffic Surge:** High request volume spikes CPU past `50%`, triggering scale-up to `4` $\rightarrow$ `8` $\rightarrow$ up to `10` replicas as needed.
3. **Cooldown Phase:** Once traffic stops, CPU drops to `0%`. After a ~5-minute stabilization window, HPA automatically scales back down to `2` replicas.
