# Oracle Integration

<p align="center">
  <img src="https://github.com/GaryHostt/Oracle_Integration/blob/master/screenshots/1.png?raw=true" alt="OIC"/>
</p>

## Introduction

This guide acts as a repository for getting started with Oracle Integration and where to find relevant resources.

Oracle Integration, OIC, is a fully managed service offered by Oracle, ranked as a [leader in application integration by Gartner](https://www.informatica.com/ipaas-magic-quadrant.html). OIC offers application integration, typically triggered by an enterprise's business events. We have [many successful customers](https://blogs.oracle.com/integration/oracle-integration-customer-success-stories) that have digitally transformed their businesses. 

OIC substantially differs from Oracle's [ODI](https://github.com/GaryHostt/Oracle_Data_Integrator), [EDQ](https://www.oracle.com/webfolder/technetwork/tutorials/tutorial/edq/pdf/edq-12.2.1-getting-started-lab/getting-started-with-edq.pdf), and [GoldenGate](https://github.com/GaryHostt/GoldenGate2ADB) offerings. The latter 3 offer data integration - moving larger quantities of data, typically to a data warehouse, for analysis. OIC can eliminate the pain points caused by [point-to-point integrations](https://www.christiernan.com/why-point-to-point-integrations-are-evil/). If this is your first time hearing about OIC, check out the [OIC Product homepage](https://www.oracle.com/middleware/application-integration/).

If you do not have an OIC environment, you can start with this [tutorial](https://www.oracle.com/webfolder/s/assets/demo/integration-simulator/index.html#step1). You can follow [these instructions](https://docs.oracle.com/en/cloud/paas/integration-cloud/integration-cloud-auton/create-oracle-integration-instance.html#GUID-EC63C933-34BF-4B13-94F7-E2979A1668DB) so spin up an instance. Once you have OIC spun up, start with these [beginner tutorials](https://docs.oracle.com/en/cloud/paas/integration-cloud/tutorials.html).

## Outline

1. General Resources
2. Components of OIC
3. **Basic types of integrations - with workshop examples**
4. Treatise: developing in OIC
5. Other resources
6. Youtube sources
7. Additional Workshops
8. Advanced use cases
9. Error Handling
10. Administering OIC
11. Beyond Application Integration

## General resources

[The main OIC documentation](https://docs.oracle.com/en/cloud/paas/integration-cloud/index.html) is your friend.

Do I need the on-premises adapters, what is process automation, how much does it cost? Check out [OIC Pricing & editions](https://www.oracle.com/cloud/integration/pricing.html), it's based on how many 'messages' you consume. But what is a message and how do you count them? Check out this [explanation](https://blog.rubiconred.com/understanding-oracle-integration-cloud-licensing/).
  
Perhaps you want to programatically schedule OIC, or even have OIC integrate with itself? Then check out the [OIC REST API documentation](https://docs.oracle.com/en/cloud/paas/integration-cloud/rest-api/index.html). What credentials do I need to connect, how do I configure SaaS outbound messaging, what can an adapter do? If these questions sound familiar, start with [the adapter documentation](https://docs.oracle.com/en/cloud/paas/integration-cloud/find-adapters.html).

The [Integration blog](https://blogs.oracle.com/integration/) is where new features for OIC are announced and very helpful articles. If you see a new feature you want to try - here's how you can [request a feature flag](https://blogs.oracle.com/integration/enabling-the-future-today-feature-flags-in-oracle-integration-cloud).

From the [marketplace](https://cloudmarketplace.oracle.com/marketplace/en_US/listing/48917996) you can download pre-made integration recipes from here and customize them to your use case. Need to start/stop your instance, scale for more message packs, or export your integrations? Check out how to [manage Oracle Integration](https://docs.oracle.com/en/cloud/paas/integration-cloud/integration-cloud-auton/manage-oracle-integration.html).

## Components of OIC

1. Application integration
2. VBCS, Visual Builder Cloud Service
3. [Insight - coming soon!](https://docs.oracle.com/en/cloud/paas/integration-cloud/user-int-insight/introduction-integration-insight.html)
4. The agent

....................
note: Enterprise edition only below
....................

5. Process Automation
6. On-premises Entrise Application adapters 
7. [B2B](https://docs.oracle.com/en/cloud/paas/integration-cloud/btob.html)

## Basic types of integrations - with workshop examples

1. SaaS triggers
2. Scheduled
3. Generic REST trigger

There are more than these, but these have been the typical use case patterns I have seen.

### SaaS triggers

SaaS trigger integrations typically rely upon these factors:
1. Use of one of the SaaS adapters in trigger or trigger & invoke mode
2. Configuration on the SaaS to fire off an outbound message or a webhook to OIC
3. An event then occuring in the SaaS application that will trigger communication with OIC

Using Eloqua as a trigger first requires [installing this agent and linking it to your OIC environment](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCAA/Help/Apps/IntegrationCloudService/Tasks/InstallingICS.htm). After installing the agent, you then need to create the rule in Eloqua that triggers the outbound call to OIC by [adding a task to a campaign canvas](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-user/Help/Apps/IntegrationCloudService/Tasks/AddingICSToCanvas.htm).

[**This workshop**](https://github.com/GaryHostt/OIC_SaaS_integration) shows how to configure Fusion/Cloud ERP to perform outbound communication with OIC. In it, a product created in Product Data Hub triggers an integration to then add the data to CPQ. It will also get you started with Process Automation.

This video shows you how to start [configuring salesforce for business events](https://www.youtube.com/watch?v=5Pq-Dme5Gvc&feature=share) as integration triggers. This article explains [configuring Service Cloud outbound communication](https://redthunder.blog/2019/04/15/oracle-service-cloud-to-eloqua-contact-create-update-using-oic/) to Eloqua.

### Scheduled

Schedule integrations are good for sending data from applications to data warehouses, reading information from csv files, or pulling information from applications and writing that data to a csv file. 

[**This workshop**](https://github.com/GaryHostt/BigQueryIntegration) is an example of pulling info from an FTP server and writing them to Google's data warehouse, BigQuery, via its REST API. However, for files larger than that and less than 1 GB - you will need the 'stage file' operation in order to read the file - as shown in [this YouTube video](https://www.youtube.com/watch?v=hE5nF-henFw&feature=share). Besides reading files, you can [write them to an FTP after invoking data (seen in this video)](https://www.youtube.com/watch?v=7wx-gAT4IdU), and [list files - even from multiple directories](https://www.techsupper.com/2019/05/how-to-list-files-from-ftp-resides-in-multiple-directories-oracle-integration-cloud.html). 

OIC can process flat files up to 1 GB in size. For larger files than that - [check out my data integration repo](https://github.com/GaryHostt/Oracle_Data_Integrator). [What if errors occur in my scheduled integration?](https://github.com/GaryHostt/Oracle_Integration/blob/master/Errors.md)

### Generic REST endpoint

The next use case pattern is basically using OIC as a 'drag & drop API builder'. You can use a blank REST adapter at the beginning of your integrations. This can also be used to abstract away SOAP endpoints. These endpoints can also invoked from Postman, SOAP UI, VBCS, your given front end framework of choice, coding language API call, etc. 

[**In this workshop**](https://github.com/GaryHostt/ATPworkshop), we basically use OIC to create a REST API for an Autonomous Transaction Processing database. [Click here](http://media.licdn.com/embeds/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2F-9nP2LaeOok%3Ffeature%3Doembed&amp;url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3D-9nP2LaeOok&amp;type=text%2Fhtml&amp;schema=youtube) for a video explanation by a colleague & I. It will also show you how to get started using VBCS.

The generic REST trigger can also be used to create SaaS trigger integrations with [webhooks](https://www.youtube.com/watch?v=rUaDIH5ZXB8). Applications need to be configurable to send payloads to a generic REST endpoint. For example, Agile PLM, an Oracle application lacking its own adapter, can be configured to make an outbound call to a generic API as a result of event updates. To create asynchronous communication, you can choose not to configure the REST adapter to receive a response from your other systems.

The generic REST endpoint can also be the beginning of creating an omni-channel communication device, like [Wuphf](https://www.youtube.com/watch?v=GT6uWYqJq6E)!

## Treatise: developing in OIC

### Mapping 
I want to explain how to go about developing for an integration, such as is in [this lab](https://github.com/GaryHostt/Fusion_PurchaseOrder_Integration). How does one know what fields to have in the mapper when you have a generic endpoint going to a SaaS API?

When trying to invoke a SaaS endpoint - perhaps with a generic REST trigger, you may find that you are inundated with hundreds of fields in the mapper you do not know. Typically, beginning by searching [the documentation is a good start](https://docs.oracle.com/en/cloud/saas/procurement/19a/oaprc/manage-purchase-orders.html#OAPRC1007407). Looking at the 'Defaults:' we can find the fields required for the lines, schedules, and distributions. If you are calling a SOAP API and simply want to abstract it away as a REST API - simply make the request payload have the same fields that the SOAP API has and then press the recommend button - it should work for the fields if they are about a 1:1 ratio and have identical names.

Then, to understand the data types for the fields - or see how they're formatted, I'd call the endpoint to give me one or all of the given business object I'm trying to create. In this case, I can look at the [get all POs or get 1 PO endpoints](https://docs.oracle.com/en/cloud/saas/procurement/19d/fapra/api-purchase-orders.html). The responses from calling the API directly show me how it expects values to be for the given fields. I can then use this information when sending in field/value pairs in the request payload.

Typically for development purposes, I would start this integration with a generic REST adapter that has ~5 fields that map to important fields for the SaaS. After filling out the important ones like POHeaderID, ItemNumber, etc. I activate the integration, then send a payload. Typically, the response will be a 500 error and the response will then contain the field that still requires an attribute. Next, deactivate the integration, map another field from the REST adapter to the business field and then from Postman you can dynamically try different values for that field. Once you find a working value - you can leave it in the submission to the generic REST adapter, or just hardcode it in the adapter. 

### If your integration is not appearing in tracking after performing the trigger action in SaaS

If you create a contact in Fusion, or a new lead in salesforce, but the business event isn't appearing in tracking - you need to verify that you properly configured the SaaS. You should also check if you are logged in as the user on the SaaS that you used to create the connection on OIC.

## Other Resources

[Adapter certifications](https://www.oracle.com/technetwork/middleware/adapters/documentation/adaptercertificationmatrix0217-3613709.pdf)

This is where I go to examine if a given SaaS version is compatible with the adapter.

[Ankur Jain's blog](https://blogs.oracle.com/author/ankur-jain)

[Shalindra's blog](https://shalindrasingh.wordpress.com/)

[RedThunder blog](https://redthunder.blog/)
    
[Oracle Integration Certification](https://learn.oracle.com/ords/training/dl4_pages.getpage?page=dl4learningpath&get_params=offering:37194,id:57591)

  [Certification Study Guide](https://www.techsupper.com/2020/02/oracle-cloud-platform-application-integration-2019-associate.html)

## Youtube

[Santa Monica Hub Youtube channel](https://www.youtube.com/channel/UCW04sPyVsthkrjPs_Gx-dFA/featured?disable_polymer=1)

This channel can get your started with VBCS, Process Automation, RPA, and more! 

[Oracle Cloud TechSupper Ankur](https://www.youtube.com/channel/UCo3xxBD5yqhofSyA_M6C10Q)

[Invoking Fusion's API](https://www.youtube.com/watch?v=6j_Qi2qpmKM)

[Using the REST adapter](https://www.youtube.com/watch?v=NDwTCKDC8LM)

[Navigating OIC](https://www.youtube.com/watch?v=U51IZ7og1Zw)

[Process Automation Introduction](https://www.youtube.com/watch?v=hVIi06pNJ20)

## Additional Workshops

The 4 workshops below are excellent for getting started with various components of OIC, they seek to minimize reliance on having pre-existing external systems with which to integrate. 

[Integration workshop with Process](https://github.com/OracleCPS/oracleintegrationday)

The above workshop, labs 100 & 300 contains great starters for basic Application Integration while seeing a variety of its features. Lab 400 has a great introduction for Process Automation that builds on the previous work. The next two workshops below expand on the use case while showing VBCS and integration with other systems. These three workshops are good starters to learn about the components of OIC and how to leverage them with more advanced use cases.

[VBCS, Process, REST](https://github.com/OracleCPS/aiconlineshopping)

This workshop builds upon the previous, adding VBCS & APIPCS to the previous work.

[Basics, database, REST, Process](https://github.com/oracle/learning-library/blob/master/ospa-library/appint/ApplicationIntegration-labguide.md)

The above workshop is excellent at continuing the previous use cases you've learned in process, but with more complex business logic, and more personas. It also shows you how to connect with an Oracle Autonomous Transaction Process database.

[Getting started with VBCS](https://github.com/OracleCPS/Visual-Builder-Cloud-Service_VBCS), with [video](https://www.youtube.com/watch?v=zPNCj4K0jSM)

The above is the best workshop for **solely** getting started with VBCS, you'll create a functioning app with multiple pages that displays information about countries.

.......

[HCM to EBS](https://github.com/KseniiaRyuma/HCM_to_EBS_integration/blob/master/oic100.md), with [video](https://www.youtube.com/watch?v=8YuxMwG8qKE&fbclid=IwAR0vTgkqxEJQyEGbw9S5ErjNWJSoH1UphbdLgR2-pT3N_NU2WgusA7gFzQo)

This lab captures new employees from HCM and creates them in EBS.

[OIC Fusion & HCM Integration](https://github.com/OracleCPS/oicsaasintegration)

This shows how to use HCM ATOM feeds.

[Fusion ERP events, FBDI, Extracts, BIP](https://antonyjr.github.io/Hands-On-Labs/ERP-Integration-Patterns/html/index.html)

This workshop contains the common ERP Cloud integration patterns with the OIC adapter. The OIC adapter is capable of interacting with Fusion via: BIP, FBDI, extracts, and events. 

[The varieties of Fusion ERP integrations + Apiary + salesforce](https://github.com/GaryHostt/Fusion_PurchaseOrder_Integration/blob/master/README.md)

The above workshop shows you the various ways to use the Cloud ERP adapter, how Apiary can substitute for a 3rd party API, and my write up "Background on the Fusion REST API" - useful for understanding how to start creating Fusion Integrations. It shows how to use the SOAP adapter with Fusion, invoke Fusion's REST API, use the ERP adapter for querying, FBDI imports, and business events firing to an Apiary endpoint. The salesforce adapter's querying capabilities are also exhibited.  

[Hybrid integration with the agent](https://github.com/OracleCPS/oichybridintegration)

The agent enables communication with systems not visible through the internet, via port 22. 

[Workshop on BigQuery + Oauth2 authentication](https://github.com/GaryHostt/BigQueryIntegration)

This workshop shows how to use the Oauth2 policy with the OIC REST adapter and the basics of scheduled integrations.

[Hitting an OIC endpoint with a Python API call](https://github.com/GaryHostt/BigQueryIntegration/blob/master/Part2.md)

In this followup, my python script only needs to worry about the basic auth to the OIC API. OIC is taking care of Oauth2 with BigQuery.

## Advanced use cases

### Embeding VBCS & Process forms 

Being able to embed VBCS & Process forms is a powerful feature - imagine a company is doing a Lift and Shift of EBS to Cloud ERP over the course of 6 months. As they move different modules, new business users can interact with forms embedded in Cloud ERP that are actually integrated to call endpoints in EBS. This means that EBS is integrated with ERP - without even being noticed. This effectively places the functionality of the non-migrated parts of EBS inside of Cloud ERP.

[Extending SaaS with VBCS](https://www.ateam-oracle.com/what-you-should-know-when-extending-saas-with-vbcs-%E2%80%93-part-1-the-user-)

This blog is the first in a series about doing this. The other parts are at the bottom ‘click here to proceed.’

[Embedding Process forms](https://javiermugueta.blog/2019/02/07/embedding-process-forms-in-your-applications/)

This describes embedding process forms elsewhere. 

[Embedding your VBCS applications - Youtube](https://www.youtube.com/watch?v=JedAYzZSX6U)

This video shows how to embed your VBCS applications elsewhere. 

[Configuring VBCS for embedding](https://docs.oracle.com/en/cloud/paas/content-cloud/developer/embed-vbcs-page-site-page.html)

Steps 1 - 10 of this show how to enable embedding on VBCS

[Using Process Automation with VBCS](https://www.youtube.com/watch?v=E6Uv7f_3Hs4)

This video shows how to integrate processes in VBCS.

### Using publish/subscribe

[Part1](https://blogs.oracle.com/integration/integration-patterns-publishsubscribe-part1)

[Part2](https://blogs.oracle.com/integration/integration-patterns-publishsubscribe-part2)

### More ways to use OIC

[Sending attachments with notifications](https://www.techsupper.com/2019/12/send-notification-with-attachment-in-oracle-integration-cloud.html)

[Invoking OIC endpoints with Oauth & IDCS](https://www.ateam-oracle.com/trigger-oic-integration-using-oauth)

[Integrating chatbots with VBCS applications](https://blogs.oracle.com/vbcs/integrating-chatbots-into-vbcs-applications)

You can also give chatbots [custom skills to call APIs](https://github.com/oracle/opa-oda-chat) from conversations - this could include the OIC API.

[Using lookup tables](https://www.techsupper.com/2019/12/oracle-integration-cloud-service-lookups-2.html)

[Using a javascript function in OIC](https://blogs.oracle.com/integration/using-a-library-in-oic)

Have to deal with an oddly formatted flat file or API response? Use a javascript function to convert it to a workable format.

[Oracle Integration Solutions Catalog](https://docs.oracle.com/en/solutions/index.html?product=Oracle%20Integration&technology=PaaS&page=0&is=true&sort=0)

This provides pre-defined cross-product architectures.

[Best video for installing an agent](https://www.youtube.com/watch?v=nYmOgX95wd4)

The agent is required to integrate with systems not accessible from the internet. You only need an egress rule allowing communication over port 22. The agent ensures your on-premises systems can live in harmony with the cloud. If you are having issues, [check this page for solutions](https://docs.oracle.com/en/cloud/paas/integration-cloud/integrations-user/troubleshooting-premises-connectivity-agent-issues.html); ex: when configuring - do not use an IDCS federated account!

[Using a Service Gateway to route agent traffic soley over OCI](https://blogs.oracle.com/cloud-infrastructure/access-oracle-services-privately-with-a-service-gateway)

### Integrating with Applications

#### Oracle

[Integration with PeopleSoft, part 1](https://redthunder.blog/2018/08/28/peoplesoft-integration-using-oracle-integration-cloud-part-1/) & [Integration with PeopleSoft, part 2](https://redthunder.blog/2018/09/06/peoplesoft-integration-using-oracle-integration-cloud-part-2/)

[Service Cloud to Eloqua, bulk](https://redthunder.blog/2019/04/15/oracle-service-cloud-to-eloqua-bulk-opportunity-import-using-service-cloud-roql-and-oic/)

[Service Cloud to Eloqua, custom objects](https://redthunder.blog/2019/04/15/oracle-service-cloud-to-eloqua-custom-object-replication/)

[Service Cloud to Eloqua, contacts](https://redthunder.blog/2019/04/15/oracle-service-cloud-to-eloqua-contact-create-update-using-oic/)

[Netsuite real time integration](https://www.ateam-oracle.com/netsuite-integration-series-part-1)

[Using FBDI with ERP](https://blog.rubiconred.com/oic-erp-integration-using-file-based-data-import-fbdi/)

[Extracting bulk data from Fusion HCM](https://docs.oracle.com/en/cloud/paas/integration-cloud/hcm-adapter/sample-integration-flow-demonstrate-extract-bulk-data-option.html)

[Using HCM ATOM feeds in OIC](http://niallcblogs.blogspot.com/2019/01/677-consume-hcm-atom-feeds-in-oracle.html)

#### Non-Oracle

[Using the ServiceNow adapter](https://niallcblogs.blogspot.com/2020/03/759-oic-creating-incident-in-servicenow.html?m=1)

[Robotic Process Automation with the UiPath adapter](https://www.ateam-oracle.com/integrating-oracle-integration-cloud-integration-with-uipath)

[Making a connection with the Workday adapter](https://shalindrasingh.wordpress.com/2017/11/21/oracle-cloud-adapter-for-workday-important-things-to-not/)

### Integrating with Oracle PaaS

[SOA with OIC](https://blogs.oracle.com/integration/how-soa-suite-adapter-can-help-leverage-your-on-premises-investments)

[Oauth2 token with the REST adapter](https://docs.oracle.com/en/cloud/paas/integration-cloud-service/icsre/configuring-rest-adapter-consume-rest-api-protected-using-2-legged-oauth-token-based-authentication.html)

[Using VBCS with Fusion](https://www.ateam-oracle.com/the-cloud-native-approach-to-extending-your-saas-applications)
    [Supplemental material on cloud native architecture](https://github.com/GaryHostt/OCI_DevOps)

[Invoking the REST API for IDCS](https://docs.oracle.com/en/cloud/paas/identity-cloud/rest-api/op-admin-v1-users-post.html)

Example: When a new user is created somewhere - they can then be created in IDCS.

[Calling a serverless function with the REST adapter](https://github.com/GaryHostt/OCI_DevOps/blob/master/304.md)

[Object Storage with OIC](https://redthunder.blog/2020/01/13/object-storage-with-oracle-integration-cloud-part-1/)

In order to call the OCI REST API, like above, - you have to configure the REST adapter connection to use the [OCI Signature Version 1 security policy](https://docs.oracle.com/en/cloud/paas/integration-cloud/whats-new/index.html#INTWN-GUID-39D35E54-3FA5-4A44-A6FB-7C6496ED7E84). This policy enables you to use Oracle Cloud Infrastructure services. For example, you can create an integration that lists your VCNs. 

An event that occurs in OCI can also be fired to an [integration HTTPS endpoint](https://github.com/GaryHostt/OCI_DevOps/blob/master/Lab100.md).

You can also call the [Oracle Blockchain platform's API](https://www.youtube.com/watch?v=StxK60ZtBok). 

## Administering OIC

[Administration homepage](https://docs.oracle.com/en/cloud/paas/integration-cloud/integration-cloud-auton/index.html)

[OIC API](https://docs.cloud.oracle.com/en-us/iaas/api/#/en/integration/20190131/)

The API documentation above can be used to automate turning off/on your Integration instance or change the compartment where it's located. 

[BYOL metering, do I qualify? Turning it on/off](https://blogs.oracle.com/integration/turn-byol-metering-on-or-off-in-oracle-integration-cloud)

[Sending notifications with a custom email](https://blogs.oracle.com/integration/sending-oic-notifications-from-an-email-address-of-your-choice)

[Creating budgets and budget alerts in OCI](https://blogs.oracle.com/cloud-infrastructure/how-to-get-control-of-your-spending-in-oracle-cloud-infrastructure)

[CI/CD for OIC (video)](https://www.youtube.com/watch?v=FiC7PfN7wZ0), CI/CD for OIC [(article)](https://redthunder.blog/2017/02/26/teaching-how-to-use-developer-cloud-service-to-promote-ics-integrations-into-new-environments/)

This is an alternative to just using import/export.

Integration roles in IDCS (gen1) - [IDCS roles for OIC](https://docs.oracle.com/en/cloud/paas/integration-cloud/integration-cloud-auton/assign-roles-user.html)

Integration roles in IAM (gen2) - [IAM roles for OIC](https://docs.oracle.com/en/cloud/paas/integration-cloud/oracle-integration-oci/setting-users-and-groups.html)

## Error Handling in OIC

[Error Handling Guide](https://www.ateam-oracle.com/oic-error-handling-guide)

[Error management](https://blogs.oracle.com/integration/oic-bulk-recovery-of-fault-instances)

[Real examples of error with & without error handling](https://github.com/GaryHostt/Oracle_Integration/blob/master/Errors.md)

## Beyond application integration

[Hybrid & Multi cloud integration](https://medium.com/@bennett.stephen/hybrid-multi-cloud-integration-75290733a41b)

[Data Integration](https://s-bennett.com/data-integration/)

[Managing your integration endpoints with API Gateway](https://github.com/GaryHostt/OCI_DevOps/blob/master/Lab301.md)

[Getting started with APIPCS + Apiary](https://github.com/OracleCPS/End-to-end-API-Workshop/tree/master/workshop)

[Managing your endpoints with APIPCS](https://github.com/OracleCPS/APIPCS-ICS), seen in action [here](https://www.youtube.com/watch?v=dJlAA71whVY)

[Other Fusion integration methods](https://shalindrasingh.wordpress.com/2019/03/12/all-you-need-to-know-about-integrating-oracle-erp-cloud/)

[Using the BI Cloud Connector with Fusion, BICC](https://www.ateam-oracle.com/bi-cloud-connector-download-data-extraction-files)

[Build an APEX app in 30 minutes](https://github.com/fatih-keles/30-min-workshops)
  
[More from Oracle's Cloud Solution Hubs](https://www.oracle.com/cloud/solution-hubs/demos.html)










