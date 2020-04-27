<img align="right" width="100px" src="../../_images/Foundation29.png">

## 2.1. Level 1: System Context
(TODO)

There are four user profiles on this platform, so each one will have different associated functionalities:

- User. You can manage your profile (name, language, weight and length units), FAQs, personal information, social information, anthropometry, disease course (datapoints), medical care, medication, clinical trials, phenotype and genotype.  
- Administrator (of each patient group). You can request new language or translations and manage FAQs, request translations. In addition, you can obtain information from your patients (statistics) or send them notifications/alerts. 
- Super Administrator. This is a more technical profile. You can add languages to the platform, manage translations, and manage the different groups of patients (Add symptoms, FAQs, datapoints, medicines). 
- Clinical. You can create patients, and work on the phenotype and genotype.

For Android and iOS apps, we use the cordova framework. When compiling the Angular project, it generates the dist folder, which is the one to be added to the www folder of the Corova project.  We were evaluating ionic, nativescript and reactscript, they would be good options if we were more people, but at the moment we fit more cordova.

Our idea is to migrate to FHIR, so we would have to modify our API to make calls to [FHIR's API](https://www.hl7.org/fhir/ "Title")

Health29 provides a web API to access the data. Anyone can develop an application to access and modify the data of a Health29 user. OAuth 2.0 is used as an authorization protocol to give an API client limited access to the user's data. 