# OIC logs: FTP unzipping to embedded File Server + Object Storage

## Intro

In this lab, you will download OIC's auditlogs and interact with it via the REST and FTP adapters.

The audit log which "Contains all the changes users have made on design-time artifacts such as integrations, connections, adapters, and so on. Contains information for all integrations."

You will learn how to unzip files downloaded from REST endpoints and subsequently store them via FTP and to object storage. Nothing besides a configured OIC instance is required to complete this article.

### Additional links

[Official OIC Documentation](https://docs.oracle.com/en/cloud/paas/integration-cloud/index.html)

[My OIC Repo](https://garyhostt.github.io/Oracle_Integration/)

[OIC File Server + GEN 2](https://github.com/GaryHostt/Oracle_Integration/blob/master/Intro.md)

[ERP with OIC](https://garyhostt.github.io/beginner_ERP_OIC/)

## Pre-requisites
[Configure OIC File Server](https://blogs.oracle.com/integration/embedded-file-server-sftp-in-oracle-integration)
[Filezilla](https://filezilla-project.org/) or [cyberduck](https://cyberduck.io/)

## Integration export + video

[my .iar export]()

To use the export, you need to edit the connections, FTP directories, and REST url of object storage, along with any mappings that display a warning symbol. See monitoring for any errors during development. Be sure to track the payload when activating your integration for troubleshooting.

[video]

You will need to edit the following after importing
  - FTP, OIC REST, OCI REST connections
  - FTP connections and OCI Object storage REST connection on canvas
  -Any mapper that gains a wanring symbol

## Workshop

### 1: Connect to file server with FTP adapter

[FTP adapter documentation](https://docs.oracle.com/en/cloud/paas/integration-cloud/ftp-adapter/create-connection.html#GUID-662EF1FD-2841-4A9A-87B3-FD8B8796510D)
```
You can use a ssh key you upload for your user or their password. On the adapter page, you need to set the SFTP connection selectBox to yes. You can get your IP address and port from the Settings -> File Server page in your OIC instance. Make sure your user is enabled and has write access. Inside your integration, you need to use the folder for which your user has write access.
```

[Ensure your integration users have sufficient permissions](https://blogs.oracle.com/integration/leveraging-oracle-integration-file-server-for-file-based-integrations-v2)

### Connect to OIC API with REST Adapter

![](Logscreenshots/1.png)

To authenticate to OIC’s API with the REST adapter, simply base the url of your instance before /ic/home ; and use the basic auth security with the user you are logged in with, or another IDCS user that has the ’Service Administrator’ role for your OIC instance.

### Connect OCI with REST adapter
[Official documentation on REST adapter & signature policy](https://docs.oracle.com/en/cloud/paas/integration-cloud/whats-new/index.html#INTWN-GUID-39D35E54-3FA5-4A44-A6FB-7C6496ED7E84)

Object storage with OIC:

[Part 1](https://redthunder.blog/2020/01/13/object-storage-with-oracle-integration-cloud-part-1/comment-page-1/)
- This article shows you how to use PUT via REST in OIC, which is how we upload files to object storage in OIC.

[Part 2](https://redthunder.blog/2020/03/20/object-storage-with-oracle-integration-cloud-part-2/)
- You can also use the GET verb to download files from object storage, not used in this article. 

### Create the scheduled integration

You can import my .iar export to get to this part.

![](Logscreenshots/2.png)

The getOICLogs connection uses this endpoint below. You can change the id to get different files.

[Download OIC logs this API endpoint](https://docs.oracle.com/en/cloud/paas/integration-cloud/rest-api/op-ic-api-integration-v1-monitoring-logs-id-get.html)
```
id: string
Log file identifier. Specify which file to download. 

Types:
icsflowlog: Contains information about what is happening during runtime, for active integrations that have tracing enabled.
icsdiagnosticlog: Contains information for all integrations about how the system is working. This log can be useful when you are analyzing an issue.
icsauditlog: Contains all the changes users have made on design-time artifacts such as integrations, connections, adapters, and so on. Contains information for all integrations.
```

![](Logscreenshots/3.png)

You will need to change the directories in the FTP connections to write to a directory for a user you have enabled for the file server.

#### Within for each loop

![](Logscreenshots/4.png)

You will need to change the directories on these two FTP adapters in the for each loop, and then for 'writeToObject' you will need to change the url to use your tenancy namespace and desired bucket.

### Monitoring

![](Logscreenshots/5.png)

After proper configuration, you should see a 'Succeeded' run of your integration in tracking.

![](Logscreenshots/6.png)

While the for each loop shows 3 files, only 2 are written to object storage because I have the switch statement ignore the 'environment.txt' file in the unzipped folder.

![](Logscreenshots/7.png)

In your object storage bucket, you should see files with the current date-time attached at the beginning of the file names. 

## What to do with your logs

You can call this OIC endpoint, and have your OIC logs stored on your embedded FTP server and/or sent to object storage for analysis by your security platform of choice, such as [Oracle Management Cloud](https://docs.oracle.com/en/cloud/paas/management-cloud/logcs/ingest-logs-oci-object-storage-buckets.html#GUID-4B2BED39-CF5F-450A-B0E5-6C36FBFB80F4).

Congrats, you now know how to use OIC to:
  Download a zip file from REST API
  Unzip that zip file
  Write the contents to an FTP Server
  Send the contents to object storage

  Connect to embedded OIC file server
  Use OCI object storage


