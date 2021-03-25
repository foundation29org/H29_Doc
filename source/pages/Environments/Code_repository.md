<img align="right" width="100px" src="../../_images/Foundation29.png">

## 1.2. Code repositories

The code is open source, and is public on GitHub. 
- [Client](https://github.com/foundation29org/H29_Client) and [Server](https://github.com/foundation29org/H29_Server)

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

For the development and versioning of the code we will follow the GitFlow defined in this [guide](https://blog.axosoft.com/gitflow/).

<p style="text-align: center;">
	<img width="500px" src="../../_images/gitflow.jpg">
</p>

It consists of a division by branches according to the task to be performed. Like this:
>- The master branch will contain the latest version of the code uploaded to production.
>- The develop branch will be used to prepare the next versions. It will be the main branch on which the developers will work.
>- Feature branches will be created to implement the functionality of the tasks defined in each sprint.
>- Hotfixes branches will be created to implement the bugs defined in each Sprint.
>- The releases branch will serve as a link between the development and master branches. It will be the one on which the following versions will be built and added to the production.

Thus, there will be a relationship between each of the branches:
>- The Develop branch is generated from the Master branch. This is part of the latest production version when starting a Sprint.
>- Features branches are created from the Develop branch. A branch will be created for each Sprint Task.
>- From the Master branch the Hotfixes branches are created where the Sprint bugs will be resolved.
>- The releases branch will be the union between Develop and Master, so that to move to Master (new version) you will first have to move from Develop to the releases branch, and then perform a validation phase. When this validation phase is finished, you can move to the Master branch. 





