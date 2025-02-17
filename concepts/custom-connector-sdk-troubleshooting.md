---
title: "Troubleshoot issues with the Microsoft Graph connectors SDK"
author: rchanda1392
manager: harshkum
ms.localizationpriority: medium
doc_type: conceptualPageType
ms.prod: search
description: "Learn how to troubleshoot issues with the Microsoft Graph connectors SDK."
---

# Troubleshoot issues with the Microsoft Graph connectors SDK

This article describes some of the most common issues with the Microsoft Graph connectors SDK, and how to troubleshoot them.

## Items missing from the index

If items that previously existed are missing from the index, this might be due to the delete detection logic in the platform. Items missing from a success response in [OperationStatus](/graph/custom-connector-sdk-contracts-common#operationstatus) that are already in the index will be removed from the index.

If the connector sends transient failure responses, and more than 10% of the items resulted in crawl failures, items that aren't included in the last two crawls will be deleted.

## Handle connector port changes

When the connector needs to run on a different port, you need to update the port map configuration file with the new values. When you edit the port map configuration file, you must restart the GCA service for the changes to take effect. To restart the service, open services.msc and restart **GcaHostService**.

![Screenshot of the services window with GcaHostService running](images/connectors-sdk/services.png)

## Connector service is unavailable

If the crawls are failing with a connector unavailable on specified port error, verify the following:  

1. The connector is indeed running on the specified port and hasn't crashed and isn't stuck.
2. The port specified in the port map configuration file is correct.
3. If the port map configuration file has been edited, be sure to restart **GcaHostService**.

## Handle RPC errors

If you see any RPC errors during the communication between the Microsoft Graph connector agent platform and the connector, you can look up the error codes on the [status codes](https://grpc.github.io/grpc/core/md_doc_statuscodes.html) page.

If the error code is **Unknown**, there's likely an unhandled exception in your connector code. Make sure that you send a response with success/failure operation status in all cases.

## Errors with hosting a connector as a Windows service

### Service failed to start due to Access denied error

Use the following steps to make sure that the path of the executable is accessible to the LocalSystem account.

1. Right-click the folder that contains the executable and choose **Properties**.

2. Open the **Security** tab and under **Group or user names**, choose **Edit**.

3. Choose **Add**.

4. Enter 'LOCAL SERVICE' as the object name and choose **Check Names**.

    ![Screenshot of the Object name field with Local Service input](images/connectors-sdk/troubleshoot3.png)

5. Choose **OK** on each dialog box.

### Service fails to start with any error

If the service fails to start, check the event viewer error logs. Open the event viewer and go to **Windows logs > Application** and **Windows logs > System**.

![Screenshot of the error logs in the event viewer](images/connectors-sdk/troubleshoot4.png)

## See also

* [Best practices](/graph/custom-connector-sdk-best-practices)
