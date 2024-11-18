# Retrieval Augmented Generation (RAG) with Azure

A Retrieval Augmented Generation example with Azure, using Azure OpenAI Service, Azure Cognitive Search, embeddings, and a sample CSV file to produce
a powerful grounding to applications that want to deliver customized generative AI applications.

## Install the prerequisites

Use the `requirements.txt` to install all dependencies

```bash
python -m venv .venv
./.venv/bin/pip install -r requirements.txt
```

### Add your keys

Find the Azure OpenAI Keys in the Azure OpenAI Service. Note, that keys aren't in the studio, but in the resource itself. Add them to a local `.env` file. This repository ignores the `.env` file to prevent you (and me) from adding these keys by mistake.

Your `.env` file should look like this:

```
# Azure OpenAI
OPENAI_API_TYPE="azure"
OPENAI_API_BASE="https://dedssdds.openai.azure.com/"
OPENAI_API_KEY="0ds34"
OPENAI_API_VERSION="2dsew"

# Azure Cognitive Search
SEARCH_SERVICE_NAME="https://ddsch.windows.net"
SEARCH_API_KEY="zlkjhsdg098srtiuy"
SEARCH_INDEX_NAME="demdsx"
```
Alternative: please update secureity in setting, the AZURE_CREDENTIALS will have format explained later.
![image](https://github.com/user-attachments/assets/ef27f86c-166a-4d55-ba31-d260075d83a1)

Note that the Azure Cognitive Search is only needed if you are following the Retrieval Augmented Guidance (RAG) demo. It isn't required for a simple Chat application.

## Generate a PAT

The access token will need to be added as an Action secret. [Create one](https://github.com/settings/tokens/new?description=Azure+Container+Apps+access&scopes=write:packages) with enough permissions to write to packages. It is needed because Azure will need to authenticate against the GitHub Container Registry to pull the image.

## Get an Azure Service Principal

You'll need the following:

1. An Azure subscription ID [find it here](https://portal.azure.com/#view/Microsoft_Azure_Billing/SubscriptionsBlade)
2. Then go to Azure web, open terminal and using:
   az ad sp create-for-rbac `
         --name "myApp" --role contributor `
         --scopes /subscriptions/<Subscription_id> `
         --sdk-auth

   Put the output format to AZURE_CREDENTIALS which is the output of previous command.

   {
  "clientId": "your_clientd",
  "clientSecret": "your_clientSecret",
  "subscriptionId": "your_subscriptionId",
  "tenantId": "your_tenantId",
  "activeDirectoryEndpointUrl": "your_activeDirectoryEndpointUrl",
  "resourceManagerEndpointUrl": "https://.azure.com/",
  "activeDirectoryGraphResourceId": "https://.windows.net/",
  "sqlManagementEndpointUrl": "https://manas.net:8443/",
  "galleryEndpointUrl": "https://g.com/",
  "managementEndpointUrl": "https://mwindows.net/"
}



## Azure Container Apps

Make sure you have one instance already created, and then capture the name and resource group. These will be used in the workflow file.

## Change defaults 

Make sure you use 2 CPU cores and 4GB of memory per container. Otherwise you may get an error because loading HuggingFace with FastAPI requires significant memory upfront.

## Gotchas

There are a few things that might get you into a failed state when deploying:

* Not config right in workflow/main.yml
  Please give the right name of your application
    AZURE_CONTAINER_APP_NAME: container
    AZURE_GROUP_NAME: zure_duke

* Not having enough RAM per container
* Not using authentication for accessing the remote registry (ghcr.io in this case). Authentication is always required
* Not using a `GITHUB_TOKEN` or not setting the write permissions for "packages". Go to `settings/actions` and make sure that "Read and write permissions" is set for "Workflow permissions" section
* Different port than 80 in the container. By default Azure Container Apps use 80. Update to match the container.



