step-1 create a deployment file 
subhanshu [ ~ ]$ cat deployment.yaml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world
  namespace: cert-manager
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: hello-world-service
  namespace: cert-manager
spec:
  type: LoadBalancer
  selector:
    app: hello-world
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80

apply kubectl apply -f deployment.yaml 

now step-2 Install the NGINX Ingress Controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
kubectl get all -n ingress-nginx
step-3 write ingress.yaml for this 
subhanshu [ ~ ]$ cat ingress.yaml 
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-world-ingress
  namespace: cert-manager
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: abhi.parveen232.tech    
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: hello-world-service
            port:
              number: 80

apply it and see 
kubectl get ingress -n cert-manager

you will get something like 
subhanshu [ ~ ]$ kubectl get ingress -n cert-manager
NAME                  CLASS   HOSTS                  ADDRESS          PORTS   AGE
hello-world-ingress   nginx   abhi.parveen232.tech   135.234.237.86   80      23m

confirm address and access it using domain 

Step-4 now work for ssl 

Install Cert-Manager using Helm:
verify installation kubectl get pods -n cert-manager

Create the ClusterIssuer YAML file (e.g., clusterissuer.yaml):
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
spec:
  acme:
    # You must replace this email address with your own.
    # Let's Encrypt will use this to contact you about expiring
    # certificates, and issues related to your account.
    email: raspberry-pi@home.pi
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      # Secret resource that will be used to store the account's private key.
      name: example-issuer-account-key
    # Add a single challenge solver, HTTP01 using nginx
    solvers:
    - http01:
        ingress:
          class: nginx
Create the Certificate YAML file (e.g., certificate.yaml):
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: hello-world-tls
  namespace: cert-manager
spec:
  secretName: hello-world-tls-secret   # Secret to store the certificate and private key
  issuerRef:
    name: letsencrypt-staging          # Reference to the ClusterIssuer
    kind: ClusterIssuer
  commonName: abhi.parveen232.tech    # Domain name you want the certificate for
  dnsNames:
    - abhi.parveen232.tech            # Add other DNS names here if needed
  acme:
    config:
      - http01:
          ingressClass: nginx          # Use the nginx ingress controller for the HTTP-01 challenge

Update Your Ingress Resource to Use TLS
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-world-ingress
  namespace: cert-manager
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: abhi.parveen232.tech
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: hello-world-service
            port:
              number: 80
  tls:
  - hosts:
    - abhi.parveen232.tech
    secretName: hello-world-tls-secret  # Reference to the secret that stores the certificate
