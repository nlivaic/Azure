# About Azure

## Overall Structure

### Resources

* Basic building block in Azure.
* Usually assigned to a region.
* Must have a name.
* Most resources have a cost.

### Resource Groups

* Logical grouping of resources.
* Resources can be in any region.
* All resources must be in a resource group.
* Recommendation is to group resources with same lifecycle and access control needs.
* Characteristics:
  * Each resource can be in only one resource group.
  * Resources within a resource group can be in different regions.
  * Resources can communicate across resource groups (resource group is not an access boundary).
  * Resources can easily be moved between resource groups.
  * You can assign access control to a resource group and everything in it.
  * A resource group itself has a region as it needs to store metadata about its resources.
* Not a communication boundary. Resources from one RG can communicate with resources in another RG.

### Subscriptions

* High-level billing. Useful if we want different organizational units to manage their own billing.
* Access control.
* Resource management.

### Management Groups

* Optional. Suitable for larger organization to organize subscriptions.
* Allow better access, policies and compliance management across multiple subscriptions.
* Central management for multiple subscriptions.
* A management group can have additional management groups underneath itself.
* Policies applied to management group are applied to all child objects.

## Additional Notes

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
    * The container where your resources are created.
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

* The point of this structure is to help organizations manage and structure their resources, enforce policies and manage access control.

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

# Architecture

## Regions

* A region is a **set of datacenters** deployed withing a **latency-defined perimeter** and connected throught a dedicated **regional low-latency network**.
* Geographically distributed groups of DCs.
* Usually two or more DCs, not too far from each other (there might be some exceptions to this).
* Each DC is connected using high-speed connection.
* Each DC within a region is completely independent (power, cooling, networking).
* Placed world-wide.
* Regions are connected between themselves using Microsoft's global fiber network. Microsoft global network does not traverse public network, it is completely private.
  * Any traffic between DCs, within Microsoft Azure or between Microsoft services such as Virtual Machines, Microsoft 365, Xbox, SQL DBs, Storage goes over Microsoft global network.
* How to select a region:
  * Closest to your user - better performance
  * Features - some features are not available in all regions (especially newest features)
  * Price - not all features have the same price across all regions
* Region pairs
   * Share a special relationship for enhanced resilience and disaster recovery.
   * Extra layer of failover protection.
   * Two closely linked regions for redundancy.
   * Only one region gets updated at any one time.
   * Usually within the same geographical area.
   * If possible, the region is linked with another region in the same country but that region has to be at least 300 miles apart.
   * Replication - some PaaS service might have automatic replication between region pairs.
* Sovereign regions:
  * Same as ordinary region.
  * Isolated regions ensuring government compliance within specific jurisdictions.
  * Not part of global Azure public cloud and are not part of Microsoft's global private fiber network. You must be approved to join.
  * Not all services are available.
  * Focus is on security and compliance.
  * Only two at the moment:
    * Azure US government - you must be part of US gvt to use these.
    * Azure China - there is no Azure public cloud in China, so if you want to use Azure in China you must use one of Azure China sovereign regions. Operated by 3rd party. All data must stay in those data centers. Also requires your business presence in China.

## Availability Zones

* Physical location:
  * Each AZ is a physical location within a region. There is at least one DC per AZ.
  * Several DCs inside one AZ do **NOT** share a building, but are very close by (several kms).
  * This distance protects against local building-level issues (e.g., fire, cooling failure) but not against large regional events (e.g., floods, earthquakes).
* Independence:
  * Each AZ has independent powering, cooling and networking, so no AZ depends on another AZ for those (i.e. they share infrastructure only within that zone).
  * Allows high availability.
* Minimum of 3 AZs per region (but this is not available in all regions).
* Latency within AZ is typically <1ms (rount trip latency 2ms), so it is treated as one logical unit.
* Why use AZs?
  * Provide fault isolation within a region.
  * Services are replicated across multiple zones (e.g. data in Azure Storage).
  * If one zone goes down, service is still operational.
  * Adds redundancy to your application.
* How to use AZs?
  * Zonal services are services allowing us to choose an AZ within the region. E.g. VMs allow choosing what AZ we deploy to.
  * Zone-redundant services are usually Azure PaaS services that allow automatically replicating across zones (this is usually an optional feature). E.g. Zone-redundant storage (ZRS) account. This has additional costs if the service has a single-zone option.

# Azure Compute

