<img align="right" width="100px" src="../_images/Foundation29.png">

# 4. Technical debt
There are some aspects that have remained pending during the implementation of the platform. These are both new functionalities and the modification of some implementations that, due to lack of time, we do not consider optimal.

Thus, this section will list the proposals of the technical team or the user feedback gathered to date.
It should be noted that the content of this section is of a high technical level, so it is necessary to understand the architecture of the application at the level of the code in order to follow it.

## 4.1. Modifications requiered
- User has to reload page to be able to be able to login (all times but the first time)
- Create registration such us in this [link](https://docs.microsoft.com/en-us/rest/api/notificationhubs/create-registration) for native android/iOS notifications.
- Alerts are not shown until the site is refreshed
- Think about providing the user with the possibility of snooze notifications "freely". Fields "number" of "hours?/days/months"
- Deploy and test pipelines.
- Translation management.


## 4.2. New items
- Incorporate [FHIR](https://docs.microsoft.com/en-US/azure/healthcare-apis/) for data storage.
