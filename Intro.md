
# Oracle Integration (OIC) Generation 2 + File Server information

..this page is a continuation of my [overarching OIC knowledge repo here](https://garyhostt.github.io/Oracle_Integration/). 

This page details:
- Why you should upgrade to OIC generation 2 and how to use it
- Using the OIC File Server  (gen2 only)
   - Configuration
   - Use cases
   - Best practices
   - Videos
   - FTP tools

## Oracle Integration, Generation 2

[Main Documentation for OIC Gen 2](https://docs.oracle.com/en/cloud/paas/integration-cloud/oracle-integration-oci/overview-oracle-integration-generation-2.html)
```
Oracle Integration Generation 2 is the next generation of our Oracle Integration platform. This upgrade delivers improved performance and reliability as well as significant improvements in provisioning and other lifecycle management activities by more deeply leveraging the power of Oracle Cloud Infrastructure.
```

[Lists all benefits of Gen2 over Gen1](https://blogs.oracle.com/integration/oracle-integration-oic-generation-2-is-now-available-in-all-cloud-tenancies) - more than just the File Server!

- OIC generation 2 enables OIC to be managed by the OCI API, you can see an example of interacting with OIC via the OCI API [here](https://garyhostt.github.io/OIC_start-stop/).

[How to upgrade from Gen1 to Gen2](https://docs.oracle.com/en/cloud/paas/integration-cloud/oracle-integration-oci/upgrade-oracle-integration-generation-2.html#GUID-22F20017-87C7-47A5-8AEF-1CDBF564C7A6) - it's easy as 1, 2, 3 **and doesn't cost extra $**

- You can also manually export your integrations, processes, and VBCS apps. Spin up a gen 2 instance, and then import those to your new environment. However, your instance URL will change and you will need to reconfigure your connections & users. IF this manually process is verified 100% to work for you - then you can delete your gen 1 instance. **This is not the default recommended approach**, but it may be satisfactory work for some.

[IAM roles for OIC Gen2](https://docs.cloud.oracle.com/en-us/iaas/integration/doc/setting-users-and-groups-oracle-integration-generation-2.html)
- With generation 2, you still use IDCS to manage permissions of what users can on the OIC Service Console, but for managing OIC on the OCI console, that is managed by IAM. Click here to learn more about [identity federation](https://docs.cloud.oracle.com/en-us/iaas/Content/Identity/Tasks/federatingIDCS.htm) on OCI. This and other tasks are explained on the 
[main documentation for gen2](https://docs.oracle.com/en/cloud/paas/integration-cloud/oracle-integration-oci/overview-oracle-integration-generation-2.html).

**How do I know if I have gen2 or gen1?**

How to access each in the [OCI console](https://console.us-ashburn-1.oraclecloud.com/):

- Oracle Integration Generation 2: Application Integration -> Integration
- Oracle Integration Generation 1: Platform Services -> Integration

## File Server

[Main file server documentation](https://docs.oracle.com/en/cloud/paas/integration-cloud/file-server.html)

[PDF form of documentation](https://docs.oracle.com/en/cloud/paas/integration-cloud/file-server/using-file-server-oracle-integration-generation-2.pdf)

The file server **does not cost any extra money**. If you use it in integrations, it will count towards your billable messages.

### Configuration

[Follow these steps to configure your file server](https://blogs.oracle.com/integration/embedded-file-server-sftp-in-oracle-integration)

[To configure the file server with the FTP adapter in OIC](https://docs.oracle.com/en/cloud/paas/integration-cloud/ftp-adapter/create-connection.html#GUID-662EF1FD-2841-4A9A-87B3-FD8B8796510D)

- You can use a ssh key you upload for your user or their password. On the adapter page, you need to set the SFTP connection selectBox to yes. You can get your IP address and port from the Settings -> File Server page in your OIC instance. Make sure your user is enabled and has write access. Inside your integration, you need to use the folder for which your user has write access.

[User permissions and enabling users](https://blogs.oracle.com/integration/leveraging-oracle-integration-file-server-for-file-based-integrations-v2)

### Use Cases

1. OIC log collection & analysis

* [Download OIC logs from the API](https://docs.oracle.com/en/cloud/paas/integration-cloud/rest-api/op-ic-api-integration-v1-monitoring-logs-id-get.html)
    * You can call this OIC endpoint, and have your OIC logs stored on your embedded FTP server and/or sent to object storage for analysis by your security platform of choice
    * To authenticate to OIC’s API with the REST adapter, simply base the url of your instance before /ic/home ; and use the basic auth security with the user you are logged in with, or another IDCS user that has the ’Service Administrator’ role for your OIC instance.
    * Object storage with OIC:
        * [Part 1](https://redthunder.blog/2020/01/13/object-storage-with-oracle-integration-cloud-part-1/comment-page-1/)
        * [Part 2](https://redthunder.blog/2020/03/20/object-storage-with-oracle-integration-cloud-part-2/)
        * [Official documentation on REST adapter & signature policy](https://docs.oracle.com/en/cloud/paas/integration-cloud/whats-new/index.html#INTWN-GUID-39D35E54-3FA5-4A44-A6FB-7C6496ED7E84)

2. BICC Extract from object storage to File Server

    * [Configuring BICC with Object Storage](https://www.ateam-oracle.com/reference-architecture-fusion-saas-data-replication-into-adw-%3A-using-odi-marketplace-and-bicc)
    * Ignore the parts about ODI and ADW, follow the steps to configure BICC And object storage

3. Pulling to/from external system

    * [This previous lab shows how OIC can read data in a csv and pass it elsewhere](https://garyhostt.github.io/BigQueryIntegration/)
    * [You can use your file server for FBDI import files](https://antonyjr.github.io/Hands-On-Labs/ERP-Integration-Patterns/html/erp-cloud-fbdi-import-simple.html)
      - [alternative FBDI lab](https://github.com/maldu23/Fusion-FBDI-Integration/blob/master/FBDI_Wkshp.md)
    * [Or HDL imports in HCM](https://antonyjr.github.io/Hands-On-Labs/HCM-Integration-Patterns/html/hcm-cloud-pending-worker-import-simple.html)

### Best practices for handling files in OIC
[File Handling Primer](https://www.ateam-oracle.com/integration-cloud-file-handling-primer)

[File integration best practices](https://blogs.oracle.com/fmw/oracle-integration-cloud-oic-file-based-integration-best-practices)

* How to handle the file depends upon its size, where it’s from, and what you want to do with it. For using the FTP adapter in integration, you will typically use scheduled integrations - maybe app-driven if you’re having an API append rows to an existing csv file.

[Scheduling scheduled integrations](https://docs.oracle.com/en/cloud/paas/integration-cloud/integrations-user/creating-scheduled-integrations.html#GUID-9632A5C8-98A7-4371-B542-6A8583427C8D)

* This shows how to use iCal expressions to set up more complex schedules with OIC.

### Videos
The latest videos describe some key components used creating an integration to import and process bulk files using the FTP Adapter with Oracle Integration:
* [Bulk File Transfer with the FTP Adapter](https://www.youtube.com/watch?v=fWvbnIh6WvQ&t=9s)
* [Use the Stage File Action in Oracle Integrations](https://www.youtube.com/watch?v=LLEHt4kno9M&t=158s)
* [Use the For-Each Action in Oracle Integrations](https://www.youtube.com/watch?v=-Cfq2fYwCTk)

### FTP tools

[cyberduck](https://cyberduck.io/download/)

[filezilla](https://filezilla-project.org/download.php)

- the above are both clients you can use with your laptop to access FTP servers

[Python with sFTP](https://pysftp.readthedocs.io/en/release_0.2.9/)

- the OIC file server uses sFTP protocol
