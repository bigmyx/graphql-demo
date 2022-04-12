# Deployment

  

### Install cert-manager

- Install the CustomResourceDefinition
   ```sh
  kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.8.0/cert-manager.crds.yaml
  ```
- Add the Jetstack Helm repository
   ```sh
	helm repo add jetstack https://charts.jetstack.io
  ```
- Update your local Helm chart repository cache
   ```sh
	helm repo update
  ```
- Update your local Helm chart repository cache
   ```sh
	helm install \
	    cert-manager jetstack/cert-manager \
	    --namespace cert-manager \
	    --create-namespace \
	    --version v1.8.0
  ```
- Verify the installation
   ```sh
	$ kubectl get pods --namespace cert-manager
  NAME                                       READY   STATUS    RESTARTS   AGE
  cert-manager-5c6866597-zw7kh               1/1     Running   0          2m
  cert-manager-cainjector-577f6d9fd7-tr77l   1/1     Running   0          2m
  cert-manager-webhook-787858fcdb-nlzsq      1/1     Running   0          2m
  ```

### Deploy the app

> NOTE: Replace `demo.bigmyx.net` with your domain in Ingress file `ingress.yml`

- Create Issuer
   ```sh
	kubectl apply -f clusterissuer.yml
  ```
- Create Ingress
   ```sh
	kubectl apply -f ingress.yml
  ```
- Create Deployments
  ```sh
	kubectl apply -f deployments.yml
  ```
- Create Services
  ```sh
	kubectl apply -f services.yml
  ```
- To workaround cert-manager's ACME HTTP-1 challenge limitation (it needs `/` path to return `200` code), deploy Nginx service:  
  ```sh
	kubectl apply -f nginx.yml
  ```
- Update DNS A record to point to the newly assigned Ingress IP address.
