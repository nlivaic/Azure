## Description
* Azure Account: Your overall account to start you Azure journey. Also your billing account
* Azure AD: Your directory for authentication and authorization
* Azure Subscription: The container where your created resources are created. Billing is per subscription. Azure subscription has a trust relationship with Azure AD instance (multiple subscriptions can have the same Azure AD). You can also set specific Azure policies on subscription level. Using multiple subscriptions can be convenient for administrative/billing use, or for example sandbox and test vs production environment. I don't recommend a subscription per department except when for example developers having their separate subscriptions.
* Azure Resource Groups: A logical group of resources belonging to the same application environment and lifecycle.

## Structure

Azure Account
    Subscription #1
        Resource Group #1
        Resource Group #2
    Subscription #2
        Resource Group #3
        Resource Group #4
