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
* Azure Subscription:
    * The container where your created resources are created.
    * Billing is per subscription.
    * Azure subscription has a trust relationship with Azure AD instance. Each subscription belongs to one tenant. Multiple subscriptions can belong to the same Azure AD tenant. You can also set specific Azure policies on subscription level. Using multiple subscriptions can be convenient for administrative/billing use, or for example sandbox and test vs production environment. It's not recommended to do a subscription per department except when for example developers having their separate subscriptions.
* Azure Resource Groups: A logical group of resources belonging to the same application environment and lifecycle.

## Structure

```
Azure Account
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
