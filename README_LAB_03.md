# GET ID VNET 
az network vnet subnet list --resource-group teamResources --vnet-name vnet --query "[0].id" --output tsv

# CREATE AKS
az aks create --resource-group teamResources --name azr-aks-aad --enable-aad --aad-admin-group-object-ids 5e5e05ef-2bf9-489c-97e2-3297f2e13cea --network-plugin azure --vnet-subnet-id "/subscriptions/1e4f899e-c18a-482c-b3ad-c26d2c742b4d/resourceGroups/teamResources/providers/Microsoft.Network/virtualNetworks/vnet/subnets/vm-subnet" --generate-ssh-keys --vm-set-type VirtualMachineScaleSets --load-balancer-sku standard --node-count 3 --zones 1 2 3

# ATTACH ACR
az aks update -n azr-aks-aad -g teamResources --attach-acr /subscriptions/1e4f899e-c18a-482c-b3ad-c26d2c742b4d/resourceGroups/teamResources/providers/Microsoft.ContainerRegistry/registries/registryltb1998


# LOGIN
az login 
az aks get-credentials --resource-group teamResources --name azr-aks-aad

# ADD GROUP > AKS
az aks show --resource-group teamResources --name azr-aks-aad --query id -o tsv
az role assignment create --assignee 0b8b55c0-d4c9-4624-977a-a59ee9b94d5b --scope /subscriptions/1e4f899e-c18a-482c-b3ad-c26d2c742b4d/resourcegroups/teamResources/providers/Microsoft.ContainerService/managedClusters/azr-aks-aad --role "Azure Kubernetes Service Cluster User Role"
az role assignment create --assignee 368f7716-ccef-448e-b517-f47061b5bbd8 --scope /subscriptions/1e4f899e-c18a-482c-b3ad-c26d2c742b4d/resourcegroups/teamResources/providers/Microsoft.ContainerService/managedClusters/azr-aks-aad --role "Azure Kubernetes Service Cluster User Role"

# Create Roles
kubectl create role Editor --resource=pod,svc,deploy --verb=* -n api
kubectl create role Editor --resource=pod,svc,deploy --verb=* -n web
kubectl create role Reader --resource=pod,svc,deploy --verb=get,list -n web
kubectl create role Reader --resource=pod,svc,deploy --verb=get,list -n api

# Create RoleBindings


#######################
### Sua equipe criou com sucesso um cluster AKS habilitado pela RBAC dentro do espaço de endereço alocado para você pela equipe de rede
### Sua equipe reimplantou com sucesso o aplicativo TripInsights, agora segmentado e namespaces

kubectl get po -n api

NAME                           READY   STATUS    RESTARTS   AGE
poi-76959d5cbb-nw7bq           1/1     Running   0          82m
trips-956858bcd-vlt8g          1/1     Running   0          82m
user-java-7984c8697c-zsmqg     1/1     Running   0          80m
userprofile-744d4448f8-wjf6f   1/1     Running   0          82m

kubectl get po -n web

NAME                              READY   STATUS    RESTARTS   AGE
dpl-tripviewer-6bdf98c54b-l62x9   1/1     Running   0          82m

### Sua equipe deve demonstrar conectividade de e para o seu cluster, podendo alcançar o (já implantado)internal-vm

### Sua equipe deve demonstrar que você é solicitado no acesso de cluster para autenticar com AAD
# az login
A web browser has been opened at https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize. Please continue the login in the web browser. If no web browser is available or if the web browser fails to open, use device code flow with `az login --use-device-code`.
[
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "e8b71458-a9f3-4f89-8c30-67945f9d4c98",
    "id": "1e4f899e-c18a-482c-b3ad-c26d2c742b4d",
    "isDefault": true,
    "managedByTenants": [],
    "name": "Azure subscription 1",
    "state": "Enabled",
    "tenantId": "e8b71458-a9f3-4f89-8c30-67945f9d4c98",
    "user": {
      "name": "hacker5o7g@msftopenhack7093ops.onmicrosoft.com",
      "type": "user"
    }
  }
]

# az aks get-credentials --resource-group teamResources --name azr-aks-aad
A different object named clusterUser_teamResources_azr-aks-aad already exists in your kubeconfig file.
Overwrite? (y/n): y
Merged "azr-aks-aad" as current context in C:\Users\Bolsa Balcão Brasil\.kube\config

# kubectl get po
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FSY9VD5TT to authenticate.
No resources found in default namespace.

