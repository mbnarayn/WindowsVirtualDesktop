# Windows Virtual Desktop

The high level components that make up Windows Virtual Desktop are a Tenant Group, Tenant, Host Pool and App Groups.

- Tenant Group - When you create a tenant in Windows Virtual Desktop it is associated by default to a Tenant Group called "Default Tenant Group". While the New-RdsTenantGroup PowerShell cmdlet appears to make it possible to create multiple Tenant Groups, there is no documentation or use cases available from Microsoft regarding the creation of multiple tenant groups so it is better to leave this as is for now and operate from the Default Tenant Group. Management access and end user permissions can be configured via controls available at the Tenant and Application Group levels.

- Tenant - Creating a tenant in Windows Virtual Desktop is the first step toward building your desktop virtualization solution. A tenant is a group of one or more host pools. With a tenant, you can build out host pools, create app groups, assign users, and make connections through the service. 

- Host Pool - Host pools are a collection of one or more identical virtual machines running as session hosts in Azure and registered to a Windows Virtual Desktop tenant. Each host pool will contain an app group that end users can interact with.  

- App Groups - Each host pool consists of one or more app groups that are used to publish remote desktop and remote application resources to users. The default app group "Desktop Application Group" created for a new Windows Virtual Desktop (WVD) Host Pool provides the full desktop experience. In addition, you can create one or more RemoteApp application groups for the host pool to provide an app only experience without overwhelming the user with a full blown desktop experience. 
