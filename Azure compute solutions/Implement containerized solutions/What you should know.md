# Implement containerized solutions

[Preparing for AZ-204 - Develop Azure compute solutions (1 of 5)](https://learn.microsoft.com/en-us/shows/exam-readiness-zone/preparing-for-az-204-develop-azure-compute-solutions-1-of-5)

## Create and manage container images for solutions

ACR Tasks is a suite of features within Azure Container Registry. It provides cloud-based container image building platforms including Linux, Windows, and ARM, and can automate OS and framework patching for Docker containers.

## You should know how to do the following task scenarios

- Quick tasks, which are for simple commands such as build and push. To push a single container image to a container registry on-demand completely on Azure without needing a local docker engine installation.

- Know how to configure automatically triggered tasks, such as trigger task on source code update, which will trigger container image build or a multi-step task when source code is updated in our repository. Trigger on base image update, with this you can set up ACR Tasks to track a dependency on a base image when it builds an application image. Then if the base image is updated ACR Tasks can automatically build any application images build from that base image and schedule a task where you can schedule the task by setting up one or more timer triggers when you create or update the task.

- Multi-step task which will extend the single image, build and push capabilities of ACR Tasks with multi-step, multi-container based workloads. For this you should be familiar with how to configure the YAML file to define the tasks.

For all of these you should also be familiar with using **AZ ACR Tasks command from the CLI**

## Discover the Azure Container Registry (1)

Use the ACR service with existing container development and deployment pipelines or use ACR Tasks to build container images in Azure.

Use cases:
Pull images from an Azure container registry to various deployment targets:

1) **Scalable orchestration systems** that manage containerised application across clusters of hosts.
2) **Azure Services** that support building and running applications at scale.

- You should know the use cases where ACR can be used such as with scalable orchestration systems, including Kubernetes, DCOS, Docker Swarm and more, and within Azure services which includes Azure Kubernetes Service (AKS), App Service, Azure Batch, Service Fabric, and others.

- Developers can push to the container registry as part of the container development workflow using ACR.

TIP: Think about the way behind the choices that you need to make. An example of this is in choosing Azure Container Registry service tiers.

- You should be familiar with the 3 tiers and why you would choose one over another.

Standard > storage and image throughput than Basic.
Premium + features e.g. geo-replication, it supports content trust for image tag signing and it supports private link with private endpoints.

## Discover the Azure Container Registry (2)

Supported images and artifacts:

1) Grouped in a repo, each image is a read-only snapshot of a Docker compatitable container.
2) Azure Container Registries can include windows and Linux images.
3) ACR also stores Helm charts and images build to the Open Container Initiative (OCI) Image Format Specification.

Azure Container Registry Tasks:

- Use ACR Tasks to streamline building, testing, pushing, and deploying images in Azure.

- You should know how to configure the supported images and artifacts and Azure Container Registry Tasks based on your requirement.
- How to manage images with the ACR. Know how to create a repository in the registry but also how to manage the artifacts in the repository.
- How to use ACR Tasks to manage you images and artifacts.

## Explore Azure Container Instances (1)

- Understand why you would use ACI and what are its capabilities.
- Why use ACI and when you shouldn't. e.g. understand when to use AKS instead of ACI.
- How to work with ACI

|Feature|Description|
|---------|------------|
|Fast startup times| Containers can start in seconds without the needs to provision and mange VMs|
| Public IP connectivity and DNS name| Containers can be directly exposed to the internet with an IP address and fully qualified domain name|
|Hypervisor-level security| Container applications are as isolated in a container as they would be in VM|
|Custom sizes| Container nodes can be scaled dynamically to match actual resource demands for the application|
|Persistent storage| Container support direct mounting of Azure File shares|
|Linux and Window containers| Same API is used to schedule both Linux and Windows containers|
|Co-scheduled groups| Container Instances supports scheduling of multi-container groups that share host machine resources|
|Virtual network deployment| Container instances can be deployed into an Azure Virtual Network|

For deploying an ARM template is recommended when you need to deploy additional azure services. YAML files are recommended when your deployment only requires ACI since the file format is more concise than an ARM template.

For resource allocation container groups combined resources, for example if you create a container group of two instances each requesting one CPU than the container group will allocate 2 CPUs.

For networking, each container group needs to have its own port exposed on the shared IP address. So they each have their own port and IP address/

For storage, the supported volumes include Azure File Shares, secrets, an empty directory and a cloned git repo.

- specify external volumes to mount within a container group
- map those volumes into specific paths within the individual containers in a group

Multi-groups are useful in cases where you want to divide a single functional task into a small number of container images.

*Common scenarios for multi-container groups* can include a container serving a web application and a container pulling the latest content from source control.

An application container and a logging container, where the logging container collects the logs and metrics output by the main application and writes them to long term storage.

Application container and a monitoring container where the monitoring container periodically makes a request to the application container to ensure that it's running and responding correctly and raises an alert if it's not.

Or a frontend container and backend container where the frontend serves a web application and the backend running a service to retrieve data.

## Create solutions by using Azure Container Apps

Azure Container Apps helps to execute application code packaged in any container and is open to runtime or programming model.

You can use Azure Container Apps to:

1) Deploy API endpoints
2) Host background processing applications
3) Handle event-driven processing
4) Run microservices

**Key advantage over ACI is the ability to scale.**

Build applications on Azure Container Apps to dynamically scale based on:

1) HTTP traffic
2) Event-driven processing
3) CPU or memory load
4) Any KEDA-supported scaler

Using Azure Container Apps you can run microservices and containerised applications on a serverless platform.

- They have more functionality that ACI, and are simpler to implement than AKS.

- You should be familiar with the different configuration options for Azure Container Apps for the Azure exam.
