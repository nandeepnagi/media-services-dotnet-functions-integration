# Logic Apps which use Azure Functions and Azure Media Services

### 1. Create an Azure Media Services account

Create a Media Services account in your subscription if don't have it already.

### 2. Create a Service Principal

Create a Service Principal and save the password. It will be needed in step #4. To do so, go to the API tab in the account ([follow this article](https://docs.microsoft.com/en-us/azure/media-services/media-services-portal-get-started-with-aad#service-principal-authentication))

### 3. Make sure the AMS streaming endpoint is started

To enable streaming, go to the Azure portal, select the Azure Media Services account which has been created, and start the default streaming endpoint.

![Screen capture](images/start-se-1.png?raw=true)

![Screen capture](images/start-se-2.png?raw=true)

### 4. Deploy the Azure functions
If not already done : fork the repo, deploy Azure Functions and select the **"media-functions-for-logic-app"** Project (IMPORTANT!)

Follow the guidelines in the [git tutorial](1-CONTRIBUTION-GUIDE/git-tutorial.md) for details on how to fork the project and use Git properly with this project.

Note : if you never provided your GitHub account in the Azure portal before, the continous integration probably will probably fail and you won't see the functions. In that case, you need to setup it manually. Go to your azure functions deployment / Functions app settings / Configure continous integration. Select GitHub as a source and configure it to use your fork.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fmedia-services-dotnet-functions-integration%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>




## Live analytics processing

This template creates a Logic app which processes a live program (from a live channel in Azure Media Services) for media analytics. What it does :

* subclips the last minute
* sends this subclip asset to Azure Media Indexer, Motion Detection and Face Redaction processors (3 tasks in one job)
* gets the text, faces and motion detection information and sends this data to a Cosmos database,
* optionally copy the faces to a dedicated Azure storage container.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fmedia-services-dotnet-functions-integration%2Fmaster%2Fmedia-functions-for-logic-app%2Flogicapp4-liveanalytics-deploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Then fix the errors.

You need to customize the channel name, program name and language of the audio. To do so, change the parameters in the live-subclip-analytics function call.

![Screen capture](images/logicapp4-live-1.png?raw=true)

Notes

* you should allocate sufficient reserved units in the Media Services account otherwise the job queue will grow over time. Start with 4 S2 reserved units and monitor the queue. 
