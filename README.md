# jubilant-funicular

1. Create Standard Cluster

- 2 nodes
- e2.small
- name: jubicular-cluster
- location: europe-west4-a

2. Set kubectl context
   kubectl config get-contexts
   kubectl config delete-context CONTEXT_NAME

- Add new context for another cluster (assumes gcloud init had default zone assigned, otherwise use --region=COMPUTE_REGION)
  gcloud container clusters get-credentials jubicular-cluster

3. Install ingress via helm
   helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
   helm repo update
   helm install ingress-nginx ingress-nginx/ingress-nginx -n ingress-nginx --create-namespace

- Check status of load balancer IP
  kubectl get service --namespace ingress-nginx ingress-nginx-controller --output wide --watch

4. Install cert-manager via helm
   helm repo add jetstack https://charts.jetstack.io --force-update
   helm repo update

- Install Helm chart - since this is not prod, just use --set installCRDs=true
  helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.14.2 --set installCRDs=true

5. Deploy project using Github actions