* On-demand computing service providing resources such as disks, processors, memory, networking and operating systems.
* Services:
  * VMs (IaaS)
  * Azure App Service (PaaS)
  * Function Apps (PaaS)
  * Azure Container Instances (PaaS)
  * Azure Kubernetes Service (PaaS)
  
## VMs

* IaaS
* Virtualized server hosted on underlying hypervisor.
* User has full control over the virtualized server and can run almost any application.
* Linux or Windows OS
* Components:
  * CPU/Memory
  * Storage
  * Networking
  * OS
  * Network Controls (Firewall)
* Every VM must be assigned to a Virtual Network (VNet). Each VM is assigned a public and private IP address (these are created as a separate resource).
* Configuration:
  * VM Size = preconfigured CPU/RAM combinations, optionally GPU and Disk sizes.
  * VM Sizes are grouped into series for different use cases.
    * Prefix - always `Standard`
    * Different series numbers denote how many CPUs it has.
    * Letters denote a family:
      * `A` - budget
      * `B` - burst. A good option for general-purpose machines that are not running at full capacity the entire time.
      * `D` - default general-purpose
      * `DC` - secure/confidential variant of D-family workloads
    * There are other elements to a name that also follow some known patterns (e.g. `C` for compute-optimized, `S` for premium storage capability)
* Pricing:
  * CPU/RAM:
    * Priced per CPU/RAM by the minute. More compute results in higher costs.
    * Priced by the minute. If you stop (deallocate) VM you are not charged for compute.
  * Disks:
    * Charged separately even if VM is deallocated.
  * OS:
    * Licensed OSs (Windows, Red Hat).
    * Pay-as-you-go license is charged same as compute.
  * Public IP Addresses:
    * Charged by the hour.
    * Pricing model depends on the SKU:
      * Dynamic: no charge when VM is deallocated.
      * Static: charged regarless of VM state.
* Scale Sets:
  * Centrally managed creation of auto-scaled identical VMs based on load, for availability purposes.
  * Paired with load balancer.
  * VM clones are created from a baseline image, with a configuration script usually deployed to each VM to help facilitate an identical setup.
* Availability Sets:
  * Logical groupings of VMs with separate _fault domains_ and _update domains_ with the purpose of achieving high availability. Using availability set results in your VMs being assigned to hypervisors in different racks and with different maintenance windows.
  * Useful for multiple unique VMs that don't need auto-scaling.
  * Fault domain is essentially a single rack connected to a single power source and cooling. If this rack fails for whatever reason, all the hypervisors running on that rack will fail also, taking the VMs down with them. Keep in mind that when you create a VM you cannot choose which hypervisor or server rack it gets placed on.
  * Update domain is a group of hypervisors that have the same maintenance window and there is chance they might get rebooted at the same time. These hypervisors could be spread out across different server racks. If your VMs get randomly assigned to different hypervisors that are in the same update domain there is a chance your VMs might be unavailable in the same period.
  * Max limits: 20 update domains and 3 fault domains.

## App Service

* Fully managed application hosting with minimal management overhead.
* Things managed by Azure:
  * OS
  * Web Server (IIS, NGINX, Apache)
  * Network management
  * High availability
  * Disk management
  * Load balancers
  * Scaling (Scale sets): OS images, configuration scripts
* Hosting capabilities:
  * Web apps
  * Web jobs
  * API apps

## Container Options

* Azure Container Instances:
  * PaaS container service. Ideal for short-lived (allowing for cost control), simple workloads. Single containers only, no load-balancing/auto-scaling.
* Azure Kubernetes Service:
  * Open-source container orchestration.
  * Large-scale, high-volume, high-complexity containerized workloads.
  * Allows for more control and customization than ACI.
  * Automatic scaling, load balancing and failover.
  * It is in between IaaS and PaaS, allowing for more control than ACI, but also more costs.
* Azure Container Apps:
  * In between ACI and AKS.
  * Fully-managed, with simplified management.
  * Ideal for serverless and scalable microservices, with auto-scaling enabled.
  * No access to k8s API, less customization than AKS.
* VMs:
  * we can also deploy containers to VMs, but that requires a lot of configuration and management.
* App Service:
  * Ideal for long-running web applications.
  * Use any programming language and framework version.
* Azure Functions:
  * Containerized event-driven applications
  * Use any programming language and framework version.

# Azure Virtual Networks

