# demopythonaca
Demo application in Python hosted on Azure Container Apps

## Build the container
`docker build src\. -t demopythonaca:v1`

## Run the container locally

`docker run -d -p 80:8080 demopythonaca:v1`

## Deploy the infrastructure

### Login to Azure
`az login`

### Create the resource group
`az group create --location francecentral --name demopythonaca-rg`

### Deploy the bicep template
`az deployment group create --resource-group demopythonaca-rg --template-file ./Infrastructure/main.bicep`

## Push the Docker image to Azure

### Get the credentials from Azure Portal

![Get the credentials from Azure Portal](/assets/acr-credentials.png "")

Replace the <REGISTRY_SERVER> below with login server

### Login to Azure Container Registry
`docker login <REGISTRY_SERVER>`

### Tag local image
`docker tag demopythonaca:v1 <REGISTRY_SERVER>/demopythonaca`

### Push image to Azure
`docker push <REGISTRY_SERVER>/demopythonaca`

## Create the Container app

### Define some variables (Get the password from ACR Access keys blade)

Replace the <REGISTRY_SERVER> below with ACR login server

Replace the <REGISTRY_USERNAME> with ACR username

Replace the <REGISTRY_PASSWORD> with ACR password

`
CONTAINER_IMAGE_NAME=demopythonaca
REGISTRY_SERVER=<REGISTRY_SERVER>
REGISTRY_USERNAME=<REGISTRY_USERNAME>
REGISTRY_PASSWORD=<REGISTRY_PASSWORD>
`

### Create the Container app
`
az containerapp create
  --name my-container-app
  --resource-group $RESOURCE_GROUP
  --image $CONTAINER_IMAGE_NAME
  --environment $CONTAINERAPPS_ENVIRONMENT
  --registry-server $REGISTRY_SERVER
  --registry-username $REGISTRY_USERNAME
  --registry-password $REGISTRY_PASSWORD
`
