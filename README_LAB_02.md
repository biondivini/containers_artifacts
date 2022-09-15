## Create AKS
az aks create --resource-group teamResources --name azr-aks-lab --node-count 1 --attach-acr registryltb1998 --generate-ssh-keys

## Create Secret
kubectl create secret generic db-user-pass --from-literal=username=sqladminlTb1998 --from-literal=password=dL3mw7Jk3 -n api
kubectl create secret generic db-user-passç-java --from-literal=username=sqladminlTb1998 --from-literal=password=dL3mw7Jk3 -n api
kubectl create secret generic db-user-pass --from-literal=username=sqladminlTb1998 --from-literal=password=dL3mw7Jk3 -n web