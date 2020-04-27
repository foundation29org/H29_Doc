<img align="right" width="100px" src="../../_images/Foundation29.png">

## 2.1. Level 1: System Context
(TODO)

For Android and iOS apps, we use the cordova framework. When compiling the Angular project, it generates the dist folder, which is the one to be added to the www folder of the Corova project.  We were evaluating ionic, nativescript and reactscript, they would be good options if we were more people, but at the moment we fit more cordova.

Our idea is to migrate to FHIR, so we would have to modify our API to make calls to [FHIR's API](https://www.hl7.org/fhir/ "Title")

Health29 provides a web API to access the data. Anyone can develop an application to access and modify the data of a Health29 user. OAuth 2.0 is used as an authorization protocol to give an API client limited access to the user's data. 