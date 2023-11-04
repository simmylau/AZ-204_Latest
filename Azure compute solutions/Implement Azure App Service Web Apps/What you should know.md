# Implement Azure App Service Web Apps

[Preparing for AZ-204 - Develop Azure compute solutions (1 of 5)](https://learn.microsoft.com/en-us/shows/exam-readiness-zone/preparing-for-az-204-develop-azure-compute-solutions-1-of-5)

## You should know how to do the following

- How to create an Azure App Service Web App.
- Know the difference between the app service plan and the app service web app.
- Know which pricing tiers you should choose in a given scenario. Key differences.
- Know the scaling limits between the standard, premium and isolated tiers.
- Be familiar with the features of web apps. Why, when and how you would use them.
- Know how to work with deployment slots.
- Able to configure diagnostic logging for Windows and Linux.
- Know the different types of logs and what information is in each of them.
- How to write to the log from application code.
- Configure app settings for the web app.
- Know how to store something in Azure Key Vault and reference it in app settings.
- How to configure deployment options for the web app.
- Know the core app settings for the app service.
- Platform settings - understand why and when you would set the options listed.
- Be comfortable with all of the configuration options for TLS, SSL and when you would use each of them. e.g. when you would use the key vault to store over uploading it directly to the service.
- Know the basics of certificates e.g. when to use a public or private certificate.
- Scenarios that are best resolved by autoscaling.
- Know the limits of autoscaling and various service tiers.

## Create an Azure App Service Web App (1)

It is possible to share App service Plan across multiple web apps however, for improved performance Microsoft recommends isolating the web apps into seperate app service plans.

Free and shared tiers cannot be scaled they're based on CPU minutes. Other tiers can scale to multiple instances. Web apps support nearly instant scaling and autoscaling.

Web apps can be used with Azure Functions and to create web app, RESTful API, mobile backend.

## Create an Azure App Service Web App (2)

### Scaling

Scaling happens at the app service plan level. All the different web apps in the same plan scale together.

All web apps within the same app service plan will share and compete for resources within the plan.

Scaling + resources = why you may wish to seperate web apps into different service plans.

## Create an Azure App Service Web App (3)

### Web App Features

|Feature| Description|
|-----|-----|
|Built in auto-scale support| Supports scale in and out. Depending on the useage of the web app, the resources of the underlying machine that are hosting your web app can be scaled up/down manually in the basic tier up to 3 compute machines. **Auto-scaling only works in the standard tier is managed based on schedules and rules**, **automatic scaling works in Premium V2 and V3 tiers. The platform manages the scale out and in based on HTTP traffic.**|
|Continuous integration/ deployment support| Azure DevOps, GitHub, BitBucker, FTP, or a local Git repo on your dev machine. Can configure manual source deployments from other sources, such as OneDrive|
|Deployment slots| You can add deployment slots to a Web App|
|App service on Linux| Hosts web apps natively on Linux for supported application stacks. Run custom Linux containers (also known as Web App for Containers) **You can't mix Windows and Linux apps in the same service plan**.

When to consider autoscaling?

- Provide elasticity
- Improves availability and fault tolerance
- Works by adding/removing web servers
- Autoscaling not good for handling long-term growth
- Number of instances of a service is also a factor.

To enable autoscaling navigate to the app service plan, select Scale out - you can then edit auto generated scale conditions or create your own scale conditions.
Metric based scale conditions contain one or more scale rules. Add a rule link to add your own custom rules.
Track when autoscaling has occurred through the Run history chart.

**Important:** if there are multiple rules that trigger at the same time, the rule that specifies nore resources will be the rule that wins.

A P1-B3 App Service Plan can scale to a maximum of 30 instances.
An app service enviornment allows for auto-scaling up to 100 instances.

Best Practice for autoscaling:

1) max and min values are different and have an adequate margin between them. Used to prevent flapping where a service is scaling up and down rapidly.
2) choose the appropriate statistic for your diagnostic metric.
3) choose threshold carefully.
4) remember considerations for scaling when multiple rules are configured in a profile.
5) always select a safe default instance count.
6) configure autoscale notifications.

## Enable Diagnostic Logging

- Enable application logging (Windows)
- Enable application logging (Linux/Container)
- Enable web server logging
- Add log messages in code
- Stream logs
- Access log files

Steps to enable applicating lobbying is different for Windows, Linux and/or containers.

## Configure application settings (1)

In App Service, **app settings are passed as environment variables** to the application code.

### Adding and editing settings

- Go to App Service, Configuration.
- To add a new app setting, click New **Application Setting**.
- To add or edit app settings in bulk, click the **Advanced edit** button.

### Configure connection strings

Follows same principle as other app settings, they can also be tied to deployment slots.

**Important**: These set environment variables in the app so they override settings in the web config or app settings JSON file. You need to understand how these settings work with deployment slots. E.g. If you mark the setting as a deployment slot setting, it will stay with the deployment slot and keeping secrets like database password string is better kept in the Azure Key Vault and referenced from App settings.

Configure multiple app settings an example following format that illustrates setting connection strings:

`

    [
        {
            "name": "name-1",
            "value": "conn-string-1",
            "type": "SQLServer",
            "slotSetting": false
        },
        {
            "name": "name-2",
            "value": "conn-string-2",
            "type": "PostgreSQL",
            "slotSetting": false
        },
    ]

`

## Deploy to App Service

App service supports automated (Azure DevOps, GitHub, BitBucket) and manual deployments (Git, CLI, ZipDeploy, FTP/S).

CLI: You can use `az webapp up` which will package and deploy a web app - even if you haven't created one yet.
ZipDeploy: Use `curl` or similar HTTP utility to send a zip of the application files to the app service.

Use deployments slots when deploying a new production build. <- Best practice>

Staging slots: standard and higher tiers of App service!

## Configure settings in App Service (1)

|Setting|Description|
|-------|-----------|
|Stack settings| The software stack to run the app, including language and SDK versions.|
|Platform settings| Configure settings for the hosting platform: - Bitness (32/36 - WebSocket Protocol - Always On - Managed pipeline version - HTTP version - ARR affinity) |
| Debugging| Enable remote debugging for ASP.NET, ASP.NET Core, or Node.js apps.|
| Incoming client certificates| Require client certificates in mutual authentication. TLS mutual authentication is used to restrict access to your app by enabling different types of authentication for it.|

Platform settings, enabling the webSocket protocol is used to support things like signal R or socket IO.
Always on is required for continuos or scheduled web jobs to function.

### Configure settings in App Service (2): Configuring SSL

To configure SSL, you can:

- Buy and import App Service certificate
- Import a certificate from Key Vault
- Upload a private/public certificate
- Renew an expiring/uploaded/App Service certificate
- Renew a certificate imported from Key Vault
- Manage App Service certificates
- Automate with scripts
