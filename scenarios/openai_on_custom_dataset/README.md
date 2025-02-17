# Using Azure OpenAI on custom dataset
### Scenario summary:
This scenario allows use cases to use Open AI as an intelligent agent to answer questions from end users or assist them using knowledge of a proprietary corpus and domain.
Applications can be: 
- Giving direct answer to questions about specific product, service and process based on a knowledge corpus that can be updated frequently. This is an alternative to classic search where the result are just documents with relevant information to the question. Think of this as Bing Chat on proprietary data.
- Giving recommendation & assistance: based on information that can be implicitly gathered about the user, formulate useful content for the user's purpose. For example, a travel website may utilize users' personal information, past posts and transaction history to personalize recommendations when users need to be helped with creating next trip idea/itinerary

Regardless of the application scenario, the solution flow is:
- Step 1 prepare the context information: context information can be retrieved from proprietary knowledge corpus and other systems based on the user's query and user's information. The retrieval mechanism can be a semantic search engine to retrieve right content for unstructured data corpus or SQL query in case of structured dataset.
- Step 2 fomulate prompt to Open AI: from the context and depending on the goal of user, formulate GPT prompt to get the final response to end user. For example, if it's knowlege retrieval vs. recommendation

This implementation scenario focuses on building a knowledge retrieval chatbot application on top of unstructured data corpus but the same design can be used for recommendation & generative scenarios.

### Architecture Diagram
![OpenAI on custom dataset](../../documents/media/AzureCognitiveSearchOpenAIArchitecture.png)
From the user's query, the solution uses two-stage information retrieval to retrieve the content that best matches the user query. 
In stage 1, full text search in Azure Cognitive Search is used to retrieve a number of relevant documents. In stage 2, the search result is applied with pretrained NLP model and embedding search to further narrow down the the most relavant content. The content is used by orchestrator service to form a prompt to OpenAI deployment of LLM. The OpenAI service returns result which is then sent to Power App client application.
### Deployment


### Prerequisites

