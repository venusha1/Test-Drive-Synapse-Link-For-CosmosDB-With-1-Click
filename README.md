## Azure Synapse Link For CosmosDB 1-click environment
This 1-click deployment allows the user to deploy a environment of Azure Synapse Analytics ,Synapse Link for CosmosDB,CosmosDB (Analytical Store),Notebook (Sales Forecasting)

## Prerequisites

Owner role (or Contributor roles) for the Azure Subscription the template being deployed in. This is for creation of a separate Resource Group and to delegate roles necessary for this proof of concept. Refer to this [official documentation](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-steps) for RBAC role-assignments.

## Deployment Steps
1. Fork out [this github repository](https://github.com/Azure/Test-Drive-Synapse-Link-For-CosmosDB-With-1-Click) into your github account. 
    
   **If you don't fork repo:** 
   + **Notebook will not be deployed**
   + **You will get a Github publishing error**
   
   
  <!--  ![Fork](https://raw.githubusercontent.com/Azure/Test-Drive-Synapse-Link-For-CosmosDB-With-1-Click/main/images/4.gif) -->
 
2. While in your forked repo,Click 'Deploy To Azure' button given below to deploy all the resources.

    [![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fnashahz%2FTest-Drive-Synapse-Link-For-CosmosDB-With-1-Click%2Fmain%2Fazuredeploy.json)

   - Provide the values for:

     - Resource group (create new)
     - Region
     - Company Tla
     - Option (true or false) for Allow All Connections
     - Option (true or false) for Spark Deployment
     - Spark Node Size (Small, Medium, large) if Spark Deployment is set to true
     - Sql Administrator Login
     - Sql Administrator Login Password
     - Sku
     - Option (true or false) for Metadata Sync
     - Frequency
     - Time Zone
     - Resume Time
     - Pause Time
     - CosmosDB Account Name
     - Throughput Policy
     - Manual Provisioned Throughput
     - Autoscale Max Throughput
     - Analytical Store TTL (Default -1)
     - Github Username (username for the account where [this github repository](https://github.com/Azure/Test-Drive-Synapse-Link-For-CosmosDB-With-1-Click) was forked out into)

   - Click 'Review + Create'.
   - On successful validation, click 'Create'.

## Azure Services being deployed
This template deploys necessary resources to support an Azure Synapse link for CosmosDB which includes following resources along with some RBAC role assignments:

- An Azure Synapse Workspace 
- An Azure Synapse SQL Pool
- An optional Apache Spark Pool
- Azure Data Lake Storage Gen2 account
- A new File System inside the Storage Account to be used by Azure Synapse
- A Logic App to Pause the SQL Pool at defined schedule
- A Logic App to Resume the SQL Pool at defined schedule
- A key vault to store the secrets
- CosmosDB with three containers(Analytical store Enabled)
- AML workspace
- Pyspark Notebook to ingest data into CosmosDB containers, Fetch data from CosmosDB,Join dataset together,Execute Near Real time Sales Forecasting 

<!-- The data pipeline inside the Synapse Workspace gets New York Taxi trip and fare data, joins them and perform aggregations on them to give the final aggregated results. Other resources include datasets, linked services and dataflows. All resources are completely parameterized and all the secrets are stored in the key vault. These secrets are fetched inside the linked services using key vault linked service. The Logic App will check for Active Queries. If there are active queries, it will wait 5 minutes and check again until there are none before pausing -->

## Post Deployment
- Current Azure user needs to have ["Storage Blob Data Contributor" role access](https://docs.microsoft.com/en-us/azure/synapse-analytics/get-started-add-admin#azure-rbac-role-assignments-on-the-workspaces-primary-storage-account) to recently created Azure Data Lake Storage Gen2 account to avoid 403 type permission errors.
- After the deployment is complete, click 'Go to resource group'.
- You'll see all the resources deployed in the resource group.
- Click on the newly deployed Synapse workspace.
- Click on link 'Open' inside the box labelled as 'Open Synapse Studio'.
- Click on 'Log into Github' after workspace is opened. Provide your credentials for the github account holding the forked out repository.
- After logging in into your github account, click on 'Notebook' icon in the left panel. A blade will appear from right side of the screen.
- Make sure that 'main' branch is selected as 'Working branch' and click 'Save'.

![Start Workspace](https://github.com/nashahz/Test-Drive-Synapse-Link-For-CosmosDB-With-1-Click/blob/main/images/Start_Workspace.gif)

#### Configuring Synapse Link for CosmosDB
- Click on the 'Manage' icon in the left panel and navigate to 'Linked Services' menu option.
- Click on 'RetailSalesDemoDB' linked service to open up configuration settings.
- Select 'From Azure subscription' under 'Account selection method'.
- Select Subscription under which the package is deployed.
- Select CosmosDB account
- Select CosmosDB Databasename 'RetailSalesDemoDB'
- Click 'Apply' to save the changes

![Configure Comsos Link](https://github.com/nashahz/Test-Drive-Synapse-Link-For-CosmosDB-With-1-Click/blob/main/images/Configure_CosmosLink.gif)

#### Navigating Synapse Link for CosmosDB
- Open the CosmosDB account, Under the 'Features' tab Synapse Link will appear as Enabled.
- To check the pre created database and containers with Analytical store navigate to 'Data Explorer'.
- Same containers will also be available under Synapse Studio.Click on the 'Data' icon in the left panel and navigate to 'Linked' menu option.
- Expand 'Azure CosmosDB', There will be three containers listed with 'Analytical Store' enabled.

![Navigate Synapse Link](https://github.com/nashahz/Test-Drive-Synapse-Link-For-CosmosDB-With-1-Click/blob/main/images/Navigate_Synapse_Link.gif)

#### Notebook Execution Using Synapse Link for CosmosDB
- Click on the 'Devlop' icon in the left panel and navigate to '1-SalesForecastingWithAML' notebook.
- Follow the instructions in each cell of Notebook to execute:
  - Ingest Data into CosmosDB containers using Synapse Link for CosmosDB
  - Create Spark tables out of CosmosDB using Synapse link for CosmosDB
  - Join spark tables using Synapse spark serverless
  - Execute Machine learning model on this dataset using Azure ML

![Navigate Notebook](https://github.com/nashahz/Test-Drive-Synapse-Link-For-CosmosDB-With-1-Click/blob/main/images/Navigate_Notebook.gif)

#### CosmosDB Populated Containers
- On completion of Notebook CosmosDB containers will be populated with sample dataset.

https://github.com/nashahz/Test-Drive-Synapse-Link-For-CosmosDB-With-1-Click/blob/main/images/CosmosDB_Containers_Data.gif

#### Read and Explore CosmosDB data within Synapse studio Using Synapse Link for CosmosDB
![Read and Explore](https://github.com/nashahz/Test-Drive-Synapse-Link-For-CosmosDB-With-1-Click/blob/main/images/Read_Container.gif)

#### Publishing Changes
- Once published all the resources will now be available in the live mode.
- To switch to the live mode from git mode, click the drop down at top left corner and select 'Switch to live mode'.

![Publish](https://github.com/nashahz/Test-Drive-Synapse-Link-For-CosmosDB-With-1-Click/blob/main/images/Publish.gif)
