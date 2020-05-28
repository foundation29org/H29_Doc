<img align="right" width="100px" src="../_images/Foundation29.png">

# 1. Environments
The different phases defined for software development are followed:

<p style="text-align: center;">
	<img width="400px" src="../_images/applicationPhases.jpg">
</p>

In the development phase, it will be achieved: 
- Integrate the work of different developers in a central repository, resulting in an updated and consolidated version of the code.
- Automate the integration tests and their validation before being moved to the next environment.
- Send the code to the next environment if the tests have been passed successfully.

Once the integration tests in the development environment have been passed, the code will be moved to the pre-production (test) environment. Here the validation tests will be performed to the whole software, with the objective of locating any error before reaching the production environment and thus avoid problems arising from them. This environment can also be used as a demo environment, where the final customer can test the new application or the modifications or corrections made to the existing application. From here, the customer's impressions will be extracted and possible deficiencies in the initial requirements, in the design or in its implementation will be located early on.
Finally, the version will be moved into the production environment.

So, the Health29 platform is divided into three environments:
- The **development** environment, for programmers.
- The **test** environment for the pre-production phase.
- The **production** environment for users.

<img width="800px" src="../_images/Environments.jpg">

On the one hand, These environments are created as [slots of an Azure App Service](https://docs.microsoft.com/en-us/azure/azure-functions/functions-deployment-slots). Deployment slots are live apps with their own hostnames. 
In particular, you can access the **Health29's Azure App Service** through the following [link](https://portal.azure.com/#@foundation29outlook.onmicrosoft.com/resource/subscriptions/53348303-e009-4241-9ac7-a8e4465ece27/resourceGroups/health29/providers/Microsoft.Web/sites/health29/appServices). 

On the other hand, each environment will use specific or common resources or containers for all. Thus:
- The client-server webapp will be located in a different repository for each environment.
- The healthbot will also be specific to each environment
- There will be two database containers: the test and development environments will share the resource while production will have its own.
- The rest of the resources will be shared by all the environments.

We will not go into more detail on this since the functionality, architecture and usability of each of these containers will be explained in section 2 of this document.

In this section we will only explain the architecture of the platform environments, the distribution of the webapp repositories and the steps to follow for the build and the deploy of the platform.

Therefore, the section will be divided into different sections:
- Design and implementation of the Azure App Service architecture.
- Division of the code repositories for each environment
- Build and deploy of each environment