* [PostMan Client Installed](https://www.postman.com/downloads/) for testing Azure Functions. Azure portal can also be used to test Azure Functions.  
* Azure Cloud Shell is recommended as it comes with preinstalled dependencies. 
* Azure Open AI already provisioned and text-davinci-003 model is deployed. Other deployments can also be used, the configs below needs to be updated accordingly.  
* Conda is recommended if local laptops are used as pip install might interfere with existing python deployment.



## 1. Azure services deployment

Deploy Azure Resources namely - Azure Function App to host facade for OpenAI and Search APIs, Azure Search Service and a Azure Form Recognizer resource.

Here are the SKUs that are needed for the Azure Resources:

- Azure Function App - Consumption Plan
- Azure Cognitive Search - Standard (To support semantic search)
- Azure Forms Recognizer - Standard (To support analyzing 500 page document)
- Azure Storage - general purpose V1 (Needed for Azure Function App and uploading sample documents)


The Azure Function App also deploys the function code needed for powerapps automate flow. 


[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmicrosoft%2FOpenAIWorkshop%2Fanildwa-dev%2Fscenarios%2Fopenai_on_custom_dataset%2Fdeploy%2Fazure-deploy.json)



- Step 1: Setup Azure Cognitive Search and prepare data

    As part of data preperation step, to work in Open AI, the documents are chunked into smaller units(20 lines) and stored as individual documents in the search index. The chunking steps can be achieved with a python script below. 
    To make it easy for the labs, the sample document has already been chunked and provided in the repo. 

    * Enable Semantic Search on Azure Portal. Navigate to Semantic Search blade and select Free plan. 
    
        ![](../../documents/media/enable-semantic-search.png)

    *   Create Search Index, Sematic Configuration and Index a few documents using automated script. The script can be run multiple times without any side effects.
        Run the below commands from cmd prompt to conifgure python environment. Conda is optional if running in Azure Cloud Shell or if an isolated python environment is needed. 

            
            git clone <repo>
            cd OpenAIWorkshop\scenarios\openai_on_custom_dataset
            conda create env -n openaiworkshop python=3.9
            conda activate openaiworkshop
            pip install -r .\orchestrator\requirements.txt


    *   Update Azure Search, Open AI endpoints, AFR Endpoint and API Keys in the secrets.env. 
        Rename secrets.rename to secrets.env. (This is recommended to prevent secrets from leaking into external environments.)
        The secrets.env should be placed in the ingest folder along side the python script file search-indexer.py.
        The endpoints below needs to have the trailing '/' at end for the search-indexer to run correctly.

            AZURE_OPENAI_API_KEY=""
            AZURE_OPENAI_ENDPOINT="https://<>.openai.azure.com/"
            AZURE_OPENAI_API_KEY_EASTUS=""
            AZURE_OPENAI_ENDPOINT_EASTUS="https://<>.openai.azure.com/"

            AZSEARCH_EP="https://<>.search.windows.net/"
            AZSEARCH_KEY=""
            AFR_ENDPOINT="https://westus2.api.cognitive.microsoft.com/"
            AFR_API_KEY=""
            INDEX_NAME="azure-ml-docs"

    *   The document processing, chunking, indexing can all be scripted using any preferred language. 
        This repo uses Python. Run the below script to create search index, add semantic configuration and populate few sample documents from Azure doc. 
        The search indexer chunks a sample pdf document(500 pages) which is downloaded from azure docs and chunks each page into 20 lines. Each chunk is created as a new seach doc in the index. The pdf document processing is achieved using Azure Form Recognizer service. 
     

            cd .\scenarios\openai_on_custom_dataset\ingest\
            python .\search-indexer.py
            

    *   Optional Manual Approach. If you prefer to not use the python/automated approach above, the below steps can be followed without automation script. 
        To configure Azure Search, please follow the steps below

        - In the storage container, that is created as part of the template in step 1, create a blob container. 
        - Extract the data files in the .scenarios/data/data-files.zip folder and update this folder to the blob container using Azure Portal UI.   The data-files.zip contains the Azure ML sample pdf document chunked as individual files per page.  
        - Import data in Azure Search as shown below. Choose the blob container and provide the blob-folder name in to continue. 

            ![](../../documents/media/search1.png)
        - In the Customize Target Index, use id as the Azure Document Key and mark text as the Searchable Field. 
        - This should index the chunked sample

## Step 2: Automated orchestrator service with Azure Function App

    Update the below configuration in Azure Function App configuration blade. 

            {
                "name": "GPT_ENGINE",
                "value": "text-davinci-003",
                "slotSetting": false
            },
            {
                "name": "INDEX_NAME",
                "value": "azure-aml-docs",
                "slotSetting": false
            },
            {
                "name": "OPENAI_API_KEY",
                "value": "<>",
                "slotSetting": false
            },
            {
                "name": "OPENAI_RESOURCE_ENDPOINT",
                "value": "https://<>.openai.azure.com/",
                "slotSetting": false
            },
            {
                "name": "SEMANTIC_CONFIG",
                "value": "semantic-config",
                "slotSetting": false
            }

## Step 3. Test Azure service deployment

Launch Postman and test the Azure Function to make sure it is returning results. The num_search_result query parameter can be altered to retrieve more or less search results. Notice the query parameter num_search_result in the screen shot below. 

![](../../documents/media/postman.png)

## Step 4. Deploy client Power App

Navigate to https://make.powerapps.com/ and click on Apps on the left navigation. 

![](../../documents/media/powerapps1.png)


From the top nav bar, click Import Canvas App and upload the Semantic-Search-Package.zip file from this git repo path. 


![](../../documents/media/powerapps2.png)


![](../../documents/media/powerapps3.png)


Click on Import to import the package into powerapps environment. 


![](../../documents/media/powerapps4.png)


This will import the Power App canvas app and Semantic-Search Power Automate Flow into the workspace. 



![](../../documents/media/powerapps7.png)


Edit the Power Automate Flow and update Azure Function Url. Optionaly num_search_result query parameter can be altered.



![](../../documents/media/powerapps5.png)

## Step 5. Test

Click on the play button on the top right corner in the PowerApps Portal to launch PowerApp.
Select an  FAQ from dropdown and click Search. This is should bring up the answers powered by Open AI GPT-3 Models. 
Feel free to make changes to the PowerApps UI to add your own functionality and UI layout. You can explore expanding PowerAutomate flow to connect to other APIs to provide useful reference links to augment the response returned from OpenAI.


