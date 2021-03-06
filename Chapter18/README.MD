# Chapter 18 - Deploying Services on Azure

## Verify the implementaiton

### Push the image to the container registry

1. Login into azure:

```
az login
```
2. Create a resource group:

```
az group create --name handsOn --location westeurope
```

3. Create a registry:

```
az acr create --resource-group handsOn --name <container_registry_name> --sku Basic
```

4. Login into the registry:

```
az acr login --name <container_registry_name>
```

5. Run docker compose into the `Catalog` folder:

```
docker-compose build
```

6. Tag the image with the registry url and push the image:

```
docker tag catalog_api
<container_registry_name>.azurecr.io/catalog_api:v1

docker push <container_registry_name>.azurecr.io/catalog_api:v1
```


### Configuring Azure Container Instances

1. Create a new resource group: 

```
az group create --name handsOnContainerServices --location westeurope
```
2. Get container credentials:

```
az acr credential show -n <container_registry_name>
```

3. Populate the `registry_address`, `registry_username` and `registry_password` variables into the `aci-deploy.sh` script
4. Run the `aci-deploy.sh` script
5. Get the container resulting address from the [https://portal.azure.com](https://portal.azure.com)


### Configuring app services
1. Create a new resource group: 

```
az group create --name handsOnAppService --location westeurope
```
2. Create a new app service plan:

```
az appservice plan create --name catalogServicePlan   --resource-group handsOnAppService --sku FREE --is-linux
```

3. Populate the `registry_name` variable into the `appservice-deploy.sh` script

4. Run the `azuresql-deploy.sh` script in order to create the database instance

5. Run the `appservice-deploy.sh` script in order to setup the container

5. Get the container resulting address from the [https://portal.azure.com](https://portal.azure.com)