* Core network building block in Azure. Central construct for private and public communications.
* VNet belongs to a single region. All resources inside that VNet must be in the same region.
* A subscription and resource groups can have multiple VNets.
* Networking in Azure is Software Defined Network (SDN). Physical hardware is abstracted away and we have no access to it. All networking components and functions are different software configurations.
* IP Addressing:
  * VNet must have one or more address space(s). Subnets are then contained inside that address space.
  * Allow for public networking: public IP addresses (IPv4 and IPv6).
  * VMs must have private IPs. Public IPs are optional.
  * IPs can be dynamic or static. Static IPs are extra cost even if not used.
* Multiple communication options:
  * Public IP addressing
  * Communication inside a VNet is done privately.
  * VNets can be peered privately with other Azure VNets.
  * Hybrid networking allows for private networking with on-prem and other cloud.
  * Communication with other managed services.
* VNets and IP addresses can be easily scaled.
* VNets enable high availability for our resources by peering VNets, using a load balancer or using a VPN gateway.
* Security is facilitated by filtering (using network security groups). We can also isolate our resources with subnets and network security groups.

## Network Security

### Network Security Groups

* Foundation of network security in Azure.
* Type of firewall.
* Primary traffic filter for subnets.
* Applied on subnet or individual VM level.
* It is a free service.
* Allow for granular rules for fine network control:
  * Inbound/outbound filters, e.g. by protocols and ports: TCP/UDP ports, ICMP.
  * Allow sources and destinations: we can limit access to only certain networks and not the entire internet.
  * Allow or deny multiple types of traffic.
  * Priority of overlapping rules.
  * Example: only allow RDP (TCP: 3389) access from our corporate office.

### Azure Firewall

* More advanced firewall services than NSGs, for multiple networks.
* Centralized traffic control for multiple VNets.
* Paid service, expensive.
* Additional services compared to NSGs:
  * Threat intelligence
  * Advanced filtering rules (filtering traffic by domain name and not just IP addresses)
  * URL filtering
  * DNS proxy allowing using our own DNS
  * Integration with on-premises networks.
 
### Azure Bastion

* Allows secure SSH/RDP access for private VMs.
* If your VM has no public IP address it cannot be access from outside.
* Azure Bastion provides a managed jump server that can access private VMs.
* It is a fully managed Azure service deployed to a VNet. Must have a specific subnet name.

## Network Peering

* Allows connecting networks across resource groups, subscriptions and tenants.
* Features:
  * Speed - low latency, maximum bandwidth connection because it is routed across Microsoft's backbone.
  * Traffic Privacy - routed across Microsoft's backbone instead of the public internet.
  * Control Traffic between VNets - NSG, custom routes.

## Azure DNS

* Azure DNS Zones
* Azure Private DNS Zones
* Both are global services (not tied to any region).

## Hybrid Connection

* Connecting external resources to Azure (e.g. other cloud networks, on-prem, other Azure VNets).
 * Site-to-Site VPN
   * Using VPN to connect two networks over the public internet.
 * Point-to-Site VPN
   * Using VPN to connect a network to an individual computer over the public internet.
   * Useful for working remotely.
 * ExpressRoute
   * Dedicated, private connection not using the internet, but a dedicated line.
* Regardless of which option we choose, we will need Azure service called Virtual Network Gateway. It is a managed service inside Azure, acting as an endpoint with our hybrid connectivity options.

## Azure VPN

* Point-to-site
* Point-to-point
* Active/Passive
* Active/Active

## Azure Storage

* Management plane and data plane
* Blobs
  * Block blob - any file.
  * Page blob - optimized for random access, like `.vhd` files (VM hard disks).
  * Append blob - optimized for i.e. log files.
* Files
* Tables
* Queues

### Redundancy options

* Locally-redundant storage: replicated inside a single DC, 3 copies with independent racks, cooling and storage.
* Zone-redundant storage: replicated inside multiple AZs (at least 3). Mitigates DC failures.
* Geo-redundant storage: replicated over multiple regions, using LRS inside a single region.
* Geo-zone-redundant storage: replicated over multiple regions, using ZRS inside a single region.

# Disks

* Managed Disks are VM hard disks that are created to support VMs. These are abstracted away and you cannot access these through UI.
* Disk types:
  * Standard HDD - back, non-frequent access.
  * Standard SSD - for regular workloads (web server, lightly used enterprise applications).
  * Premium - for intensive workloads (production and performance sensitive workloads).
  * Ultra - most demanding workloads with very low latency.

 <img width="1235" height="583" alt="image" src="https://github.com/user-attachments/assets/8670bd90-c811-4b50-a115-48bdaa3195c5" />

