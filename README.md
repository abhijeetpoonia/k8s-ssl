step-1 create a deployment file 
 
apply kubectl apply -f deployment.yaml 

step-2 Install the NGINX Ingress Controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
kubectl get all -n ingress-nginx
Step-3 write ingress.yaml for this 

apply it and see 
kubectl get ingress -n cert-manager

you will get something like 
subhanshu [ ~ ]$ kubectl get ingress -n cert-manager
NAME                  CLASS   HOSTS                  ADDRESS          PORTS   AGE
hello-world-ingress   nginx   abhi.parveen232.tech   135.234.237.86   80      23m

Step-5 confirm address and access it using domain 

Step-6 now work for ssl 

Install Cert-Manager using Helm:
verify installation kubectl get pods -n cert-manager

Create the ClusterIssuer YAML file (e.g., clusterissuer.yaml):
Create the Certificate YAML file (e.g., certificate.yaml):
Update Your Ingress Resource to Use TLS
