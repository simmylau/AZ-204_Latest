# Linux web apps

Linux web apps run in containers called web app for linux.

To check current languages and versions supported for Linux web apps:

`az webapp list-runtimes --os-type linux`

## Limitations

- When deployed to built-in images, your code and content are allocated a storage volume for web content, backed by Azure Storage. The disk latency of this volume is higher and more variable than the latency of the container filesystem. Apps that require heavy read-only access to content files may benefit from the custom container option, which places files in the container filesystem instead of on the content volume.
- Not on shared tier.
- Features enabled as they go.

## Examine Azure App Service plans

Each App Service plan defines:

- Operating System (Windows, Linux)
- Region (West US, East US, etc.)
- Number of VM instances
- Size of VM instances (Small, Medium, Large)
- Pricing tier (Free, Shared, Basic, Standard, Premium, PremiumV2, PremiumV3, Isolated, IsolatedV2)

### Shared compute

**Free** and **Shared,** the two base tiers, runs an app on the same Azure VM as other App Service apps, including apps of other customers. These tiers allocate CPU quotas to each app that runs on the shared resources, and the **resources can't scale out**.

Receives shared CPU minutes on a shared VM instance - no scaling out.

### Shared compute 2

### Dedicated compute

The **Basic, Standard, Premium, PremiumV2,** and **PremiumV3** tiers run apps on dedicated Azure VMs. Only apps in the **same App Service plan share the same compute resources.** The higher the tier, the **more VM instances are available to you for scale-out**.

### Isolated

The **Isolated** and **IsolatedV2** tiers run dedicated Azure VMs on dedicated Azure Virtual Networks. It provides **network isolation** on top of **compute isolation** to your apps. It **provides the maximum scale-out capabilities.**

- An app runs on all the VM instances configured in the App Service plan.
- If multiple apps are in the same App Service plan, they all share the same VM instances.
- If you have multiple deployment slots for an app, all deployment slots also run on the same VM instances.
- If you enable diagnostic logs, perform backups, or run WebJobs, they also use CPU cycles and memory on these VM instances.

In this way, the App Service plan is the scale unit of the App Service apps. If the plan is configured to run five VM instances, then all apps in the plan run on all five instances. If the plan is configured for autoscaling, then all apps in the plan are scaled out together based on the autoscale settings.

### Improvements

If app resources are running out or poor performance you can improve the app's performance by isolating the compute resources. Do this by setting up the app seperately in another app service plan.

Isolate your app into a new App Service plan when:

- The app is resource-intensive.
- You want to scale the app independently from the other apps in the existing plan.
- The app needs resource in a different geographical region.

## Auto deployment 

- GitHub
- BitBucket
- Azure DevOps

## Manual deployment

- Git: App Service web apps feature a Git URL that you can add as a remote repository. Pushing to the remote repository deploys your app.
- CLI: webapp up is a feature of the az command-line interface that packages your app and deploys it. Unlike other deployment methods, az webapp up can create a new App Service web app for you if you haven't already created one.
- Zip deploy: Use curl or a similar HTTP utility to send a ZIP of your application files to App Service.
- FTP/S: FTP or FTPS is a traditional way of pushing your code to many hosting environments, including App Service.

## Deployment slots 

Recommended to set staging which comes with standard and higher. If you use swap operation it warms up the necessary worker instances to match your production scale, thus eliminating downtime.