# Azure AD

## Basic Concepts

* Identity - who you are.
* Authentication - proof of your identity.
* Authorization - scopes your identity.
* AAD provides all three.
* When an organization signs up for Azure they get an Entra ID tenant. With this tenant comes one default user.
* Entra ID =/= Active Directory.
* Each user can be a member in 1 and guest of up to 499 tenants (500 in total).

## Zero Trust

* Before talking  about zero trust model we have to describe classic trusted model:
  * Corporate network with typical devices. Problem here is how to allow for remote workers. One approach is through VPNs, but these are only good for laptops and are problematic when used with mobile devices. Another problem is how to make sure a device that is connecting does not have malware. Solution - zero trust principle.
* All users assumed untrustworthy unless proven otherwise. Pushes trust down to the individual level.
* Trust based on identity, not on asset location. Identities can be validated.
* No trusted locations.
* Least privilege access - just enough permissions to perform the job.
* Simplified, centralized management.
* I.e. we don't neccessarily trust a device because it is on a corporate network, but rather we trust a device because it has a certain identity attached to it that we allow according to our centrally controlled policies.
* Problem zero trust solves: having remote workers who are not on the corporate network and still being sure their approved managed devices won't cause havoc once connected by having remote control of their devices.
* Example: access MS365 email and documents remotely without having to use VPN.

## MFA

* Provides layered security for user identity.
* Authenticating using at least two:
  * Something you know: username and password.
  * Something you have: authenticator application on your mobile phone.
  * Something you are.
 
## Conditional Access Policy

* Additional layer of security.
* If/then statements that permit or deny access depending whether the rules are met.
* Allowing access to a user only if certain signals (conditions) are met:
  * Is user a member of a certain group?
  * What application are they trying to access?
  * What is their IP location?
  * Are they using one of their approved devices?
* Access decisions:
  * Grant access.
  * Block access.
  * Require MFA.
* Scenarios
  * Enforce MFA for all admins or all users.
  * Block sign-ins using legacy authentication protocols.
  * Grant access only to specific locations.
  * Require organization-managed devices for application sign-in - prevent using a mobile device to access a certain application if the device is not managed by your organization.

## Passwordless authentication

