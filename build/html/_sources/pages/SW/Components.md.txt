<img align="right" width="100px" src="../../_images/Foundation29.png">

## 2.3. Level 3: Component

Taking into account the division by containers exposed in the previous point, we are now going to study in detail the internal architecture and the components that make up each one of them.

### 2.3.1. Webapp

Health29's architecture uses a client-server software design model, so that the architecture of the webapp is like:

<img width="800px" src="../../_images/Webapp.jpg">

For the client we use the Angular 5 framework, and for the nodejs - express server, in which we have implemented an API to connect to the CosmoDb databases using mongoose for its management.

<img width="800px" src="../../_images/Webapp_frameworks_architecture.jpg">

These modules are intercommunicated using a [REST](https://restfulapi.net/) interface, that is, the communication is established according to the [HTTP protocol](https://restfulapi.net/http-methods/). 

This communication between client and server is mainly used for data management, i.e. to obtain, add or modify information from databases.
For this purpose, an HTTP service will be used to allow, through requests from the client to the server, to carry out these operations. The server will listen to these requests and will perform the appropriate operations to give an answer to the client.

The Health29 platform has been developed as a Platform as a Service (PaaS) in Azure. That is, an App Service has been created that contains the client-server webapp. In particular, the App Service created is [health29](https://portal.azure.com/#@foundation29outlook.onmicrosoft.com/resource/subscriptions/53348303-e009-4241-9ac7-a8e4465ece27/resourceGroups/health29/providers/Microsoft.Web/sites/health29/appServices)


### 2.3.2. External APIs
**[Foundation29 API](https://f29api.northeurope.cloudapp.azure.com/index.html)**, implemented by Foundation29 to use it as an intermediary between the webapp and the azure qnamaker service. It is used for QNA functions for the different roles of Health29 platform.

<p style="text-align: center;">
	<img width="400px" src="../../_images/F29API_component.jpg">
</p>

**The [Monarch](https://api.monarchinitiative.org/) API**, is an external API that is used to obtain symptom information. 

<p style="text-align: center;">
	<img width="300px" src="../../_images/monarchAPI_component.jpg">
</p>

### 2.3.3. Azure cognitive services

As explained in the previous section of this document (2.2.Containers) several "Azure cognitive services" will be used.

In particular, services will be used for the conversion of formats, for the translation of texts and for the creation of databases for the FAQs of each group of patients.

All of them will communicate with webapp.

In the following subsections each of them is introduced.

#### 2.3.3.1. Qna maker
QnA Maker is a cloud-based Natural Language Processing (NLP) service that easily creates a natural conversation layer with the data. 

In Health29 it is used to manage the FAQs in different ways and from different points of the platform:

- To show the list of FAQs to the **users**. From the user profile you can access the FAQ page where you will be shown the results of the service consultation in the form of a list.

- So that the **administrators** of the platform can manage the list of FAQs. From the administrator profile you can add, delete and edit the list of FAQs of the service.

- It is integrated into the **healthbot** to allow users to ask questions or doubts in a more guided and personal way. 

In this case the communication of the webapp with this service is requested from the client through an external API that acts as an intermediary. However, there will also be information that will be stored in the databases, therefore, the client will also establish a communication with the server who will manage the storage of this information.

You can configure and use this azure service by following the steps in the [Microsoft guide](https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/).

In particular for Health29 the following databases have been created in qnamaker: [My knowledge bases](https://www.qnamaker.ai/Home/MyServices). One would be created for each group of patients to contain their specific information and this in turn would be replicated in as many languages as the question-answer pairs are translated on the Health29 platform.


#### 2.3.3.2. Translator
This service is used to be able to make a translation of the different datapoints, to add a new language to the platform (it translates all the tags), and to translate into English the text obtained from the vision service to call the NCR service that extracts the symptoms.

You can configure and use this azure service by following the steps in the [Microsoft guide](https://docs.microsoft.com/bs-cyrl-ba/azure/cognitive-services/translator/).

### 2.3.4. Azure healthbot

Health29 has an assistant to guide the user in the use of the platform.

<p style="text-align: center;">
	<img width="300px" src="../../_images/healthbot_component.jpg">
</p>


For this purpose, the Azure [Healthbot](https://docs.microsoft.com/en-us/healthbot/) service is used.

It guides the user through different configured scenarios where he or she can perform different actions. Initially we can divide the scenarios into three blocks:
- **New user** scenario or first time in the platform. The chatbot will provide the user with help and guidance in this process and provide he or she with more information if required.
- Scenario of **pending notifications**. In case the user has any pending notifications, he or she will be informed during the first run of the assistant.
- **Main** scenario. A series of guided scenarios will appear where the user will be able to make different types of queries.

You can configure and use this azure service by following the steps in the [Microsoft guide](https://docs.microsoft.com/en-us/healthbot/quickstart-createyourhealthcarebot). 


### 2.3.5. Azure blobs
Azure Blob storage is Microsoft's object storage solution for the cloud. Blob storage is optimized for storing massive amounts of unstructured data. Unstructured data is data that doesn't adhere to a particular data model or definition, such as text or binary data.

One "Storage accounts (classic)" container called "blobgenomics" has been created to store information from various sections of Health29:
- Medical care section. Here, one container per patient is created to store their medical information.
- Diagnosis section. As in the previous point, one container per patient is created to store the diagnostic information.
- Genotype section. One container per patient is created to store the genotype data of the patient.
- Phenotype section. Same as above but for saving the phenotype data.
- Support section. To manage the data of this section.

You can configure and use this azure service by following the steps in the [Microsoft guide](https://docs.microsoft.com/es-es/azure/storage/blobs/storage-blobs-introduction). 


### 2.3.7. Databases
We have several separate collections in two databases, one for accounts and general things, and one for patient data. 
 
The development and test environments share the same databases, while the production ones do not. So, in total we have 4 databases:
- Two for accounts
>- Development and testing
>- Production
- Two for patient data
>- Development and testing
>- Production


<p style="text-align: center;">
	<img width="600px" src="../../_images/ddbb_component.jpg">
</p>

Access from the Health29 platform is done from the application server,using [mongoose](https://mongoosejs.com/docs/).
