# Oracle Integration

<p align="center">
  <img src="https://github.com/GaryHostt/Oracle_Integration/blob/master/screenshots/1.png?raw=true" alt="OIC"/>
</p>

## Introduction

This guide acts as a repository for getting started with Oracle Integration and where to find relevant resources. For the past ~2 years I have worked at the Santa Monica Cloud Solution hub, creating [demos like these](https://www.oracle.com/cloud/solution-hubs/demos.html).

OIC is a fully managed service offered by Oracle. OIC offers application integration, typically triggered by an enterprise's business events. OIC substantially differs from Oracle's [ODI](https://github.com/GaryHostt/Oracle_Data_Integrator), EDQ, and [GoldenGate](https://github.com/GaryHostt/GoldenGate2ADB) offerings. The latter 3 offer data integration - moving larger quantities of data, typically to a data warehouse, for analysis. Of course, the 2 families of products can be used for more than just those use cases, but this is just to give an idea of the difference between application and data integration.

## Outline

1. General Resources
2. Components of OIC
3. Basic types of integrations
4. Treatise: developing in OIC
5. Other resources
6. Youtube sources
7. Workshops
8. Advanced use cases
9. Administering OIC
10. Beyond Application Integration

## General resources

[OIC documentation](https://docs.oracle.com/en/cloud/paas/integration-cloud/index.html)

[OIC Product homepage](https://www.oracle.com/middleware/application-integration/)

[OIC adapters](https://docs.oracle.com/en/cloud/paas/integration-cloud/find-adapters.html)

What credentials do I need to connect, how do I configure SaaS outbound messaging, what can an adapter do? If these questions sound familiar, start with the adapter documentation.

[OIC Pricing & editions](https://www.oracle.com/cloud/integration/pricing.html)
  
  [Oracle Integration for Oracle SaaS](https://docs.oracle.com/en/cloud/paas/integration-cloud/integration-cloud-auton/oracle-integration-oracle-saas.html)

[The first tutorial](https://www.oracle.com/webfolder/s/assets/demo/integration-simulator/index.html#step1)

If you do not have an OIC environment, you can do the tutorial above. 

[Beginner tutorials](https://docs.oracle.com/en/cloud/paas/integration-cloud/tutorials.html)

You can begin these tutorials after you spin up OIC.

[Marketplace](https://cloudmarketplace.oracle.com/marketplace/en_US/listing/48917996)
You can download pre-made integration recipes from here and customize them to your use case. 

[Managing Oracle Integration](https://docs.oracle.com/en/cloud/paas/integration-cloud/integration-cloud-auton/manage-oracle-integration.html)

This goes into topics such as scaling, starting/stoping, and how to import/export integrations.

[Integration blog](https://blogs.oracle.com/integration/)

This is where new features for OIC are announced and very helpful articles. If you see a new feature you want to try - here's how you can [request a feature flag](https://blogs.oracle.com/integration/enabling-the-future-today-feature-flags-in-oracle-integration-cloud).

[OIC REST API documentation](https://docs.oracle.com/en/cloud/paas/integration-cloud/rest-api/index.html)

[Customer success stories](https://blogs.oracle.com/integration/oracle-integration-customer-success-stories)

## Components of OIC

1. Application integration
2. VBCS
3. Insight
4. The agent

....................
note: Enterprise edition only below
....................

5. Process Automation
6. On-premises Enterprise Application adapters 

## Basic types of integrations

1. SaaS triggers
2. Scheduled
3. Generic REST trigger

There are more than these, but these have been the typical use case patterns I have seen.

### SaaS triggers

SaaS trigger integrations typically rely upon these factors:
1. Use of one of the SaaS adapters in trigger or trigger & invoke mode
2. Configuration on the SaaS to fire off an outbound message or a webhook to OIC
3. An event then occuring in the SaaS application that will trigger communication with OIC

[Eloqua trigger](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCAA/Help/Apps/IntegrationCloudService/Tasks/InstallingICS.htm)

Using Eloqua as a trigger rquires installing this and linking it to your OIC environment. 

[Eloqua continued](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-user/Help/Apps/IntegrationCloudService/Tasks/AddingICSToCanvas.htm)

After installing the agent, you then need to create the rule in Eloqua that triggers the outbound call to OIC.

[Fusion](https://github.com/GaryHostt/OIC_SaaS_integration)

This workshop shows how to configure Cloud ERP to perform outbound communication with OIC. In it, a product created in Product Data Hub triggers an integration to then add the data to CPQ. It will also get you started with Process Automation.

[Configuring salesforce for business events](https://www.youtube.com/watch?v=5Pq-Dme5Gvc&feature=share)

### Scheduled

Schedule integrations are good for sending data from applications to data warehouses, reading information from csv files, or pulling information from applications and writing that data to a csv file. 

1. Pulling from FTP/Writing to FTP/Load data warehouse

[BigQuery data load](https://github.com/GaryHostt/BigQueryIntegration)

This is an example of pulling info from applications and writing them to Google's data warehouse via its REST API. 

[Reading large files](https://www.youtube.com/watch?v=hE5nF-henFw&feature=share)

OIC can process flat files up to 1 GB in size. For larger files - [check out my data integration repo](https://github.com/GaryHostt/Oracle_Data_Integrator). 

[Listing files from FTP](https://www.techsupper.com/2019/05/how-to-list-files-from-ftp-resides-in-multiple-directories-oracle-integration-cloud.html)

[File integration for Fusion ERP](https://www.youtube.com/watch?v=7wx-gAT4IdU)

### Generic REST endpoint

The next use case pattern is basically using OIC as a drag and drop API builder. You can use a blank REST adapter at the beginning of your integrations. This can be used to abstract away SOAP endpoints. 

[ATP Workshop](https://github.com/GaryHostt/ATPworkshop)

In this workshop, we basically use OIC to create a REST API for an Autonomous Transaction Processing database. [Click here](http://media.licdn.com/embeds/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2F-9nP2LaeOok%3Ffeature%3Doembed&amp;url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3D-9nP2LaeOok&amp;type=text%2Fhtml&amp;schema=youtube) for a video explanation by a colleague & I. It will also show you how to get started using VBCS.

[Webhooks](https://www.youtube.com/watch?v=rUaDIH5ZXB8) can be configured to send payloads to a generic REST endpoint.

## Treatise: developing in OIC

### Mapping 
I want to explain how to go about developing for an integration, such as is in [this lab](https://github.com/GaryHostt/Fusion_PurchaseOrder_Integration). How does one know what fields to have in the mapper when you have a generic endpoint going to a SaaS API?

When trying to invoke a SaaS endpoint - perhaps with a generic REST trigger, you may find that you are inundated with hundreds of fields in the mapper you do not know. Typically, beginning by searching [the documentation is a good start](https://docs.oracle.com/en/cloud/saas/procurement/19a/oaprc/manage-purchase-orders.html#OAPRC1007407). Looking at the 'Defaults:' we can find the fields required for the lines, schedules, and distributions.

Then, to understand the data types for the fields - or see how they're formatted, I'd call the endpoint to give me one or all of the business object I'm trying to create. In this case, I can look at the [get all POs or get 1 PO endpoints](https://docs.oracle.com/en/cloud/saas/procurement/19d/fapra/api-purchase-orders.html).

Typically for development purposes, I would start this integration with a generic REST adapter that has ~5 fields that map to important fields for the SaaS. After filling out the important ones like POHeaderID, ItemNumber, etc. I activate the integration, then send a payload. Typically, the response will be a 500 error and the response will then contain the field that still requires an attribute. Next, deactivate the integration, map another field from the REST adapter to the business field and then from Postman you can dynamically try different values for that field. Once you find a working value - you can leave it in the submission to the generic REST adapter, or just hardcode it in the adapter. 

### If your integration is not appearing in tracking after performing the trigger action in SaaS

If you create a contact in Fusion, or a new lead in salesforce, but the business event isn't appearing in tracking - you need to verify that you properly configured the SaaS. You should also check if you are logged in as the user on the SaaS that you used to create the connection on OIC.

## Other Resources

[Adapter certifications](https://www.oracle.com/technetwork/middleware/adapters/documentation/adaptercertificationmatrix0217-3613709.pdf)

This is where I go to examine if a SaaS version is compatible with the given adapter.

[Ankur Jain's blog](https://blogs.oracle.com/author/ankur-jain)

[Shalindra's blog](https://shalindrasingh.wordpress.com/)
    
[Oracle Integration Certification](https://learn.oracle.com/ords/training/dl4_pages.getpage?page=dl4learningpath&get_params=offering:37194,id:57591)

  [Certification Study Guide](https://www.techsupper.com/2020/02/oracle-cloud-platform-application-integration-2019-associate.html)

## Youtube

[Santa Monica Hub Youtube channel](https://www.youtube.com/channel/UCW04sPyVsthkrjPs_Gx-dFA/featured?disable_polymer=1)

This channel can get your started with VBCS, Process Automation, RPA, and more! 

[Invoking Fusion's API](https://www.youtube.com/watch?v=6j_Qi2qpmKM)

[Using the REST adapter](https://www.youtube.com/watch?v=NDwTCKDC8LM)

[Navigating OIC](https://www.youtube.com/watch?v=U51IZ7og1Zw&t=1353s)

## More Workshops

[HCM to EBS](https://github.com/KseniiaRyuma/HCM_to_EBS_integration/blob/master/oic100.md)

[OIC Fusion & HCM Integration](https://github.com/OracleCPS/oicsaasintegration)

[Integration workshop with Process](https://github.com/OracleCPS/oracleintegrationday)

[Hybrid integration with the agent](https://github.com/OracleCPS/oichybridintegration)

[Getting started with VBCS](https://github.com/OracleCPS/Visual-Builder-Cloud-Service_VBCS)

[VBCS, Process, REST](https://github.com/OracleCPS/aiconlineshopping)

[Basics, database, REST, Process](https://github.com/oracle/learning-library/blob/master/ospa-library/appint/ApplicationIntegration-labguide.md)

[Hitting an OIC endpoint with a Python API call](https://github.com/GaryHostt/BigQueryIntegration/blob/master/Part2.md)

## Advanced use cases

### Integration with bigquery + Oauth2 authentication

[Workshop on BigQuery + Oauth2 authentication](https://github.com/GaryHostt/BigQueryIntegration)

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

### Other use cases 

[Integrating chatbots with VBCS applications](https://blogs.oracle.com/vbcs/integrating-chatbots-into-vbcs-applications)

[VBCS with Fusion](https://www.ateam-oracle.com/the-cloud-native-approach-to-extending-your-saas-applications)
    [Supplemental material on cloud native architecture](https://github.com/GaryHostt/OCI_DevOps)

[Service Cloud to Eloqua](https://redthunder.blog/2019/04/15/oracle-service-cloud-to-eloqua-contact-create-update-using-oic/)

[Oauth2 token with the REST adapter](https://docs.oracle.com/en/cloud/paas/integration-cloud-service/icsre/configuring-rest-adapter-consume-rest-api-protected-using-2-legged-oauth-token-based-authentication.html)

[REST API for IDCS](https://docs.oracle.com/en/cloud/paas/identity-cloud/rest-api/op-admin-v1-users-post.html)

Example: When a new user is created somewhere - they can then be created in IDCS.

[Fusion SOAP API](https://docs.oracle.com/en/cloud/saas/procurement/18b/oeswp/Purchase-Order-Service-Version-2-PurchaseOrderService-svc-3.html)

When the SaaS adapter and the REST API don't have something you need - check the SOAP API. 

[Extracting bulk data from Fusion HCM](https://docs.oracle.com/en/cloud/paas/integration-cloud/hcm-adapter/sample-integration-flow-demonstrate-extract-bulk-data-option.html)

[Object Storage with OIC](https://redthunder.blog/2020/01/13/object-storage-with-oracle-integration-cloud-part-1/)

In order to call the OCI REST API, like above, - you have to configure the REST adapter connection to use the [OCI Signature Version 1 security policy](https://docs.oracle.com/en/cloud/paas/integration-cloud/whats-new/index.html#INTWN-GUID-39D35E54-3FA5-4A44-A6FB-7C6496ED7E84). This policy enables you to use Oracle Cloud Infrastructure services. For example, you can create an integration that lists your VCNs. 

An event that occurs in OCI can also be fired to an [integration HTTPS endpoint].(https://github.com/GaryHostt/OCI_DevOps/blob/master/Lab100.md)

[Oracle Integration Solutions Catalog](https://docs.oracle.com/en/solutions/index.html?product=Oracle%20Integration&technology=PaaS&page=0&is=true&sort=0)

This provides pre-defined cross-product architectures.

[Installing an agent](https://www.youtube.com/watch?v=nYmOgX95wd4)

The agent is required to integrate with systems not accessible from the internet. You only need an egress rule allowing communication over port 22. The agent ensures your on-premises systems can live in harmony with the cloud.

## Administering OIC

[Turning on/off BYOL metering](https://blogs.oracle.com/integration/turn-byol-metering-on-or-off-in-oracle-integration-cloud)

[Sending notifications with a custom email](https://blogs.oracle.com/integration/sending-oic-notifications-from-an-email-address-of-your-choice)

[CI/CD for OIC](https://www.youtube.com/watch?v=FiC7PfN7wZ0)

This is an alternative to just using import/export.

Integration roles in IDCS (gen1) - [IDCS roles for OIC](https://docs.oracle.com/en/cloud/paas/integration-cloud/integration-cloud-auton/assign-roles-user.html)

Integration roles in IAM (gen2) - [IAM roles for OIC](https://docs.oracle.com/en/cloud/paas/integration-cloud/oracle-integration-oci/setting-users-and-groups.html)

[Error management](https://blogs.oracle.com/integration/oic-bulk-recovery-of-fault-instances)

## Beyond application integration

[Hybrid & Multi cloud integration](https://medium.com/@bennett.stephen/hybrid-multi-cloud-integration-75290733a41b)

[Data Integration](https://s-bennett.com/data-integration/)

[Managing your integration endpoints with API Gateway](https://github.com/GaryHostt/OCI_DevOps/blob/master/Lab301.md)

[Managing your endpoints with APIPCS](https://github.com/OracleCPS/APIPCS-ICS)

[Other Fusion integration methods](https://shalindrasingh.wordpress.com/2019/03/12/all-you-need-to-know-about-integrating-oracle-erp-cloud/)
  [Using the BI Cloud Connector](https://www.ateam-oracle.com/bi-cloud-connector-download-data-extraction-files)










