<img align="right" width="100px" src="../../_images/Foundation29.png">

## 2.2. Level 2: Containers

Health29 consists of several connected containers.
We have divided them according to their functionality and use within the platform.Thus, we will have:

- Webapp container.
- External APIs
- Azure cognitive services container.
- Azure healthbot container.
- Azure blobs container.
- Other services container.
- Databases container.

<p style="text-align: center;">
	<img width="500px" src="../../_images/containersH29.jpg">
</p>

These modules are intercommunicated using a [REST](https://restfulapi.net/) interface, that is, the communication is established according to the [HTTP protocol](https://restfulapi.net/http-methods/). 

In this way, it will be necessary to configure the different services you want to use so that communications can be established. To do this, during the implementation of the webapp the keys and the corresponding endpoint will be used to establish communication, and the sending of the REST commands will use specific authorization headers. 

The most common headers we can find are:
- Ocp-Apim-Subscription-Key. Use with Cognitive Services subscription if you pass your secret key. 
- Authorization. Use with your Cognitive Services subscription if you pass an authentication token. The value is the bearer token: Bearer token_value 

The webapp will be the core of the Health29 application, from where the frontend will be developed and the communications with different services to provide functionalities.

Health29 is a clinical history application that will allow users to enter their medical data. Different Azure services will be used to carry out the relevant actions:

- External APIs that will be used as intermediaries between the webapp and Azure's cognitive services. To add different functionalities to the application and thus simplify the development and implementation of the webapp. 
- Healthbot. The application will have a chatbot to help the user.
- Azure blobs and databases. To store information.
- Other services that add additional functionalities. (*NOTE: In this section we will add services from Azure*)
