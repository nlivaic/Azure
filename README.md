# About Azure
* Azure Account:
    * Your overall account to start you Azure journey. Also your billing account.
    * All the concepts below exist inside Azure and are available for creation and management once you create an Azure Account.
* Azure AD:
    * A service for authentication and authorization.
    * Tenants:
        * Azure AD service can have multiple Azure Active Directories (AAD, a.k.a. tenant).
        * Every tenant is linked to a single Azure AD instance.
        * A tenant is associated with a single identity (person, company, or organization) and can own one or several **subscriptions**. You can think of a tenant as a way to manage the employees of the company.
        * Multiple subscriptions can share the Azure AD tenant.
        * **You get one tenant/AAD by default when you create an Azure account.**
* Azure Subscription:
    * The container where your created resources are created.
    * Billing is per subscription.
    * Azure subscription has a trust relationship with Azure AD instance. Each subscription belongs to one tenant. Multiple subscriptions can belong to the same Azure AD tenant. You can also set specific Azure policies on subscription level. Using multiple subscriptions can be convenient for administrative/billing use, or for example sandbox and test vs production environment. It's not recommended to do a subscription per department except when for example developers having their separate subscriptions.
    * **You get one subscription by default when you create a new Azure account.**
    * Subscription can be of four types as per below list:
        * Free
        * Pay-as-you-go
        * Enterprise agreement
        * Cloud Solution Provider
* Azure Resource Groups: A logical group of resources belonging to the same application environment and lifecycle.

# Structure

```
Azure Tenant
    Subscription #1
        Billing #1
        Resource Group #1
        Resource Group #2
    Subscription #2
        Billing #2
        Resource Group #3
        Resource Group #4
```

## Examples

### Example 1:
* Contoso decides to have a tenant with 2 subscriptions:
    * one subscription for the Prod department with Credit Card A
    * one subscription for the Dev department with Credit Card B
    * (but could also be the same Credit Card as the one of another subscription)
* In this example, the two departments share the same Azure AD database. However, resources are isolated between departments, and budgets can be separated too.

### Example 2:
* A holding company decides to have 2 tenants:
    * one tenant for subsidiary Contoso with one subscription for Dev and Prod
    * one tenant for subsidiary Fabrikam with one subscription for Dev and another subscription for Prod
* In this example, both companies have a different Azure AD database.

### Example 3:

* You have a tenant for your personal training. In this tenant, you can have:
    * one free Azure subscription (linked to a credit card but not charged, and can be converted to a Pay-As-You-Go subscription after the free trial)
    * one or several Pay-As-You-Go subscriptions (linked to different credit cards)
    * one or several Azure Pass Sponsorship subscriptions, not linked to any credit card because these subscriptions are obtained during Microsoft trainings
    * one Visual Studio subscription (linked to a credit card) and with different quotas (of free resources) than the free subscription

## Short Summary

* If you want to work in Azure you need an Azure account. To create an Azure account you need an active email id.
* If you want to add people/employees or machines/devices who would be part of your IT infrastructure you need a tenant/AAD. You get one tenant/AAD by default when you create an Azure account. You can create more if you require for any kind of logical separation. AAD service is a global service spanning across all locations in Azure which manages all of our AAD instances. AAD is also known as Azure Active Directory, AAD, an Azure AD instance, an AAD Instance, an Azure AD Tenant, an AAD tenant, simply tenant or an organization, etc. They all mean the same. Therefore:
    Organization == Tenant == Azure Active Directory
* If you require logical separation of billing for users of your Azure account then you need multiple subscriptions. You get one subscription by default when you create a new Azure account. Subscription can be of four types, as mentioned earlier.
* If you want to enable the users to do things then you issue license(s) e.g. license to be able to create VM or Azure app service. Also remember that license and Role Based Access Control (RBAC) are not same although both enable you to do things in Azure portal. But they've different nuances which you can explore on your own.

# Azure AD

## AAD vs Graph API

* AAD is an implementation of OAuth2 (and other things). IT IS NOT A DIRECTORY. Active Directory (AD) is an identity provider and a directory service, but AAD isn't - it lacks the directory capabilities.
* Graph API is similar to LDAP. Handles stuff like:
    * Directory services - stores credentials and information about each user. Please note "user" might also be a printer, a computer, a service etc...
    * When you access e.g. Sharepoint it check for licences.
    * Getting an access token.

