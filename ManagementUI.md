# Windows Virtual Desktop Management UI

A new native GUI admin experience for managing Windows Virtual Desktops is currently being developed by Microsoft but this is not yet in public preview. An early peek was demoed in November 2019 at Ignite. The demo can be seen here https://www.youtube.com/watch?v=dCR9U7bODm0 at around the 14th minute of the video. This will be the new native experience for managing Windows Virtual Desktops in future. But in the meantime, Microsoft have made available the source code for an interim management tool in GitHub. This interim management tool which is basically a WVD SaaS web app can be deployed directly from GitHub. The admin experience is relatively basic but suitable enough to be able to manage host pools, groups and group membership, published applications and desktops which is clearly easier than using PowerShell. This is important mainly because management of the WVD tenant PaaS service can only be accomplished via PowerShell at present as the Orchestration Control Plane for WVD isn’t available yet.
 
https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux/deploy
 
**Required Permissions for the Deployment Account**

Before deploying the management tool, you'll need an Azure Active Directory (Azure AD) user to create an app registration and deploy the management UI. This user must:
 
•	Have Azure Multi-Factor Authentication (MFA) disabled
•	Have Global Admin Rights on Azure AD.
•	Have Owner Rights on the Azure Subscription. Contributor rights are not enough because in addition to creating resources, you must also have sufficient permissions to register an application with your Azure AD tenant, and further assign roles to the application for Windows Virtual Desktop.
 
Note: When deploying the management tool, an option to use a Service Principal is presented, however at this time (1st of March 2020) using a Service Principal that meets the above requirements still does not work and results in various errors. 
 
**Issues Encountered**
 
•	Attempting to deploy the management tool with a Service Principal (Contribute and Owner) that has all the required permissions does not work as for some reason it fails to create an Automation Account which is one of the deployment steps. So, I reverted to using a standard account with all the necessary privileges.

•	Attempted to deploy with an MFA enabled account but this failed as expected. So, set up a temporary admin account and used the option to skip MFA setup for 14 days which worked.

**Resources Created**
 
The deployment process creates the following the resources;
 
1.	Enterprise Application in Azure AD with the prefix wvdSaaS. The Enterprise Application creates the Service Principal and assigns the permission to access Windows Virtual Desktop in the next step and hence the need for Owner Rights
2.	A Service Principal in Azure in Azure AD with the prefix wvdSaaS (has the same name as the Enterprise Application)
3.	A Web App, an Api App and App Service Plan. The URL of the Web App will be the management URL for WVD.
4.	An Automation Account also gets created during the deployment process and this automatically gets deleted once the deployment has successfully completed. If there are any errors during the deployment process the Automation Account either does not get created or if created, you can look through the jobs within the Automation Account to see any low-level errors that may have interrupted the successful deployment of the management tool. 
 
Once the deployment has completed logon to the management tool Web App URL as the same admin account (or any Global Admin account) to grant consent again and select consent on behalf of your organisation. Going forward the user who launches the management UI must have a role assignment that lets them view or edit the Windows Virtual Desktop tenant.
 

