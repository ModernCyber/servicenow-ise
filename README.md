# Cisco ISE App for ServiceNow

The Cisco ISE App for ServiceNow provides a complete, accurate and up-to-date view of your network connected assets integrated with ServiceNow’s CMDB. It helps you automatically true-up your CMDB for all connected IT and IOT assets by continuously sharing device properties and additional network context leveraging Cisco ISE [pxGrid](https://developer.cisco.com/docs/pxgrid/#!what-is-pxgrid). This integration will provide the following additional benefits:
- Allow ServiceNow CMDB users to view ISE Session Directory Information, such as MAC Address, IP Address, Device Type, Compliance Status, Network Device, Location, etc. without the need to access ISE’s GUI.
- Allow ServiceNow CMDB administrators to classify and enrich the asset information in the CMDB providing better service management and monitoring.
- Allow other ServiceNow CMDB integrated products to have better context around assets.

![Cisco ISE App for ServiceNow Context Sharing](/img/share.png)

## Integration Overview
- This Scoped Application leverages ServiceNow’s Integration Hub ETL to aggregate Cisco ISE’s live asset data with existing assets in your CMDB.
- Allows for customization of mapping of your ISE Devices and attributes data to cmdb_ci structure in ServiceNow.
- Define import schedule for data to be synced from ISE into ServiceNow.

## Architecture Diagram
![Cisco ISE App for ServiceNow Architecture](/img/arch.png)


## Dependencies
- [ServiceNow Tokyo Release](https://www.servicenow.com/blogs/2022/welcome-now-platform-tokyo-release.html) or Above
- Cisco ISE [Version 3.2](https://www.cisco.com/c/en/us/support/security/identity-services-engine/products-release-notes-list.html) or Above

### ServiceNow Application Dependencies
- [ITOM Licensing](https://docs.servicenow.com/en-US/bundle/vancouver-it-operations-management/page/product/it-operations-management/reference/itom-su-licensing-landing-page.html)
- [Integration Commons for CMDB](https://docs.servicenow.com/en-US/bundle/store-release-notes/page/release-notes/store/platform-capabilities/store-cmdb-rn-integration-commons.html)
- [IntegrationHub ETL](https://docs.servicenow.com/en-US/bundle/store-release-notes/page/release-notes/store/platform-capabilities/store-integrationhub-rn-etl.html)
- [MID Server](https://docs.servicenow.com/bundle/tokyo-servicenow-platform/page/product/mid-server/concept/mid-server-landing.html)


## Installation & Setup Video
COMING SOON

## Installation Instructions
### 1. Deploy a MID Server (Required) - [Documentation](https://docs.servicenow.com/en-US/bundle/vancouver-servicenow-platform/page/product/mid-server/concept/mid-server-installation.html)

### 2. Import the App via Source Control 
1. Click the Fork button on this page.
2. Select your personal GitHub account as the fork Owner, then click the Create fork button.
3. Create credentials to access your repository. [Example #1](https://snprotips.com/blog/2022/3/4/how-to-change-servicenow-applications-repositoryurl) | [Example #2](https://developer.servicenow.com/dev.do#!/learn/learning-plans/tokyo/new_to_servicenow/BNI_Lab_ForkRepo_tokyo)
4. On your ServiceNow Instance, navigate to System Applications > Studio
5. Select File > Import From Source Control and enter the repository URL, Credential, select master as the branch and click Import.<br>
<center><img src="img/import.png" width="50%"><br>
<img src="img/import-success.png" width="50%"></center>

### 3. Configure Integration Settings. 
1. From your instance homepage, navigate to Cisco ISE App > Integration Settings. (Note: All required navigation for the App is in the Cisco ISE Application Menu<br>
<img src="img/nav-int.png" width="600">
2. Configure the pxGrid Node (FQDN or IP Address)
3. Configure the pxGrid Port (Default is 8910)
4. Configure the pxGrid Account Name (Must be unique from other integrations)
5. Select the MID Server to be used for the integration and enter the MID Server Name.
 <br>
<img src="img/integration-settings.png"  width="1200">
6. Select the ERS Settings Tab - ERS is used to approve the pxGrid account in ISE.
7. Configure the ERS Node (Typically the Primary Administration Node)
8. Configure the ERS Port (Default is 9060)
9. Configure the ERS Username and Password (User should be in the ERS-Admin Group. [More Info](https://developer.cisco.com/docs/identity-services-engine/latest/#!setting-up/cisco-ise))<br>
<img src="img/integration-ers-settings.png" width="1200">
10. Verify your settings and click Update.

### 4. Verify pxGrid Provisioning completes successfully<br>
<img src="img/int-status.png" width="1200">

## Extract, Transform, & Load Configuration
From your instance homepage, navigate to **Cisco ISE App > ETL Configuration**.<br> 
<img src="img/nav-int.png" width="600"><br>

Use the guided setup on the ETL Transform Map Assistant page to customize the integration data mappings to target CMDB classes based on source data. (ServiceNow Documentation: [ Create an ETL transform map](https://docs.servicenow.com/bundle/tokyo-servicenow-platform/page/product/configuration-management/concept/create-etl-transform-map.html))

<img src="img/etl.png" width="1200">

### Step 1. Provide Basic Information for the ETL Transform Map
Select a Sample Import Set or auto-pull a new one to review your data. When finished click **Mark as Complete**.
<img src="img/etl-s1.png" width="1200">

### Step 2. Prepare Source Data for Mapping
Pre-configured transform of ISE Session Directory fields have been provided, but you can customize or cleanse the source data here. When finished click **Mark as Complete**.
<img src="img/etl-s2.png" width="1200">

### Step 3. Map Data to CMDB and Add Relationships
Pre-configured mappings leverage endpoint profile as the source criteria have been provided, but you can customize the conditional class mapping and relationships. When finished click **Mark as Complete**.
<img src="img/etl-s3a.png" width="1200">
Click on **Edit Mappings** to customize the source data fields to CMDB field mappings.
<img src="img/etl-s3b.png" width="1200">
On the Relationship mapping page, configure relationships between CIs based on your ISE Session Data. (E.g. Device to Network Adapter, User to Device)
<img src="img/etl-s3c.png" width="1200">

### Step 4. Preview Sample Integration Results
Click on Test and Rollback Integration Results and then **Run Integration**. 
<img src="img/etl-s4.png" width="1200">

Verify your results and then click **Mark as Complete**.
<img src="img/etl-s4b.png" width="800">

### Step 5. Schedule Import
From the Cisco ISE App menu, click on Import Schedule.<br>
<img src="img/int-sch.png" width=400><br>
Check the box next to **Active** and setup a schedule for running the import of ISE data. The default is set to one hour but should be customized based on the environment of the deployment. <br>
<img src="img/import-schedule.png" width="1200"><br>
**Note**: From the Import Schedule page, **Execute Now** can be used to run the import on-demand.

## Limitations of the App
- This is Proof of Concept Code and should be customized and tested prior to production deployment. Please see [license](/license) disclaimer & details.
- No ISE High Availability Support
- No Multiple ISE Deployments
- No pxGrid Cloud Support

## Production Deployment & Support
For additional design, implementation or support services, please contact [ModernCyber](https://www.moderncyber.com/contact/) to learn about our premium offerings and support.