### In-depth description of Graph API

* Microsoft has lots and lots of services: Azure (which itself includes a whole bunch of services), Exchange Online (mail, calendar), SharePoint Online (files, sites), Teams (chats, meetings, channels), Entra (identity, access), Dynamics (sales, accounting), Fabric (data analytics), Intune (device management), etc. Most of those services expose APIs that an application or automation script you build can interact with.
* Microsoft Graph is an API that sits in front of many (though not all) of Microsoft services' individual APIs, providing a single endpoint to access all of them. This simplifies things for you, so that you don't have to learn how to use as many different APIs as there are services at Microsoft. It also enables things that wouldn't otherwise be possible, like aggregating together data from multiple services in a single API request.
* Today, Microsoft Graph includes APIs for most Microsoft 365 services (from my list earlier, this includes Exchange, SharePoint, Teams, Entra, and Intune, though there are many other not listed). If you want to build an app or automation script to create a user (Entra ID), send an email (Exchange), download a file (SharePoint), create a team (Teams), or list device management policies (Intune), you use Microsoft Graph.
* That said, many Microsoft APIs are not behind Microsoft Graph. Notably (for this subreddit), APIs for Azure services are not behind Microsoft Graph. Most APIs for management of Azure resources (e.g. managing subscriptions, resource groups, and resources like VMs, storage accounts, etc.), are all behind the single Azure Resource Management API. In addition, most Azure service have their own APIs for data access (see list).
* If you work primarily with Azure, you will most likely be exposed to Microsoft Graph when it comes to things related to identities: users, groups, and app identities. This is because Azure, like many Microsoft services, uses Microsoft Entra ID (a service formerly known as "Azure Active Directory") as it's single source for all identity information. Every user, group, or service principal who has access to an Azure resource is created and managed in Entra ID.

## Identity Issuers

