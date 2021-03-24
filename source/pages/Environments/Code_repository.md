<img align="right" width="100px" src="../../_images/Foundation29.png">

## 1.2. Code repositories

The code is open source, and is public on GitHub. 
- [Client](https://github.com/foundation29org/H29_Client) and [Server](https://github.com/foundation29org/H29_Server)

For the development and versioning of the code we will follow the GitFlow defined in: [GitFlow](https://blog.axosoft.com/gitflow/) 

<p style="text-align: center;">
	<img width="500px" src="../../_images/gitflow.jpg">
</p>

Each environment will be composed of two projects: client and server, using a REST architecture.
Therefore, it will have the characteristics of this type of implementation. Among them, we can mainly highlight:

**Uniform interface**
- The interface is resource-based.
- The server will send the data, so that communication with the databases will be transparent to the client.
- The representation of the resource that arrives to the client, will be enough to be able to interact with the external resource, assuming that it has permissions.

**Cacheable**
- On the web, clients can cache server responses
- Answers should be implicitly or explicitly marked as searchable or uncheckable.
- In future requests, the client will know whether or not it can reuse the data it has already obtained.
- Saving requests will improve application scalability and client performance (mainly avoiding latency).

**Client and server separation**
- The client and server are separated, their union is through the uniform interface
- The developments in frontend and backend are done separately, taking into account the API.
- As long as the interface doesn't change, we can change the client or the server without problems.


For all this, it is advisable to follow [good practices in programming REST applications](https://stackoverflow.blog/2020/03/02/best-practices-for-rest-api-design/) for the maintenance and scaling of the platform. 

The architecture of the client-server code will be explained in more detail in section 2 of this document, but here it is important to note that the client is implemented in [Angular 5](https://angular.io/) and the server in [node.js](https://nodejs.org/en/).


<p style="text-align: center;">
	<img width="500px" src="../../_images/Repositories.jpg">
</p>

In order to move from one environment to another you will have to copy the code of one over the new one. Due to the architecture of the Angular projects, in this process it must be taken into account that the environment.prod file in the environments folder of the client has been previously configured for each environment, and therefore it is different in each one of them. Before making any changes you must ensure that it is kept in the environment to be updated and that during the process of copying code from one environment to another the information in this file is not modified. 

But in addition to this, you will have to follow the steps indicated in the following section.