# kubectl get po -n api
NAME                           READY   STATUS    RESTARTS   AGE
poi-76959d5cbb-nw7bq           1/1     Running   0          88m
trips-956858bcd-vlt8g          1/1     Running   0          88m
user-java-7984c8697c-zsmqg     1/1     Running   0          86m
userprofile-744d4448f8-wjf6f   1/1     Running   0          88m

### Diferentes membros da sua equipe devem ser capazes de se conectar ao seu cluster usando os usuários AAD de api-dev e web-dev e demonstrar níveis de acesso apropriados

### API ###
# az login -u apidev@msftopenhack7093ops.onmicrosoft.com -p gQ6xk0Ky9
[
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "e8b71458-a9f3-4f89-8c30-67945f9d4c98",
    "id": "1e4f899e-c18a-482c-b3ad-c26d2c742b4d",
    "isDefault": true,
    "managedByTenants": [],
    "name": "Azure subscription 1",
    "state": "Enabled",
    "tenantId": "e8b71458-a9f3-4f89-8c30-67945f9d4c98",
    "user": {
      "name": "apidev@msftopenhack7093ops.onmicrosoft.com",
      "type": "user"
    }
  }
]

# az aks get-credentials --resource-group teamResources --name azr-aks-aad
A different object named clusterUser_teamResources_azr-aks-aad already exists in your kubeconfig file.
Overwrite? (y/n): y
Merged "azr-aks-aad" as current context in C:\Users\Bolsa Balcão Brasil\.kube\config

# kubectl get po -n api
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FCDMZ3WEG to authenticate.
NAME                           READY   STATUS    RESTARTS   AGE
poi-76959d5cbb-nw7bq           1/1     Running   0          91m
trips-956858bcd-vlt8g          1/1     Running   0          91m
user-java-7984c8697c-zsmqg     1/1     Running   0          89m
userprofile-744d4448f8-wjf6f   1/1     Running   0          91m

# kubectl get po -n web
NAME                              READY   STATUS    RESTARTS   AGE
dpl-tripviewer-6bdf98c54b-l62x9   1/1     Running   0          91m

# kubectl create svc clusterip banana --tcp=80:80 -n api
service/banana created

# kubectl create svc clusterip banana --tcp=80:80 -n web
error: failed to create ClusterIP service: services is forbidden: User "apidev@msftopenhack7093ops.onmicrosoft.com" cannot create resource "services" in API group "" in the namespace "web"

# kubectl delete svc banana -n api
service "banana" deleted

### WEB ###

# az login -u webdev@msftopenhack7093ops.onmicrosoft.com -p jX3ml6Kk3
[
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "e8b71458-a9f3-4f89-8c30-67945f9d4c98",
    "id": "1e4f899e-c18a-482c-b3ad-c26d2c742b4d",
    "isDefault": true,
    "managedByTenants": [],
    "name": "Azure subscription 1",
    "state": "Enabled",
    "tenantId": "e8b71458-a9f3-4f89-8c30-67945f9d4c98",
    "user": {
      "name": "webdev@msftopenhack7093ops.onmicrosoft.com",
      "type": "user"
    }
  }
]

# az aks get-credentials --resource-group teamResources --name azr-aks-aad
A different object named clusterUser_teamResources_azr-aks-aad already exists in your kubeconfig file.
Overwrite? (y/n): y
Merged "azr-aks-aad" as current context in C:\Users\Bolsa Balcão Brasil\.kube\config

# kubectl get po -n api
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code RCBHAUKMG to authenticate.
NAME                           READY   STATUS    RESTARTS   AGE
poi-76959d5cbb-nw7bq           1/1     Running   0          95m
trips-956858bcd-vlt8g          1/1     Running   0          95m
user-java-7984c8697c-zsmqg     1/1     Running   0          93m
userprofile-744d4448f8-wjf6f   1/1     Running   0          95m

# kubectl get po -n web
NAME                              READY   STATUS    RESTARTS   AGE
dpl-tripviewer-6bdf98c54b-l62x9   1/1     Running   0          95m

# kubectl create svc clusterip banana --tcp=80:80 -n api
error: failed to create ClusterIP service: services is forbidden: User "webdev@msftopenhack7093ops.onmicrosoft.com" cannot create resource "services" in API group "" in the namespace "api"

# kubectl create svc clusterip banana --tcp=80:80 -n web
service/banana created

# kubectl delete svc banana -n web
service "banana" deleted
