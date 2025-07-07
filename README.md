**Kubernetes Deployment and SSL Setup**


**Step 1: Create a Deployment File**

Write a deployment.yaml file for your application.
Apply the deployment configuration to the Kubernetes cluster.

 

**Step 2: Set Up SSL with Cert-Manager**

 # 1. Install cert-manager
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.0/cert-manager.yaml

# 2. Wait for cert-manager to be ready
kubectl wait --for=condition=ready pod -l app.kubernetes.io/name=cert-manager -n cert-manager --timeout=60s
kubectl wait --for=condition=ready pod -l app.kubernetes.io/name=webhook -n cert-manager --timeout=60s
kubectl wait --for=condition=ready pod -l app.kubernetes.io/name=cainjector -n cert-manager --timeout=60s

# 3. Verify cert-manager installation
kubectl get pods -n cert-manager

**Step 2: Verify SSL Configuration**

 apply both cluster-issuer and ingress
