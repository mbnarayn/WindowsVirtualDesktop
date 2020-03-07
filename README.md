# Windows Virtual Desktop

Windows Virtual Desktop is a desktop and app virtualization service that runs on Microsoft Azure. Windows Virtual Desktop or WVD for short is a Desktop as a Service offering that provides the capability to publish full desktops or individual remote apps. With WVD, all the infrastructure services, such as brokering, web access, load-balancing, management and monitoring is all automatically setup and managed by the Microsoft Azure platform. As it is no longer necessary to run or manage any of the traditional RDS roles such as the License Server, Connection Broker or Gateway servers, the associated costs to host and manage these servers are eliminated.

Windows Virtual Desktop also provides the capability to run Windows 10 multi-session. Windows 10 Enterprise multi-session can't run in on-premises production environments because it's optimized for the Windows Virtual Desktop service for Azure. Windows 10 Enterprise multi-session, formerly known as Windows 10 Enterprise for Virtual Desktops (EVD), is a new Remote Desktop Session Host that allows multiple concurrent interactive sessions. Previously, only Windows Server could do this. This capability gives users a familiar Windows 10 experience while IT can benefit from the cost advantages of multi-session and use existing per-user Windows licensing instead of RDS Client Access Licenses (CALs).

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

- Breadth-first load balancing allows you to evenly distribute user sessions across the session hosts in a host pool. The breadth-first load-balancing method allows you to distribute user connections to optimize for this scenario. This method is ideal for organizations that want to provide the best experience for users connecting to their pooled virtual desktop environment.

The breadth-first method first queries session hosts that allow new connections. The method then selects the session host with the least number of sessions. If there is a tie, the method selects the first session host in the query

- Depth-first load balancing allows you to saturate a session host with user sessions in a host pool. Once the first session reaches its session limit threshold, the load balancer directs any new user connections to the next session host in the host pool until it reaches its limit, and so on. The depth-first load-balancing method allows you to saturate one session host at a time to optimize for this scenario. This method is ideal for cost-conscious organizations that want more granular control on the number of virtual machines they've allocated for a host pool.

The depth-first method first queries session hosts that allow new connections and haven't gone over their maximum session limit. The method then selects the session host with highest number of sessions. If there's a tie, the method selects the first session host in the query.

Each host pool can only configure one type of load-balancing specific to it. However, both load-balancing methods share the following behaviors no matter which host pool they're in:

- If a user already has a session in the host pool and is reconnecting to that session, the load balancer will successfully redirect them to the session host with their existing session. This behavior applies even if that session host’s AllowNewConnections property is set to False. 
- If a user doesn't already have a session in the host pool, then the load balancer won't consider session hosts whose AllowNewConnections property is set to False during load balancing.

### Persistent/Personal Host Pool

A Persistent or Personal host pool supports two user assignment types:

- Automatic assignment is the default assignment type for new personal desktop host pools created in your Windows Virtual Desktop environment. To automatically assign users, first assign them to the personal desktop host pool so that they can see the desktop in their feed. Users are auto-assigned an available  session host during the first logon and any subsequent logins are directed to the same VM. Unlike multi user session, persistence follows a 1:1 mapping between users and session hosts. For Example: if the Host Pool has ten VM’s, they will be assigned to the first ten users and the eleventh user will get an error that enough resources (VMs) are unavailable.

 - Unlike automatic assignment, when you use direct assignment, you must assign the user to both the personal desktop host pool and a specific session host before they can connect to their personal desktop. If the user is only assigned to a host pool without a session host assignment, they won't be able to access resources.
 
## URLs

Windows Virtual Desktop Consent Page https://rdweb.wvd.microsoft.com/





