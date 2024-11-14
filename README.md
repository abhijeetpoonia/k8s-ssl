**Kubernetes Deployment and SSL Setup**


**Step 1: Create a Deployment File**

Write a deployment.yaml file for your application.
Apply the deployment configuration to the Kubernetes cluster.

**Step 2: Install NGINX Ingress Controller**

Install the NGINX Ingress Controller to manage HTTP/S traffic in the cluster.
Verify the deployment of the Ingress Controller and ensure all resources are running.

**Step 3: Create and Apply Ingress Resource**

Write an ingress.yaml file to define the ingress resource for your application.
Apply the ingress configuration to allow external access to the service.
Verify that the ingress resource is created and check the assigned address and host.

**Step 4: Confirm Address and Access Using Domain**

Once the ingress is applied, confirm the address and host assigned to your application.
Access the application via the specified domain in a browser to ensure it is reachable.

**Step 5: Set Up SSL with Cert-Manager**

Install Cert-Manager Using Helm
Install Cert-Manager, which automates the process of obtaining and renewing SSL certificates.
Verify that Cert-Manager is running properly in the Kubernetes cluster.
Create ClusterIssuer YAML File
Write a clusterissuer.yaml file to define the ClusterIssuer resource, which specifies how SSL certificates should be issued (e.g., using Let's Encrypt).
Apply the ClusterIssuer resource to the cluster.
Create Certificate YAML File
Write a certificate.yaml file to define the SSL certificate resource for your domain.
The certificate resource will request an SSL certificate from the ClusterIssuer for the specified domain name.
Update Ingress Resource to Use TLS
Modify the ingress.yaml file to enable TLS (HTTPS) by referencing the generated SSL certificate.
Update the ingress resource to ensure that all traffic to the domain is securely redirected to HTTPS.

**Step 6: Verify SSL Configuration**

After applying the necessary configurations, access the application through the domain using HTTPS (e.g., https://your-domain.com).
Check that the SSL certificate is properly applied by confirming the secure connection (indicated by the lock icon in the browser).
Additional Notes:
Ensure DNS records are correctly configured to point to the IP address of the ingress controller.
If using a different certificate authority (CA) for SSL, make sure to update the ClusterIssuer with the correct configuration.
