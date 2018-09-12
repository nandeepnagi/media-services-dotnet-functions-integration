---
services: media-services,functions
platforms: dotnet
author: johndeu
---

## Live analytics processing

This template creates a Logic app which processes a live program (from a live channel in Azure Media Services) for media analytics. What it does :

* subclips the last minute
* sends this subclip asset to Azure Media Indexer, Motion Detection and Face Redaction processors (3 tasks in one job)
* gets the text, faces and motion detection information and sends this data to a Cosmos database,
* optionally copy the faces to a dedicated Azure storage container.

## Steps

1. Create an Azure Media Services Account
2. Create Azure Media Service API principal
3. Deploy Functions:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjohndehavilland%2Fmedia-services-dotnet-functions-integration%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

3. Create a Cosmos DB with the SQL API. 

4. Create a Cosmos DB Collection (called analytics) and database (called livettml)

5. Deploy Logic Apps

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjohndehavilland%2Fmedia-services-dotnet-functions-integration%2Fmaster%2Fmedia-functions-for-logic-app%2Flogicapp4-liveanalytics-deploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

6. Download and install Azure Media Services Explorer from [here](https://github.com/Azure/Azure-Media-Services-Explorer/releases)

7. Open Azure Media Services Explorer and connect.

8. Under Live tab, first create a channel called Channel1 and then create a program called P1.

9. Start the default streaming endpoint and both the channel and program

10. Update Logic App with these values (look in the live-subclip-analytics function call).

![Screen capture](media-functions-for-logic-app/images/logicapp4-live-1.png?raw=true)

11. Create a controller logic app. Start by creating a new logic app

12. 

Notes

* you should allocate sufficient reserved units in the Media Services account otherwise the job queue will grow over time. Start with 4 S2 reserved units and monitor the queue.

### Function Application Settings 
The following applications settings are created upon deployment and are automatically linked to the resources
deployed with the azuredeploy.json template.

- **Project** - this tells the Continuous Integration system which folder to sync the Function App to.  You can modify this at any time to point to a different folder in the main solution repo to try other sample Functions. 
- **AMSAADTenantDomain** - your Media Services Account Azure AD Tenant Domain.
- **AMSRESTAPIEndpoint** - your Media Services Account REST API endpoint.
- **AMSClientId** - your Service Principal Client Id.
- **AMSClientSecret** - your Service Principal Client Secret.
- **MediaServicesStorageAccountName** - the storage account name tied to your Media Services account.
- **MediaServicesStorageAccountKey** - the storage account key tied to your Media Services account.
- **MediaServicesAttachedStorageCredentials** - list of attached storage accounts with the key, separated by ';'. This is used by some functions. Example "amstore01;gdsgdhjgj=;amstore02;dqghjqfqfjfld="
- **StorageConnection** -  the functions.json file contains a "StorageConnection" property which must be set to an App Setting value that
  contains a connection string for your input storage account. Otherwise, you may end up with an error message at startup.
  Make sure to add a new AppSetting to your Functions project with the storage account name and connection string, and update
  the functions.json file if you see this error:
- **SigningKey** - the 64-byte Base64 encoded signing key to use to protect and secure your WebHooks callbacks from Azure Media Services.
    This key is for sample purposes only and you should replace this key with your own.
    
    Example value: `wOlDEUJ4/VN1No8HxVxpsRvej0DZrO5DXvImGLjFhfctPGFiMkUA0Cj8HSfJW7lePX9XsfHAMhw30p0yYqG+1A==`
* **WebHookEndpoint** - the Webhook URL endpoint for the deployed Notification_Webhook_Function in this project to be used by Azure Media Services
  to callback to your Function from the Encoding job Functions.
  

  ### Connection Strings:
  If you are adjusting the deployment settings to use an existing Media Services account or storage account, 
  you can find the connection string for your storage account in the Azure portal(Ibiza). Go to Access Keys in Settings. In the Access Keys blade
  go to Key1, or Key2, click the "..." menu and select "view connection string". Copy the connection string.

###
  
### License
This sample project is licensed under [the MIT License](LICENSE.txt)

---
_This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments._
