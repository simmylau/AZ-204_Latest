# Authentication and authorisation in the app service

Built in authentication and authorisation.

- allows you to integrate various auth capabilities without needing to integrate them yourself.
- built into the platform.
- integrate with multiple log in providers.

App service uses Federated identity meaning a 3rd party provider will manage user identities and authentication flows for you.

A federated identity in information technology is the means of linking a person's electronic identity and attributes, stored across multiple distinct identity management systems. Federated identity is related to single sign-on, in which a user's single authentication ticket, or token, is trusted across multiple IT systems or even organizations.

These providers are available by default - Microsoft Identity Platform, Facebook, Google, Twitter (?), Any OpenID connect provider, GitHub.

(Get documents from here for the remaining <https://learn.microsoft.com/en-us/training/modules/introduction-to-azure-app-service/5-authentication-authorization-app-service>)

|Provider| Sign-In Endpoint| How-To guidance|
|-----|-----|-----|
|Microsoft Identity Platform|`/.auth/login/aad`|[App Service Microsoft identity platform login](https://learn.microsoft.com/en-us/azure/app-service/configure-authentication-provider-aad)|
|Facebook|`/.auth/login/facebook`|[App Service Facebook login](https://learn.microsoft.com/en-us/azure/app-service/configure-authentication-provider-facebook)|
|Google|`/.auth/login/google`|-----|
|Twitter (?)|`/.auth/login/twitter`|-----|
|OpenID connect provider|`/.auth/login/<providerName>`|-----|
|GitHub|`/.auth/login/github`|-----|

## How it works

An authentication and authorisation module will run in the same sandbox as the application code. When enabled, incoming HTTP requests will pass through before being handled by your application code. Module will handle:

- authetication using the specified provider/s.
- validates, stores and refreshes OAuth tokens issued by the configured identity provider/s.
- manages the authenticated session
- injects identity info into the HTTP request headers

! Module runs seperately from the application code and can be configured using Azure Resource Manager settings or a config file. No SDKs, specific programming languages, or changes to your application code are required.

**Note**
In Linux and containers the authentication and authorization module runs in a separate container, isolated from your application code. Because it does not run in-process, no direct integration with specific language frameworks is possible.

## Authentication flow

**Signing in *without* providers SDK:**

- application delegates federated sign in to app service.
- most common in browser apps, presents providers log in page to the user.
- server code manages sign in process called *server directed flow/ server flow*.

**Signing in *with* providers SDK:**:

- app signs in user manually and then submits the auth token to app service for validation.
- used for browserless apps. Where a sign in page cannot be presented to the user.
- application code manages the sign in to it's called *client-directed flow/client flow*
- applies to REST APIs, Azure Functions, JavaScript browser clients, and native mobile apps that sign users in using the provider's SDK.

![steps of the authentication flow](<../../Images/Screenshot 2023-11-04 091330.png>)

For client browsers, App Service can automatically direct all unauthenticated users to /.auth/login/<provider>. You can also present users with one or more /.auth/login/<provider> links to sign in to your app using their provider of choice.

## Authorisation behaviour

Configure behaviour based on if a request is authenticated or not.

**Allow unauthenticated requests:** defers authorisation to you app code. For authenticated requests, app service will pass along the info in the HTTP headers.

- More flexibility in handling anon requests and providing multiple sign-in provider auth options.

**Require authentication:** rejects any requests that are not authenticated. This can redirect user to sign in page.  If the anonymous request comes from a native mobile app, the returned response is an HTTP 401 Unauthorized. You can also configure the rejection to be an HTTP 401 Unauthorized or HTTP 403 Forbidden for all requests.

**Caution**
Restricting access in this way applies to all calls to your app, which may not be desirable for apps wanting a publicly available home page, as in many single-page applications.

## Token store

When you set up authentication the app service will provide built-in token store, a repo of tokens associated to the users of your app. Available immediately when you enable authentication with any provider.

## Logging and tracing

If logs are enabled they will be collected directly in the log files.