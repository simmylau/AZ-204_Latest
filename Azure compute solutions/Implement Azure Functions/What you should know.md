# Implement Azure Functions

[\Preparing for AZ-204 - Develop Azure compute solutions (1 of 5)](https://learn.microsoft.com/en-us/shows/exam-readiness-zone/preparing-for-az-204-develop-azure-compute-solutions-1-of-5)

## You should know how to do the following

- Comfortable with creating and implementing functions
- Comfortable developing the code and the configuration board
- Able to configure the function app which will host the function.
- Understand configuration options.
- How to configure various bindings and triggers for your function.
- Should be able to configure different triggers and different bindings for different configurations and services.

![Function app](<Images/Screenshot 2023-09-25 150825.png>)

## Explore Azure Functions development (1)

Function includes:

- code
- some config, the function.json file.

The function.json file defines the function's trigger, bindings, and other config settings. Every function has one and only one trigger. The runtime uses this config file to determine the events to monitor and how to pass data into and return data from a function execution.

![Alt text](<Images/Screenshot 2023-09-25 153149.png>)

## Explore Azure Functions development (2)

A function app provides an execution context in Azure in which the function runs.

- It is the unit of deployment and management for functions.
- A function app is comprised of one or more individual functions that are managed, deployed, and scaled together.
- All of the functions in a function app share the same pricing plan, deployment method, and runtime version.
- Function app is a way to organise and collectively manage functions.

## Create triggers and bindings

- Triggers cause the function to run.
- Triggers define how a function is invoked.
- A function can only have ONE trigger.
- Binding to a function is a way of declaratively connecting to another resource to the function; bindings may be connected as input bindings, output bindings, or both.
- Can mix and match different bindings to suit your needs.
- Triggers and bindings let you avoid hardcoding access to other services.

Triggers and bindings are defined differently depending on the programming language.

|Development language|Method|
|------|----------|
|C# class library| decorating methods and parameters with C# attributes|
|Java| decorating methods and parameters with Java annotations|
|JavaScript/PowerShell/Python/TypeScript| updating function.json schema|

### Binding direction

All triggers and bindings have a direction property in the function.json file:

- For triggers, direction is always IN.
- Input and output bindings use in and out.
- Some bindings support a special direction *inout*. If you use *inout*, only the **Advanced editor** is available via the **integrate** tab in the portal.

When you use attributes in a class library to configure triggers and bindings, the direction is provided in an attribute constructor or inferred from the parameter type.
![Example](<Images/Screenshot 2023-09-25 155257.png>)

JavaScript Example of the function code you would align with the configuration from above

![JavaScript Example of the function code you would align with the configuration from above.](<Images/Screenshot 2023-09-25 155341.png>)
