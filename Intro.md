
# OIC Generation 2 + File Server information

## Oracle Integration, Generation 2
[Main Documentation](https://docs.oracle.com/en/cloud/paas/integration-cloud/oracle-integration-oci/overview-oracle-integration-generation-2.html)
[Lists all benefits of Gen2](https://blogs.oracle.com/integration/oracle-integration-oic-generation-2-is-now-available-in-all-cloud-tenancies)
[How to upgrade from gen1 to gen2](https://docs.oracle.com/en/cloud/paas/integration-cloud/oracle-integration-oci/upgrade-oracle-integration-generation-2.html#GUID-22F20017-87C7-47A5-8AEF-1CDBF564C7A6)

## File Server

[Main documentation](https://docs.oracle.com/en/cloud/paas/integration-cloud/file-server.html)
[PDF form](https://docs.oracle.com/en/cloud/paas/integration-cloud/file-server/using-file-server-oracle-integration-generation-2.pdf)

### Configuration
[Follow these steps to configure your file server](https://blogs.oracle.com/integration/embedded-file-server-sftp-in-oracle-integration)

[To configure the file server with the FTP adapter in OIC](https://docs.oracle.com/en/cloud/paas/integration-cloud/ftp-adapter/create-connection.html#GUID-662EF1FD-2841-4A9A-87B3-FD8B8796510D)

- You can use a ssh key you upload for your user or their password. On the adapter page, you need to set the SFTP connection selectBox to yes. You can get your IP address and port from the Settings -> File Server page in your OIC instance. Make sure your user is enabled and has write access. Inside your integration, you need to use the folder for which your user has write access.

[User permissions and enabling users](https://blogs.oracle.com/integration/leveraging-oracle-integration-file-server-for-file-based-integrations-v2)

### Use Cases

1. OIC log collection & analysis
* https://docs.oracle.com/en/cloud/paas/integration-cloud/rest-api/op-ic-api-integration-v1-monitoring-logs-id-get.html
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

### Best practices for handling files in OIC
[File Handling Primer](https://www.ateam-oracle.com/integration-cloud-file-handling-primer)
[File integration best practices](https://blogs.oracle.com/fmw/oracle-integration-cloud-oic-file-based-integration-best-practices)
* How to handle the file depends upon its size, where it’s from, and what you want to do with it.
[Use the FTP adapter to connect to the file server, this is the FTP adapter official doc](https://docs.oracle.com/en/cloud/paas/integration-cloud/ftp-adapter/understand-ftp-adapter.html)
* For using the FTP adapter in integration, you will typically use scheduled integrations - maybe app-drive if you’re having an API append rows to an existing csv file. 
[Scheduling scheduled integrations](https://docs.oracle.com/en/cloud/paas/integration-cloud/integrations-user/creating-scheduled-integrations.html#GUID-9632A5C8-98A7-4371-B542-6A8583427C8D)
* This shows how to use iCal expressions to set up more complex schedules with OIC.

Videos
We continue to add new videos to our fundamental integration vocabulary and concepts series. The latest videos describe some key components used in the Create an Integration to Import and Process Bulk Files sample in Using the FTP Adapter with Oracle Integration:
* [Bulk File Transfer with the FTP Adapter](https://www.youtube.com/watch?v=fWvbnIh6WvQ&t=9s)
* [Use the Stage File Action in Oracle Integrations](https://www.youtube.com/watch?v=LLEHt4kno9M&t=158s)
* [Use the For-Each Action in Oracle Integrations](https://www.youtube.com/watch?v=-Cfq2fYwCTk)

FTP tools
[cyberduck](https://cyberduck.io/download/)
[filezilla](https://filezilla-project.org/download.php)
