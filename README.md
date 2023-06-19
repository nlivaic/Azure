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
        Resource Group #1
        Resource Group #2
    Subscription #2
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

## Summary

* If you want to work in Azure you need an Azure account. To create an Azure account you need an active email id.
* If you want to add people/employees or machines/devices who would be part of your IT infrastructure you need a tenant/AAD. You get one tenant/AAD by default when you create an Azure account. You can create more if you require for any kind of logical separation. AAD service is a global service spanning across all locations in Azure which manages all of our AAD instances. AAD is also known as Azure Active Directory, AAD, an Azure AD instance, an AAD Instance, an Azure AD Tenant, an AAD tenant, simply tenant or an organization, etc. They all mean the same. Therefore:
    Organization == Tenant == Azure Active Directory
* If you require logical separation of billing for users of your Azure account then you need multiple subscriptions. You get one subscription by default when you create a new Azure account. Subscription can be of four types, as mentioned earlier.
* If you want to enable the users to do things then you issue license(s) e.g. license to be able to create VM or Azure app service. Also remember that license and Role Based Access Control (RBAC) are not same although both enable you to do things in Azure portal. But they've different nuances which you can explore on your own.

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
    3. `az account list`
        * Choose any subscription on ppropriate tenant and copy the identifier (`id`, not `tenantId`. Below command is not interested in the subscription itself but rather the tenant that the subscription belongs to.
    4. `az account set -s 2ad5208c-a7ff-462c-b76a-1787ea8c0978` - to switch to dev tenant.
    5. `az login`
    6. Now you can access Key Vault that is in a different tenant from where your email is registered.
  
* **Please note** - if at some point you want to run another API locally, but the Key Vault (or some other resource) is in a different tenant than what you are currently logged into, you will have to perform above steps to change the subscription again,


