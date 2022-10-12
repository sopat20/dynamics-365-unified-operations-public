---
# required metadata

title: Cancel an executing batch job
description: This article provides information about how to cancel an executing batch job.
author: karimelazzouni
ms.date: 03/26/2021
ms.topic: article
ms.prod: 
ms.technology: 

# optional metadata

# ms.search.form: 
# ROBOTS: 
audience: IT Pro
# ms.devlang: 
ms.reviewer: sericks
# ms.tgt_pltfrm: 
ms.custom: 62333
ms.assetid: 6135bcf7-bf8f-42ae-b2c6-458f6538e6a4
ms.search.region: Global
# ms.search.industry: 
ms.author: kaelazzo
ms.search.validFrom: 2019-05-08
ms.dyn365.ops.version: Platform update 27

---

# <a id="legacy-abort"></a>Cancel an executing batch job

[!include [banner](../includes/banner.md)]

## Cancelling a batch job

Where there is a requirement to cancel an executing batch job, you can change the status of the batch job to cancelling.
This will prevent batch from picking up new tasks for execution. The tasks which are not picked up for execution will be marked as not run and executing tasks will be marked as cancelling, but it will wait till the tasks which are in cancelling state to terminate gracefully i.e. finish or error out.

## Aborting tasks in a batch job

Sometimes jobs are stay in cancelling state for quite some time and waiting for graceful termination is not possible, this option provides a system administrator or batch job manager with the ability to abort all the tasks that are in cancelling state for the batch job. This forces the jobs to immediately stop their execution.

>[!NOTE]
> * It is important to note that this feature should be used with caution. When you cancel a running process, it is an inherently unsafe action that can lead to data corruption, resulting in either orphaned or incomplete data. This action should only be used to mitigate other issues caused by the running tasks.  
> * This cannot stop the execution of jobs that are stuck in an unmanaged wait. e.g. Tasks that are stuck due to SQL blocking or DIXF related tasks. In cases like these tasks will be aborted once the task recovers from the unmanaged wait.

Complete the following steps to immediately cancel the running task.

1. Go to **System administration** \> **Inquiries** \> **Batch jobs**.
2. Select a batch job that has a **Status** of **Canceling**.
3. On the **Batch tasks** tab, select **Abort** on the task, and then select **OK**.

### Enhanced abort feature

An enhancement to the batch abort functionality has been introduced. Upon confirmation, this will restart the batch server currently running the batch tasks that you are attempting to cancel. This makes the functionality more resilient to limitations, and ensures that tasks of the job you are trying to cancel are truly preempted.

>[!NOTE]
>Starting 10.0.31, we have added a drain delay of maximum 15 minutes on use of enhanced abort. This will allow tasks that are in executing state on the server to finish propely.
Once there are no tasks in executing state on the server or 15 minutes have passed, the batch server will be restarted.

To use the new functionality, refer to the following steps:

1. Enable the **Enhanced batch abort** feature in the [Feature management](../../fin-ops/get-started/feature-management/feature-management-overview.md) workspace.
2. Follow the same instructions to [cancel an executing batch job](#legacy-abort).

You will be prompted that the batch server, which is running the canceling tasks, will be restarted. This can potentially disrupt a list of other batch jobs. You must proceed in order to end the canceling tasks.

![Confirm that you want to end the canceling tasks.](https://user-images.githubusercontent.com/7556912/112464897-ba820680-8d6c-11eb-871a-e1aff1d82665.png)

If you do not want to cancel other running batch tasks on the server and would prefer the old behavior of canceling only the tasks under the batch job and not all the jobs running, you can turn off the **Enhanced batch abort** feature in the Feature management workspace and try to [cancel the executing batch job](#legacy-abort) again.

[!INCLUDE[footer-include](../../../includes/footer-banner.md)]