* There are several possible identity issuers for users in AAD. Frequent ones are below, but there are [others](https://learn.microsoft.com/en-us/azure/active-directory/external-identities/user-properties#identities):
  * `{TennantName}.onmicrosoft.com`: we get this identity issuer when we add any member to Azure AD.
  * `ExternalAzureAD` : This identity issuer we get when we federate a user from one tenant to another tenant. Its user type is guest users.
  * `MicrosoftAccount`: This identity user we get when we send invite to any user (any email it could be) and it should be accepting the invitation from user side.

## User Type (Member vs Guest)

* [Source](https://learn.microsoft.com/en-us/azure/active-directory/external-identities/user-properties#user-type)
* This property indicates the relationship of the user to the host tenancy. This property can have two values:
  * Member: This value indicates an employee of the host organization and a user in the organization's payroll. For example, this user expects to have access to internal-only sites. This user isn't considered an external collaborator.
  * Guest: This value indicates a user who isn't considered internal to the company, such as an external collaborator, partner, or customer. Such a user isn't expected to receive a CEO's internal memo or receive company benefits, for example.

## Apps and Service Principals

### Service Principals

* Good overview [here ](https://app.pluralsight.com/course-player?clipId=ffe30395-69f4-4f73-832c-521c0630fb21).
* Security entity, paired with an application in AAD. Whenever you create an application in AAD, a service principal is created along with it.
* However it can also be created on its own, but there will still be an application paired with it. We can even login as service principal, for automation scenarios:

    az ad sp create-for-rbac -n <service_principal_name>
    az login --service-principal -u <service_principal_name> -p <password> --tenant <tenant_name>

* It isn't uncommon to have a service principal without an app.

* You cannot see the service principal directly in Azure.
    * You can go to `Groups` and try to assign users, you will find the service principal.
    * Another way to see the service principal is to go to the registered app and you can see the service principal there (under `Overview`).
    * `az ad sp list --query []."displayName"`
    * `az ad sp show --id "<application_client_id>"` (this works because application and service principal are paired up)

* Delete service principal: `az ad sp delete --id "<application_client_id>"`. This will delete the app as well.

### App Registrations

* Any application that wants to use AAD to authenticate users on its behalf has to be registered in App Registrations blade.
* It is a different entity from the service principal. When you create an app in AAD, the app gets permissions. For these permissions to make sense, a service principal must be created so it can be assigned these permissions.
* Different ways to think about application types:
  * Single tenant vs multitenant:
    * Will the application be used by users from only one AAD tenant (e.g. internal company app) or from multiple tenants?
    * This is important because different endpoints are used.
  * Native vs Web:
    * Native apps are public clients: mobile apps, SPA apps, WPF apps.
    * Web apps are confidential clients: web applications (Razor), APIs. They can safely store client secret on their backend.
* Registering apps:
  * Portal
  * Azure CLI
  * PowerShell
  * Graph API
* Example: for Azure DevOps to be able to deploy to Azure it must be registered here (`AzureDevOpsTestTemplate1`). Just for reference, this specific example is described in more details [here](https://www.devcurry.com/2019/08/service-connection-from-azure-devops-to.html). The previous link describes the process nicely, but in case it is down try [this](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/connect-to-azure?view=azure-devops#create-an-azure-resource-manager-service-connection-with-an-existing-service-principal) one.

### Certificates & Secrets

* Every client application needs a secret in order to identify itself to AAD, so as to be issued a token.
* Service principal's password is also the app's secret. So if you created a service principal directly (see under [Service Principals](#service-principals)), the secret that was generated for that service principal becomes client secret of the associated app.
* Do not allow for secrets that never expire! This also means we need a process for renewing password, but that's another story.
* Certificates - TBD

## Managed Identity

### Problem

* For an API running locally to talk to Key Vault it must have a token. This is a token that represents the API, not the user! To get this token Client Credentials flow must be executed (against AAD). However, to do this we must store client id and client secret somewhere and this opens up the potential for a leak.

![Typical token retrieval using Client Credentials flow](https://github.com/nlivaic/Azure/assets/26722936/6a1767db-91b5-4bc6-bdcf-3798beef0e8a)

### Solution - Managed Identity

* Instead of talking to AAD with your client id and secret, we can say the API has a Managed Identity. This means our API, when running inside Azure, will talk to a separate endpoint (belonging to AAD? Not sure.) that will grant a token.
* Trust relationship is established by:
    * The fact our API is running inside Azure (App Service? Or AAD?).
    * We have selected (in App Service) for our API to have a Managed Identity.
    * Telling Key Vault (or some other resource we want to access) what the Managed Identity identifier is (c/p from previous step).

![Token retrieval using Managed Identity](https://github.com/nlivaic/Azure/assets/26722936/74aa4f1f-93a2-4b23-b629-5f7785087b44)

#### Running an API locally
* Token provider will try to get the token in several ways (not sure of the precedence):
    1. Managed Identity
    2. Visual Studio
    3. Local login (you have to perform az login before)
* We can't make Managed Identity work when running the API locally.
*  We can obtain the token by:
    1. Logging in our account from inside Visual Studio locally, go to following: Visual Studio -> Options -> Azure Service Authentication
    2. Log in with a user belonging to the same tenant as the RG the KeyVault is in. Then execute a `az login`. Now you can run your API locally.
* The above might be a problem if our email is registered with production tenant. In that case we have to determine what other subscriptions on production tenant our email belongs to. We do this by:

  1.logging in from Visual Studio (see previous point).
  2. Open a CLI.
  3. `az account list -o table`
    * Choose any subscription on ppropriate tenant and copy the identifier (`id`, not `tenantId`. Below command is not interested in the subscription itself but rather the tenant that the subscription belongs to.
  4. `az account set -s 2ad5208c-a7ff-462c-b76a-1787ea8c0978` - to switch to dev tenant.
  5. `az login`
  6. Now you can access Key Vault that is in a different tenant from where your email is registered.

# Bicep

## Getting Ready

```
choco install azure-cli -y
choco install bicep -y
Install-Module Az.Account
Install-Module Az.Resources
```

## Creating a bicep file

```
bicep decompile --file ./<location_here>
```

## Logging in

* Only if you want to deploy from your local machine. Run this yourself from the CLI.

```
az login
az account set -s <target_subscription>
```

## Deploying

* You will probably want to put this into a script. You can also run these yourself from the CLI to test stuff out.

```
az group create --name $RG --location $location
az deployment group create --resource-group $RG --template-file ./arm-app-service-plan.bicep --what-if
az resource list --resource-group $RG -o table
```

* `--what-if` allows you to run the deployment and see the outcome without actually provisioning resources on Azure. This is great for testing.
* **Please note** - if at some point you want to run another API locally, but the Key Vault (or some other resource) is in a different tenant than what you are currently logged into, you will have to perform above steps to change the subscription again,


