<img align="right" width="100px" src="../../../source/images/All//Foundation29.png">

# 1. Environments
## 1.1. Build 
By running ng build --prod we minimize and compress the code. This command will create a new folder called dist, and it will have the project optimized. This dist folder will be the one uploaded to the root of the node server that manages the API. It will also be used to create the mobile apps, adding it to the www folder of the cordoba project. 

## 1.2. Deploy environments
Each environment has been created as an independent project, so in order to move from one environment to another you will have to copy the code of one over the new one. 
In this process it must be taken into account that the environment.prod file in the environments folder has been previously configured for each environment, and therefore it is different in each one of them. Before making any changes you must ensure that it is kept in the environment to be updated and that during the process of copying code from one environment to another the information in this file is not modified. 
