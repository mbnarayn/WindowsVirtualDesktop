# WindowsVirtualDesktop

Creating a tenant in Windows Virtual Desktop is the first step toward building your desktop virtualization solution. A tenant is a group of one or more host pools.

When you create a tenant in Windows Virtual Desktop it is created by default in a Tenant Group called "Default Tenant Group". While the New-RdsTenantGroup appears to make it possible to create multiple Tenant Groups, there is no documentation or use cases available from Microsoft regarding the creation of multiple tenant groups so it is better to leave this as is for now and operate from the Default Tenant Group. Management access and end user permissions can be configured via controls available at the Tenant and Application Group levels.


Enter the name of the tenant group name associated with your Windows Virtual Desktop tenant. If you were not given one, leave it as 'Default Tenant Group'.
