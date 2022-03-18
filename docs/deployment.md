
## docker
```
docker build -t alpitg/cornplex-api:5000 .
docker push  alpitg/cornplex-api:5000    

docker tag cornplex-api:5000 alpitg/cornplex-api:5000
docker run -d -p 5000:80 --name cornplexcontainer cornplex
```
- Create docker service account for CD.

# kubectl

## connect to AKS from local
- az aks get-credentials --resource-group az-rg-free --name az-kaas-cornplex
    - (config stored in my local - C:\Users\alpit\.kube\config)
-  kubectl get deployment
- kubectl create namespace cornplex-app
- kubectl get deployment -n=cornplex-app
- kubectl get pod -n=cornplex-app
- kubectl logs `<your pod name here>` -n=cornplex-app
- kubectl expose deployment cornplex-app --type="LoadBalancer" --port=8080

- secrets
- kubectl get secret cornplex-api-app  -n cornplex-app-dev  -o jsonpath='{.data}' | base64 --decode
- echo 'MWYyZDFlMmU2N2Rm' | base64 --decode

## kube dashboard
- [ingress concept](https://kubernetes.io/docs/concepts/services-networking/ingress/)
- kubectl config get-contexts
- set KUBECONFIG %UserProfile%\.kube\config

## helm
- [install helm](https://github.com/helm/helm/releases)
- Set the helm path in environment variable.
- minikube start  # if not started for local
- kubectl config get-contexts

- az aks get-credentials --resource-group myResourceGroup --name myAKSCluster  # important for 'helm install' to connect kaas

- helm create cornplex-api-chart
- helm lint ./cornplex-api-chart    # validate the chart file
- helm install --dry-run --debug ./cornplex-api-chart --generate-name   # minikube start - if got error
 <!-- helm install sample ./cornplex-api-chart --set service.type=NodePort -->
- helm upgrade --install cornplex-app-release . --namespace=cornplex-app --set image.tag="5000" --debug --dry-run
- helm upgrade --install cornplex-app-release . --namespace=cornplex-app-dev --set image.tag="5000" --set env.ASPNETCORE_ENVIRONMENT="Development"                                     
- helm delete cornplex-app-release . --namespace=cornplex-app-dev 

- secrets
  -  helm repo add csi-secrets-store-provider-azure https://raw.githubusercontent.com/Azure/secrets-store-csi-driver-provider-azure/master/charts
  -   helm install csi-azure csi-secrets-store-provider-azure/csi-secrets-store-provider-azure --namespace cornplex-app
  -    kubectl get pod -n cornplex-app