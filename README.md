# Windows Virtual Desktop

## High Level Components

The high level components that make up Windows Virtual Desktop are a Tenant Group, Tenant, Host Pool and App Groups.

- Tenant Group - When you create a tenant in Windows Virtual Desktop it is associated by default to a Tenant Group called "Default Tenant Group". While the New-RdsTenantGroup PowerShell cmdlet appears to make it possible to create multiple Tenant Groups, there is no documentation or use cases available from Microsoft regarding the creation of multiple tenant groups so it is better to leave this as is for now and operate from the Default Tenant Group. Management access and end user permissions can be configured via controls available at the Tenant and Application Group levels.

- Tenant - Creating a tenant in Windows Virtual Desktop is the first step toward building your desktop virtualization solution. A tenant is a group of one or more host pools. With a tenant, you can build out host pools, create app groups, assign users, and make connections through the service. 

- Host Pool - Host pools are a collection of one or more identical virtual machines running as session hosts in Azure and registered to a Windows Virtual Desktop tenant. Each host pool will contain an app group that end users can interact with.  

- App Groups - Each host pool consists of one or more app groups that are used to publish remote desktop and remote application resources to users. The default app group "Desktop Application Group" created for a new Windows Virtual Desktop (WVD) Host Pool provides the full desktop experience. In addition, you can create one or more RemoteApp application groups for the host pool to provide an app only experience without overwhelming the user with a full blown desktop experience. 

## Host Pool Types

Windows Virtual Desktop host pools can deployed in one of two different modes, Pooled or Persistent (also known as Personal). With a 'Pooled' host pool users are directed to the best available session host in the pool and to utilize shared multi-session virtual machines. With a 'Persistent' host pool users have their own virtual machine always have a 1:1 mapping to a session host within the host pool.

### Pooled Host Pool

Host Pools in 'Pooled' mode supports two load-balancing methods. Each method determines which session host will host a user’s session when they connect to a resource in a host pool.

- Breadth-first load balancing allows you to evenly distribute user sessions across the session hosts in a host pool.

- Depth-first load balancing allows you to saturate a session host with user sessions in a host pool. Once the first session reaches its session limit threshold, the load balancer directs any new user connections to the next session host in the host pool until it reaches its limit, and so on.

### Persistent/Personal Host Pool

A Persistent or Personal host pool supports two user assignment types:

- Automatic assignment is the default assignment type for new personal desktop host pools created in your Windows Virtual Desktop environment. To automatically assign users, first assign them to the personal desktop host pool so that they can see the desktop in their feed. Users are auto-assigned an available  session host during the first logon and any subsequent logins are directed to the same VM. Unlike multi user session, persistence follows a 1:1 mapping between users and session hosts. For Example: if the Host Pool has ten VM’s, they will be assigned to the first ten users and the eleventh user will get an error that enough resources (VMs) are unavailable.

 - Unlike automatic assignment, when you use direct assignment, you must assign the user to both the personal desktop host pool and a specific session host before they can connect to their personal desktop. If the user is only assigned to a host pool without a session host assignment, they won't be able to access resources.
 
## URLs

Windows Virtual Desktop Consent Page https://rdweb.wvd.microsoft.com/

## PowerShell Commands

### Add an authenticated account to use for Windows Virtual Desktop cmdlet requests

`Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"`

The Add-RdsAccount cmdlet adds an authenticated account to use for Windows Virtual Desktop cmdlet requests. Upon completion, the context is automatically set to use the "Default Tenant Group" as the tenant group name. You can run the Set-RdsContext cmdlet to change the context.

### Get properties of a Host Pool

`Get-RdsHostPool -TenantName yourtenantname`

The Get-RdsHostPool cmdlet gets the properties of the specified host pool. If you do not specify a host pool, this cmdlet returns properties for all host pools in the specified tenant authorized for the current user.

`Get-RdsRoleAssignment -TenantName yourtenantname`

`New-RdsRoleAssignment -TenantName yourtenantname -SignInName username@contoso.onmicrosoft.com -RoleDefinitionName "RDS Owner"`

