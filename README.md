## About Azure
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

## Structure

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
