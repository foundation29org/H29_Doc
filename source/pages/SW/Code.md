<img align="right" width="100px" src="../../../../source/images/All//Foundation29.png">

## 2.4. Level 4: Code
### 2.4.1. Webapp
All documentation for the Health29 application code is contained in:
- For the [development environment](https://health29-dev.azurewebsites.net/APIDOC/)
- For the [test environment](https://health29-test.azurewebsites.net/APIDOC)
- For the [production environment](https://health29.org/APIDOC/) 

#### 2.4.1.1. Communication

For establish the communication:
- The client will make requests of the type:
```
this.http.get(environment.api+'/api/'+<url>)
        .map( (res : any) => {
            // do something when res OK
         }, (err) => {
           // do something when error
         })
```

- The server will listen to these requests and will perform the appropriate operations to give an answer to the client.
```
api.get(<url>, auth, function)
```

#### 2.4.1.2. Code Structure: Client Structure

*NOTE: A template was purchased to have a base: [Template Link](https://themeforest.net/item/apex-angular-4-bootstrap-admin-template/20774875).*

The structure is as follows:

<p style="text-align: center;">
	<img width="150px" src="../../../../source/images/Architecture/Code/Webapp/client/Client_structure.png">
</p>

- The **src folder** has the following:

	<img align="right" width="150px" src="../../../../source/images/Architecture/Code/Webapp/client/Client_structure_src.png">

	>- app/app.component.{ts,html,css,spec.ts}: Defines the AppComponent along with an HTML template, CSS stylesheet, and a unit test. It is the root component of what will become a tree of nested components as the application evolves. In this file it is controlling the events of inactivity of a session, loading the language of the app depending on the language of the browser, the title that appears in the browser tab with the change of pages. If it is a mobile app, it also controls the backbutton, pause and resume events.
	>- app/app.module.ts: Defines AppModule, the root module that tells Angular how to assemble the application. 
	>- assets/..: It contains all the information that will be accessible from any url. Css files, js, images, language files, jsons listing countries, types of subscriptions, frequently asked questions for each group of patients, etc. It will be the only visible folder when a build is made for production.
	>- environments/..: This folder contains one file for each of your destination environments, each exporting simple configuration variables to use in your application. The files are replaced on-the-fly when you build your app. You might use a different API endpoint for development than you do for production or maybe different analytics tokens. You might even use some mock services. Either way, the CLI has you covered. At the moment url of the api, and fitbit credentials
	>- favicon.ico: Every site wants to look good on the bookmark bar. Get started with your very own Angular icon
	>- index.html: The main HTML page that is served when someone visits your site. Most of the time you'll never need to edit it. The CLI automatically adds all js and css files when building your app so you never need to add any script or link tags here manually. For the mobile version, two small changes must be made in this file:
		Change base: base href="./"
		Add cordova:  "script src="cordova.js"
	>- main.ts: The main entry point for your app. Compiles the application with the JIT compiler and bootstraps the application's root module (AppModule) to run in the browser. You can also use the AOT compiler without changing any code by passing in --aot to ng build or ng serve
	>- polyfills.ts: Different browsers have different levels of support of the web standards. Polyfills help normalize those differences. You should be pretty safe with core-js and zone.js, but be sure to check out the Browser Support guide for more information.
	>- styles.css: Your global styles go here. Most of the time you'll want to have local styles in your components for easier maintenance, but styles that affect all of your app need to be in a central place.
	>- test.ts: This is the main entry point for your unit tests. It has some custom configuration that might be unfamiliar, but it's not something you'll need to edit.
	>- tsconfig.{app|spec}.json: TypeScript compiler configuration for the Angular app (tsconfig.app.json) and for the unit tests (tsconfig.spec.json).


- The **app folder** is the one with all the code:
	>- Layouts: the different layouts that there are, at the moment two subfolders, for the logged ones (full) and for the ones that are not (content).
	>- Pages: It has two subfolders, content-pages (corresponds to the logged out pages like login page, registration, etc) and full-pages (common pages for all logged out roles, like the user-profile page).
	>- admin: contains all pages for the admin role
	>- superadmin: contains all pages for the superadmin role
	>- user: contains all the pages for the user role. 
	Each folder of these roles has a module file (it loads the needed modules), and a route file to manage the routes and control the authentication and authorization (auth-guard and role-guard)

- The **shared folder**, which is the shared code:

	<img align="right" width="150px" src="../../../../source/images/Architecture/Code/Webapp/client/Client_structure_shared.png">

	>- auth: authorization management (role-guard), authentication (auth-service and auth-guard), http interceptor, oauth.services for external services like fitbit.
	>- Configs: configuration files, for example configuration for toasts, or parameters for graphs. 
	>- Customizer: right side slide (not used at the moment)
	>- Directives: correspond to the angle directives
	>- Footer: app footer
	>- Models: data models
	>- Navbar and navbar-nolog: It corresponds to the top bar. Navbar-nolog is for when you are not logged in.
	>- Routes: route management. It has two files, one for the paths of the unlogged pages, and another for the rest. 
	>- Services: corresponds to the angle services. Several have been developed as faq.service or lang.service
	>- Sidebar: side menu, depending on the role, loads one menu or another.  

- **Routes**: In the root of the app folder, there is a file called app-routing.module.ts, which is the root of the routes management. Depending on the path, it loads some subroutes and others. For example, if it is unlogged, it loads the routes ./shared/routes/content-layout.routes, on the other hand, if it is logged in it loads ./shared/routes/full-layout.routes. In this second case, check that it is authenticated (canActivate: [AuthGuard])
The rest of the subroutes that come from full-layout, are controlled if they are logged and authenticated in the routing-module file of each module (each profile has a module) with canActivate: [AuthGuard, RoleGuard].


#### 2.4.1.3. Code Structure: Server Structure

<p style="text-align: center;">
	<img width="150px" src="../../../../source/images/Architecture/Code/Webapp/server/Server_structure.png">
</p>

- index.js: file where the app.js and config.js file is loaded It listens to requests.
- Config.js: configuration file. The server url, port, connection with the data bases are passed to it through environment variables. The credentials of the mail that send email, the secret token of jwt, and the secret of crypto are not passed through environment variables.
- Db_connect.js: will be in charge of creating the connection with the databases.
- App.js: the crossdomain is established, and other node configurations. It manages the requests, and these can be of 3 types: 
	>1. API routes managed by routes folder. 
	>2. Server-generated view paths (handlebars)
	>3. the paths of the Angular client application (dist folder)


#### 2.4.1.4. External libraries and dependencies
All the necessary dependencies or libraries that will be used in the implementation of the application can be checked in the "package.json" files of both the client and the server. The installation of all these libraries and dependencies has been done with [npm](https://docs.npmjs.com/using-npm-packages-in-your-projects), which updates the "Node modules" folder with the necessary packages to work with. Therefore, when you want to work with the projects, it is necessary to execute previously the command "npm install".

On the one hand, among all those found in this file, we can mainly highlight that the versions are being used:
- Both client and server: 
>- v10.16.3 of node.
>- 0.11.4 of botframework-webchat

- For the client: 
>- 1.7.0 of Angular CLI and 5.2.5 of the node modules angle libraries (Angular 5).
>- 2.6.2 of TypeScript. 
>- 1.0.0 of ng-bootstrap
>- Jquery 3.2.1
 
- For the server:
>- 1.12.1 of nodemon
>- 4.16.2 of express and 3.0.0 of express-handlebars
>- 4.13.1 moongose
>- 2.6.0 of async


On the other hand, specific NPM libraries have been installed for some functionalities:
- For the client:
>- For the HTTP requests, the version 1.0.0-beta.2 of the library [@auth0/angular-jwt](https://www.npmjs.com/package/@auth0/angular-jwt) is used
>- For graphic representations [d3](https://www.npmjs.com/package/d3) has been installed. Version 5.7.1 of @types/d3 and 1.2.2 of @types/d3-shape
>- Fingerprint2 version 2.0.0 (https://www.npmjs.com/package/fingerprintjs2) is used to obtain the information of the device being used to access the platform.
>- Version 0.24.1 of [angular-calendar](https://www.npmjs.com/package/angular-calendar) is used to work with the date entries of the platform.
>- For data encryption, version 0.7.1 of [js-sha512](https://www.npmjs.com/package/js-sha512) is used and for the reverse process, version 2.2.0 of [jwt-decode](https://www.npmjs.com/package/jwt-decode) is used.
>- Version 7.12.9 of [sweetalert2](https://www.npmjs.com/package/sweetalert2) is used for platform popups.
- For the server
>- For emailing tasks: 4.4.0 nodemailer and 2.0.0 nodemailer-express-handlebars.
>- For communication with Azure: 0.10.6 of azure-sb
>- To use the Authy application as 2FA: 1.4.0 of authy and 1.1.4 of authy-client
>- For data encryption, version 0.7.0 of [js-sha512](https://www.npmjs.com/package/js-sha512) is used

In addition to this, Javascript scripts and JSON files have been added as libraries to optimize the programming of different functionalities for the client:
- Javascript scripts:
>- [Jquery countdown](http://hilios.github.io/jQuery.countdown/)
>- [PDF JS](https://www.npmjs.com/package/pdfjs)
>- [Azure storage](https://www.npmjs.com/package/@azure/storage-blob)
- JSON Files located in src/assets/jsons:
>- Some JSON files for managing the users' location information (country, province, cities and phone codes): Countries folder, cities.json, countries.json, countries_nl.json and phone_codes.json
>- Some JSON files for obtain and manage the symptons: genes_to_phenotype.json, orpha-omim-orpha.json, orphaids.json and phenotypes.json.
>- A JSON with the languages available in Azure Cognitive Service for translation tasks: cognitive-services-languages.json
>- A JSON for the management of languages available on the platform: all-languages.json
>- A JSON for the types of subscription on the platform: subscription-types.json

### 2.4.2. External APIs
**API Foundation29** has been designed to act as an intermediary between the webapp and Azure Qnamaker's service.
The functions that have been implemented to perform actions on the Azure service are analogous to those in the [Azure documentation](https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/quickstarts/quickstart-rest-curl).

It can be consulted at the following link: [API Foundation29](https://f29api.northeurope.cloudapp.azure.com/index.html).
And the functionality and methodology of use is described in the qnamaker section of this document (2.5.3.2. Qna maker)

Some functions to use the monarch API from this have also been included in this API. But this is not being used at the moment in health29 and the calls to the **monarch API** are executed directly, without intermediaries.

- Request for list of related conditions
```
request({
url: https://api.monarchinitiative.org/api/sim/search?is_feature_set=true&metric=phenodigm&id=HP:0001250&id=HP:0002133&limit=100&taxon=9606’,
json: true
}
```
- To open a new link for information on a symptom
```
href="https://monarchinitiative.org/phenotype/'+res[j].id+'
```

- To obtain information about a disease:
```
href=”https://monarchinitiative.org/disease/{{value.id}}#overview 
```

And with this API, the configuration of the headers (auth.interceptor.ts) to establish the connection with Azure's service are sent are like:
```
if(req.url.indexOf('https://api.monarchinitiative.org/api/')!==-1){
  isExternalReq = true;
}
```

### 2.4.3. Azure cognitive services
#### 2.4.3.1. Computer Vision
To create it from azure you just have to select the cognitive service in the marketplace: "ComputerVision".
The configuration has no complexity, just select the Price tier and the resource group.

It is used in the webapp client, as it has been indicated until now, establishing a REST communication:
```
this.subscription.add( this.http.post('https://westeurope.api.cognitive.microsoft.com/vision/v1.0/ocr?language='+this.cognitiveServicesLanguage +'&detectOrientation=true', data)
    .subscribe( (res : any) => {
    	// Do something when result OK
	}, (err) => {
     // Do something when error
}));
```
And the configuration of the headers (auth.interceptor.ts) to use this service is:
```
if(req.url.indexOf('api.cognitive.microsoft.com/vision')!==-1){
  isExternalReq = true;
  const headers = new HttpHeaders({
    'Ocp-Apim-Subscription-Key': '992ab4c1c5ff4412a052b0d170feeab8',
    'Content-Type': 'application/octet-stream'
  });
  authReq = req.clone({ headers});//'Content-Type',  'application/json'
}
```
As it has been said before this is used for the symptom extraction, so the call is made in phenotypes.component.ts.

All the commands and settings for establishing this communication are described in the [Microsoft documentation](https://docs.microsoft.com/bs-latn-ba/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtocallvisionapi).

#### 2.4.3.2. Qna maker
To create it from azure you just have to select the cognitive service in the marketplace: "Qna maker".
The configuration has no complexity, just:
- Enter a name
- Select subscription of Azure, the Price tier, the resource group adn the localization.
- Enter an application name for establish the url: "app_name.azurewebsites.net"

As previously stated, in Health29 it is used to manage the FAQs in different ways and from different points of the platform. To work with the data of this azure service, an external API will be used as an intermediary: f29API, described in the previous section, so that all calls will be made to it. However, the same format has been used as if the calls were made directly to Azure's service, as an example:
```
this.http.get('https://f29api.northeurope.cloudapp.azure.com/api/knowledgebases/'+knowledgeBaseID+'/Prod/qna')
    .map( (res : any) => {
      // Do something when get the Knoledbase information
	}, (err) => {
     // Do something when error
}));
```
And with the configuration of the headers (auth.interceptor.ts) to use this service the same thing happens, the ones that would be necessary to establish the connection with Azure's service are sent:
```
if(req.url.indexOf('https://f29api.northeurope.cloudapp.azure.com')!==-1){
  isExternalReq = true;
  const headers = new HttpHeaders({
    'Ocp-Apim-Subscription-Key': '8d7bca89da4e42c1bf79b292207c9635'
  });
  authReq = req.clone({ headers});
}
```
 
All the commands and settings for establishing this communication are described in the [Microsoft documentation](https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/quickstarts/quickstart-rest-curl).

#### 2.4.3.3. Translator
To create it from azure you just have to select the cognitive service in the marketplace: "Translator text".
The configuration has no complexity, just select the Price tier and the resource group.

It is used in the webapp client, as it has been indicated until now, establishing a REST communication:
- First case: communication without authorization required.
```
this.subscription.add( this.http.post('https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to='+this.authService.getLang(), jsonText)
    .subscribe( (res : any) => {
    	// Do something when it returns the text from jsonText translated
    }, (err) => {
       // Do something when error
    }));
```
- Second case: communication with authorization required.

```
this.subscription.add( this.http.post('https://api.cognitive.microsoft.com/sts/v1.0/issueToken','')
    .subscribe( (res : any) => {
      	// get the token for establish communication between webapp and text translator
        sessionStorage.setItem('tokenMicrosoftTranslator', res);
	}, (err) => {
	 	// Do something when error
	}));
```
```
this.subscription.add( this.http.get('https://api.microsofttranslator.com/V2/Http.svc/Translate?appid=Bearer' + ' ' + sessionStorage.getItem('tokenMicrosoftTranslator')+'&text='+<text>+'&to=en&Authorization=Bearer' + ' ' + sessionStorage.getItem('tokenMicrosoftTranslator'))
	.subscribe( (res : any) => {
		// Do something when it returns the text from <text> translated
	}, (err) => {
	 	// Do something when error
	}));
```


And the configuration of the headers (auth.interceptor.ts) to use this service is:
```
if(req.url.indexOf('https://api.cognitive.microsoft.com/sts/v1.0/issueToken')!==-1){
  isExternalReq = true;
  authReq = req.clone({ headers: req.headers.set('Ocp-Apim-Subscription-Key',  '7174b790ef59409280f77dd94c34a9d2' ), responseType: 'text'});
}

if(req.url.indexOf('https://api.microsofttranslator.com')!==-1){
  isExternalReq = true;
  authReq = req.clone({ responseType: 'text' });
}
if(req.url.indexOf('https://api.cognitive.microsofttranslator.com')!==-1){
  isExternalReq = true;
  authReq = req.clone({ headers: req.headers.set('Ocp-Apim-Subscription-Key',  '7174b790ef59409280f77dd94c34a9d2' ) });
}
```

The first of these is used to translate the datapoints and to add a new language to the platform, while the second is used in the symptom section (phenotype) to translate into English the text obtained from the vision service.

All the commands and settings for establishing this communication are described in the [Microsoft documentation](https://docs.microsoft.com/bs-cyrl-ba/azure/cognitive-services/translator/).

### 2.4.4. Azure Healthbot
To create it from azure you just have to select the cognitive service in the marketplace: "Healthcare Bot".
To create and configure it, just follow the steps in the [Microsoft guide](https://docs.microsoft.com/en-us/healthbot/quickstart-createyourhealthcarebot).

In the case of Health29, the App service [server](https://healthbotcontainersamplef666.scm.azurewebsites.net/dev/wwwroot/server.js) has been configured to be able to deploy the three bots depending on the webapp client that is running it. So that:
```
var WEBCHAT_SECRET = process.env.WEBCHAT_SECRET;
var APP_SECRET = process.env.APP_SECRET;
if(req.headers.origin == 'https://health29.org'){
    
}else if(req.headers.origin == 'https://health29-test.azurewebsites.net'){
    WEBCHAT_SECRET = process.env.WEBCHAT_SECRET_TEST;
    APP_SECRET = process.env.APP_SECRET_TEST;
}else if(req.headers.origin == 'http://localhost:4200' || req.headers.origin =='https://health29-dev.azurewebsites.net'){
    WEBCHAT_SECRET = process.env.WEBCHAT_SECRET_DEV;
    APP_SECRET = process.env.APP_SECRET_DEV;
}
```
It should be noted that this service has a monthly message limit. Depending on the rate chosen in each of the Healthbots, a greater or lesser flow of information exchange or interactions of the Health29 assistant will be allowed. 

*NOTE: if at any time the limit is exceeded, the bot will return error 429 (Too many request).*

The incorporation of this assistant to the webapp is done from the client, in particular, from the customizer component.
This way, it is added to the HTML:
```
<div id="botContainer" style="width:95%"></div>
```
And the style is added from the scss file.

As for the functionality of the bot, this is included in the ".ts" file, taking into account:

<p style="text-align: center;">
	<img width="800px" src="../../../../source/images/Architecture/Code/Healthbot/botflow.png">
</p>

Health Bot uses [Bot Framework](https://dev.botframework.com/) under the hood as a messaging and routing platform to deliver messages to and from the end user.

So, the steps to follow are:

1. Get the token for the connection with the azure service:
```
var paramssend = { userName: this.user.userName, userId: this.authService.getIdUser(), token: this.authService.getToken(), groupId: this.groupId, lang: this.user.lang, knowledgeBaseID: this.knowledgeBaseID}; 
this.subscription.add( this.http.get('https://healthbotcontainersamplef666.azurewebsites.net:443/chatBot',{params: paramssend})
	.subscribe( (res : any) => {
		// Get the token OK
	}, (err) => {
		// Get the token Error
}));

```
2. Create a bot connection:
```
const jsonWebToken = token;
const tokenPayload = JSON.parse(atob(jsonWebToken.split('.')[1]));
const user = {
  id: tokenPayload.userId,
  name: tokenPayload.userName
};
const botConnection = new BotChat.DirectLine({
  //secret: botSecret,
  token: tokenPayload.connectorToken,
  //domain: "",
  webSocket: true
});
```
3. Start conversation
```
const botContainer = document.getElementById('botContainer');
$('#botContainer').empty();
botContainer.classList.add("wc-display");   
BotChat.App({
    botConnection: botConnection,
    user: user,
    locale: this.user.lang,
    bot: {id: ''},//id: 'h29bot-giochop' //id: 'zebrahealthbot'
    resize: 'detect'
    // sendTyping: true,    // defaults to false. set to true to send 'typing' activities to bot (and other users) when user is typing
}, botContainer);
```
4. A communication will be established with it to exchange information and perform the relevant actions:
```
this.subscription.add( botConnection.postActivity({type: "event", value: jsonWebToken, from: user, name: "InitAuthenticatedConversation"}).subscribe(function (id) {
     // Start scenarios
}, (err) => {
		// Do something when error
}));
```
All the commands and settings for using this Healthbot service ara available in [Microsoft documentation](https://docs.microsoft.com/en-us/healthbot/integrations/embed).

In addition to this, the HTTP headers have to be configured to be able to use this service in the auth.interceptor.ts file:

```
if(req.url.indexOf('healthbot')!==-1){
  console.log('epasa');
  isExternalReq = true;
  const headers = new HttpHeaders({
    'Content-Type':  'text/html; charset=utf-8',
    'Access-Control-Allow-Methods': 'GET'
  });
  authReq = req.clone({ headers, responseType: 'text'});//'Content-Type',  'application/json'
}
```

As already indicated, once the bot is embedded in the webapp you will have to indicate it to initialize the desired scenarios in each case. These scenarios are designed, implemented and configured in:

- [Healthbot](https://us.healthbot.microsoft.com/account/zebrahealthbot/scenarios/manage) of the development environment.
- [Healthbot](https://eu.healthbot.microsoft.com/account/healthbot-test-tjo3h51/scenarios/manage) of the test environment.
- [Healthbot](https://eu.healthbot.microsoft.com/account/h29bot-giochop/scenarios/manage) of the production environment.

In the microsoft documentation there are [guides](https://docs.microsoft.com/en-us/healthbot/quickstart-createyourfirstscenario) to help you create these scenarios.

The general workflow of the Healthbot scenarios defined for Health29 is

<p style="text-align: center;">
	<img width="800px" src="../../../../source/images/Architecture/Code/Healthbot/Scenarios.jpg">
</p>

Except for the control scenarios that do not send messages to the user, the rest are replicated: one is created per user language available in Health29 (as can be seen in the previous diagram, the nomenclature "scenarioName_lang" has been used). 
In this way, the scenarios that will be executed depend on the language selected by the user on the Health29 platform.

A modification of this was started to use the Healthbot localization service to avoid scenario replications. However, there is still work to be done since this translation could not be managed for some messages sent by the bot to notify of problems or errors. This has been solved so far from the Health29 client code. 
For example:

```
 if((activity.text ==="I didn't understand. Please choose an option from the list.")){
  var x = document.getElementsByClassName("format-markdown");
  for (var element=0;element<x.length;element++){
    var stringElement=x[element].firstElementChild.childNodes[0].textContent;
    if((stringElement =="I didn’t understand. Please choose an option from the list.")||
    (stringElement=="No se pudo reconocer su respuesta. Por favor seleccione una opción de la lista.")){
      var tempElement=x[element].firstElementChild.childNodes[0];
      tempElement.textContent = this.translate.instant("generics.Understand");
      $(x[element].firstElementChild.childNodes[0]).replaceWith(tempElement.textContent);
    }
  }
```
In this case, the messages shown by the bot are read and translated using the translation system used in Health29 that is explained in the "Multilanguage" section of this document.

Finally, it should also be noted that an action flow has been defined for the task of closing or minimizing the platform's assistant, taking into account that
- This will be displayed whenever a user logs in.
- The user will be given the option to end the conversation, so previous messages will be deleted and the wizard will be minimized.
- The behavior of the buttons to maximize/minimize and open/close the assistant in Health29 is maintained.

### 2.4.5. Azure blobs
To create it from azure you just have to select the cognitive service in the marketplace: "Storage account".
The configuration has no complexity, 
- Fill project details: subscription and resource group
- Fill instance details: 
>- Storage account name
>- Localization
>- Performance (default standard)
>- Account kind (blobstorage)
>- Replication
>- Access tier (hot)

It is used in the webapp client establishing a REST communication. 
In this project, the Javascript library Azure storage is used. In this way, some services have been created in the client project of Angular that will allow working with the reading and writing functions of the Azure blob. 
Thus, the following services have been defined:
- blob-storage-medical-care.service.ts
- blob-storage-support.service.ts
- blob-storage.service.ts

In all cases, an interface to work with blobs like this will be exported:

```
export interface IBlobAccessToken {
  blobAccountUrl: string;
  sasToken: string;
  containerName: string;
  patientId: string; (optional)
}
```

and a class will be defined with the possible functions that can be performed with the blob:

```
@Injectable()
export class BlobStorage<name>{...}
```

With this, from the different sections of Health29 it will be possible to create the containers in the blob and store the relevant information.
An example of use would be:

```
constructor ... (...,private blob: BlobStorageMedicalCareService,...)
...
this.blob.uploadToBlobStorage(this.accessToken, <files>, filename, index1, index2);
```

In addition to this, the HTTP headers have to be configured to be able to use this service in the auth.interceptor.ts file:

```
 if(req.url.indexOf('https://blobgenomics.blob.core.windows.net/')!==-1){
  isExternalReq = true;
  const headers = new HttpHeaders({
    'Access-Control-Allow-Origin':'*',
    'Ocp-Apim-Subscription-Key': 'lXaW8+GnmQuHYVku3GWEjZnRhi9hv5u7v2kGvRiUQR6/PTlJuIZT+hyf+nUgLGTSpIToheyZ7oXyX34+q3s63g=='
  });
  authReq = req.clone({ headers});//'Content-Type',  'application/json'
}

if(req.url.indexOf('https://health29support.blob.core.windows.net/')!==-1){
  isExternalReq = true;
  const headers = new HttpHeaders({
    'Access-Control-Allow-Origin':'*',
    'Ocp-Apim-Subscription-Key': 'g/KX0iFgXuPQMHDDLWYDXJDihvPTMv9/Uyp2lsWGu81azji7i0oPh21R3vjnn1oDGIHjsYsRIEEuR5F8PFrbAw=='
  });
  authReq = req.clone({ headers});//'Content-Type',  'application/json'
}

```
### 2.4.6. Other services
The **[DiagnosisApi](https://portal.azure.com/#@foundation29outlook.onmicrosoft.com/resource/subscriptions/53348303-e009-4241-9ac7-a8e4465ece27/resourceGroups/phenotypeBot/providers/Microsoft.Web/sites/DiagnosisApi/appServices)** is an App Service of Azure that is used for consulting the symptons of a diagnose.
To create it from azure you must create an [App service of Azure](https://docs.microsoft.com/en-US/azure/app-service/).

It is used in the webapp client establishing a REST communication:
```

let httpParams = new HttpParams();
hposStrins.forEach(id => {
  httpParams = httpParams.append('symtomCodes', id);
});
[...]
this.subscription.add( this.http.get('https://diagnosisapi.azurewebsites.net/api/Consulting/GetSymptomsFromCodes',{params: httpParams})
  .subscribe( (res : any) => {
  	 // Get the symptoms codes OK
  }, (err) => {
	// Do something when error
}));
```
And the configuration of the headers (auth.interceptor.ts) to use this service is:
```
if(req.url.indexOf('https://diagnosisapi.azurewebsites.net/api/Consulting')!==-1){
  isExternalReq = true;
  const headers = new HttpHeaders({
    'Access-Control-Allow-Origin':'*'
  });
  authReq = req.clone({ headers});//'Content-Type',  'application/json'
}
```

In the same way, we can access and use the **Genomics Functions apps**.
It is used in the webapp client establishing a REST communication:
```
this.subscription.add( this.http.post('https://genomicservices.azurewebsites.net/api/exomize?code=p/aGykMRV8KiTfo8AXO8yDjHYdImgRjjasrGu8eaBVn7U75jf1DtmQ==',jsonfile)
      .subscribe( (res : any) => {
  .subscribe( (res : any) => {
  	// Result of exomize function OK
  }, (err) => {
	// Do something when error
}));
```

And the configuration of the headers (auth.interceptor.ts) to use this functions is:
```
if(req.url.indexOf('https://genomicservices.azurewebsites.net/api/exomize')!==-1){
  isExternalReq = true;
  const headers = new HttpHeaders({
    'Access-Control-Allow-Origin':'*'
  });
  authReq = req.clone({ headers});//'Content-Type',  'application/json'
}
```

### 2.4.7. Databases
************************TODO:
