# Crossplane Jump
This repo contains some resources to make a jumpstart with Crossplane to provision resources through Kubernetes on Azure. 

## Prerequisites
* Being connected to a K8s cluster
* Helm minimum version v3.2.0
* kubectl installed
* Azure CLI installed and logged in

## Getting Started 

### Install Crosplane
* `helm repo add crossplane-stable https://charts.crossplane.io/stable && helm repo update`
* `helm install crossplane crossplane-stable/crossplane --namespace crossplane-system --create-namespace`
* Check with `kubectl get pods -n crossplane-system`

### Configure Azure Access 
* `kubectl apply -f azure-provider.yaml`
* Check if provider is healthy with `kubectl get providers`
* ` sub_id=`az account show --query "id" -o tsv` `
* `az ad sp create-for-rbac --sdk-auth --role Owner --scopes /subscriptions/$sub_id -o json >> ./azure-credentials.json`
* `kubectl create secret generic azure-secret -n crossplane-system --from-file=creds=./azure-credentials.json`
* `kubectl apply -f azure-provider-config.yaml`

## Demo 
* `kubectl apply -f azure-rg.yaml` should result in a new resource group named `cp-prov-rg` on Azure

## Additional Information
* https://docs.crossplane.io/v1.12/getting-started/provider-azure/
* https://github.com/upbound/provider-azure 
* https://marketplace.upbound.io/providers/upbound/provider-azure/v0.33.0