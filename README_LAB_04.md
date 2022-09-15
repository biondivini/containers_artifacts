### CREATE KEYVAULT
az keyvault create --name "azr-kv-aks-lab" --resource-group "teamResources" --location "Brazil South"

### CREATE SECRET IN KEYVAULT
az keyvault secret set --name SQL-PASSWORD --vault-name azr-kv-aks-lab --value dL3mw7Jk3

### ENABLE KEYVAULT SECRET PROVIDER
az aks enable-addons --addons azure-keyvault-secrets-provider --name azr-aks-aad --resource-group teamResources


# set policy to access keys in your key vault
az keyvault set-policy -n azr-kv-aks-lab --key-permissions get --spn 18e51078-5dcb-4246-baff-28fb40eae220
# set policy to access secrets in your key vault
az keyvault set-policy -n azr-kv-aks-lab --secret-permissions get --spn 18e51078-5dcb-4246-baff-28fb40eae220
# set policy to access certs in your key vault
az keyvault set-policy -n azr-kv-aks-lab --certificate-permissions get --spn 18e51078-5dcb-4246-baff-28fb40eae220


### CREATE INGRESS
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

helm install ingress-nginx ingress-nginx/ingress-nginx --create-namespace --namespace ingress-basic --set controller.service.annotations."service.beta.kubernetes.io/azure-load-balancer-health-probe-request-path"=/healthz