* Password + MFA is very safe, but inconvenient. Password is not as safe, but very convenient.
* Passwordless authentication is very safe and convenient.
* Skip using a password, but rather use MFA (#MFA). Same concepts apply.
* Example: instead of using a password, log in to your mobile phone using your fingerprint (who you are) and then confirm the numerical challenge in the Authenticator app (something you know).
* Much faster and safer than just a password, but equally convenient.

## External Guest Access

* Enables security outside of your organizational boundaries.
* Provides visibility of external guest activity within your organizational IT borders.
* Solutions for Interacting with External Users
   * Entra External ID for partners (ex Azure AD B2B)
      * Designed for interacting with internal resources.
      * User has an identity inside of Entra ID that is managed by your org and created based on an external ID. 
      * The external user already has an ID with another service or provider. Your organization initiates an invitation to that external user's ID. External user accepts the invitation and a user object is created inside your organization based on that external ID.
      * Authorization is happening inside our Entra ID.
   * Entra External ID for customers (ex Azure AD B2C)
     * Designed for interacting with externally facing resources, like application, websites, etc.
     * User's identity is managed by an extenral identity provider (e.g. Google, Facebook).
     * User already has an ID with another provider and they keep using this ID. External user logs in using their current ID with their provider and that authorization is then passed into the application via a B2C directory (aka federation). Authorization is **NOT** happening inside our Entra ID.
* Make sure to use the principle of least privilege.
* Assign a guest user to access application if needed.

## Azure AD Domain Services

* Provide legacy AD features inside of Azure.
* AAD DS is a managed service inside of Azure.
* Integrates with Entra ID.
* Helpful when migrating or integrating legacy applications that do not support modern protocols.

## RBAC

* Control access to resources and services based on role assigned to the user, device, application or service.
* Built-in roles:
  * Based on board general use case.
  * Variations of:
    * Owner
    * Contributor
    * Reader
* Custom roles:
  * Fine-grained, specific use cases
  * Components:
    * Role definitions
    * Permissions
    * Scope
    * Inheritance
* Scope:
  * Only the permissions necessary
  * Only the resources necessary
  * Only to individuals when absolutely necessary (**absolutely** prefer groups).
  * **This is the most important principle!**
* Inheritance:
  * The capability of custom roles to inherit permissions from other roles.
  * The application of permission to a lower level organization construct.
* Best practices:
  * Least privilege: only the permissions necessary
  * Use of roles: use built-in roles whenever possible, use custom roles with care.
  * Role segregation: separate duties and responsibilities; avoid combining roles that grant conflicting permissions or excessive access.
  * Resource or service scope: avoid permission sprawl. You don't want your permissions going all over the place, keep them precise.
  * Review, audit and document: check, double check and write it down.
 
## Defense in depth

<img alt="image" src="https://github.com/user-attachments/assets/83639512-a3c5-435d-998a-637c8c4d67e6" width="300" height="300" />

* Objective: mitigate unauthorized data access.
* Method: layered defense centered around defending the user data. If one layer is breached other layers can still hold the attacker.
* Example: defending the user data:
  * User data is central to defend.
  * Password is a way for the user to prove their identity by providing something only they know.
  * MFA is an additional layer of security by requiring the user to prove their identity by proving they own something only they have (mobile phone with an authenticator app).
  * Conditional access policy to e.g. allow access only from a certain location.

<img width="736" height="301" alt="image" src="https://github.com/user-attachments/assets/bb00a568-7d6b-4a49-9a50-39e20c836519" />

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

# Azure Monitor

* Metrics Explorer
* Azure Monitor Logs
* Activity Logs
* Application Insights
* Monitoring Insights
* Alerts and Action Groups

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

# Observability in Azure

* Azure Monitor is the umbrella service for observability in Azure. Under Azure Monitor, you have:
  * Application Insights - telemetry from your apps
  * Log Analytics Workspace - storage and query engine for that telemetry
  * Metrics database - optimized store for numeric, time-series data

## How observability concepts map to Azure concepts

```
| Concept                  | Azure term                      | KQL tables                 | Example query  |                                    |
| ------------------------ | ------------------------------- | -------------------------- | -------------- | ---------------------------------- |
|   Logs                   | Trace & exception telemetry     | `traces`, `exceptions`     | `traces        | where severityLevel >= 2`          |
|   Metrics                | Numeric telemetry               | `metrics`, `customMetrics` | `customMetrics | summarize avg(value) by name`      |
|   Traces (Distributed)   | Requests and their dependencies | `requests`, `dependencies` | `requests      | join dependencies on operation_Id` |
```

## How telemetry flows into Azure

```
        ┌────────────────────────────────────────────┐
        │        User calls your Web App             │
        │   (HTTP request from browser or client)    │
        └────────────────────────────────────────────┘
                                 │
                                 ▼
─────────────────────────────────────────────────────────────
 ASP.NET Core + Application Insights SDK instrumentation
─────────────────────────────────────────────────────────────
                                 │
                                 ▼
                      [ Trace / Distributed Trace ]
                   Operation_ID = 1234abcd5678efgh
                                 │
        ┌────────────────────────────────────────────┐
        │                Request                     │
        │  Table: requests                           │
        │  e.g. GET /api/products                    │
        │  duration, resultCode, success, url, etc.  │
        └────────────────────────────────────────────┘
                                 │
                                 ▼
        ┌────────────────────────────────────────────┐
        │             Dependencies                   │
        │  Table: dependencies                       │
        │  e.g. SQL query, HTTP call to another API  │
        │  duration, target, success, dependencyType │
        └────────────────────────────────────────────┘
                                 │
                                 ▼
        ┌────────────────────────────────────────────┐
        │                 Logs                       │
        │  Table: traces                             │
        │  From ILogger.LogInformation/Warning/Error │
        │  Includes operation_Id for correlation     │
        └────────────────────────────────────────────┘
                                 │
                                 ▼
        ┌────────────────────────────────────────────┐
        │              Exceptions                    │
        │  Table: exceptions                         │
        │  Includes stack trace, message, severity   │
        └────────────────────────────────────────────┘
                                 │
                                 ▼
        ┌────────────────────────────────────────────┐
        │               Metrics                      │
        │  Tables: metrics, customMetrics            │
        │  Aggregated data (request count, duration) │
        │  or custom telemetry (e.g. cache hits)     │
        └────────────────────────────────────────────┘
                                 │
                                 ▼
─────────────────────────────────────────────────────────────
 Application Insights → Log Analytics Workspace
─────────────────────────────────────────────────────────────
                                 │
                                 ▼
                           🔍 Query with KQL
           (e.g. using traces, requests, dependencies tables)
─────────────────────────────────────────────────────────────
```
