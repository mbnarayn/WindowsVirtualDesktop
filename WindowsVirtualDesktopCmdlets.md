## Windows Virtual Desktop Cmdlets for Windows PowerShell

**Install the Windows Virtual Desktop PowerShell module**

`Install-Module -Name Microsoft.RDInfra.RDPowerShell`

**Import the Windows Virtual Desktop PowerShell module (If necessary)**

`Import-Module -Name Microsoft.RDInfra.RDPowerShell`

**Update the Windows Virtual Desktop PowerShell module**

`Update-Module -Name Microsoft.RDInfra.RDPowerShell`

**Sign in to the Windows Virtual Desktop environment**

`Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"`

**Get all Windows Virtual Desktop tenants in the current context**

`Get-RdsTenant`

**View the role assignments on a Windows Virtual Desktop Tenant**

`Get-RdsRoleAssignment -TenantName yourtenantname`

or

`Get-RdsRoleAssignment`

The Get-RdsRoleAssignment cmdlet lists all role assignments such as RDS Owner, RDS Contributor, RDS Reader and RDS Operator.

**Assign the RDS Owner role on a Windows Virtual Desktop Tenant**

`New-RdsRoleAssignment -TenantName yourtenantname -SignInName username@contoso.onmicrosoft.com -RoleDefinitionName "RDS Owner"`

**Get properties of a Host Pool**

`Get-RdsHostPool -TenantName yourtenantname`

The Get-RdsHostPool cmdlet gets the properties of the specified host pool. If you do not specify a host pool, this cmdlet returns properties for all host pools in the specified tenant authorized for the current user.

**Add Users to an App Group**

`Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>`

The user’s UPN should match the user’s identity in Azure Active Directory (for example, user1@contoso.com). If you want to add multiple users, you must run this cmdlet for each user.

**Remove Users from an App Group**

`Remove-RdsAppGroupUser -TenantName "contoso" -HostPoolName "contosoHostPool" -AppGroupName "officeApps" -UserPrincipalName "user1@contoso.com"`

**Disable new connections to a session host (aka, set the host to drain mode)**

`Set-RdsSessionHost -TenantName "contoso" -HostPoolName "contosoHostPool" -Name "sh1.contoso.com" -AllowNewSession $false`

**Enable new connections to a session host (aka, remove the host from drain mode)**

`Set-RdsSessionHost -TenantName "contoso" -HostPoolName "contosoHostPool" -Name "sh1.contoso.com" -AllowNewSession $true`

**Configure a host pool to perform breadth-first load balancing and to use a new maximum session limit**

`Set-RdsHostPool <tenantname> <hostpoolname> -BreadthFirstLoadBalancer -MaxSessionLimit ###`

**Configure a host pool to perform depth-first load balancing**

`Set-RdsHostPool <tenantname> <hostpoolname> -DepthFirstLoadBalancer -MaxSessionLimit ###`

**Create a pooled host pool**

`New-RdsHostPool -TenantName "contoso" -Name "contosoHostPool"`

**Delete a Host Pool**

`Remove-RdsHostPool -TenantName "contoso" -Name "contosoHostPool"`

You must first remove all session hosts and app groups associated with the host pool before running this command.

**Create new registration information for a host pool**

`New-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname> -ExpirationHours 6 | Select-Object -ExpandProperty Token | Out-File -FilePath C:\Token\Token.txt`

**List all users assigned to an app group**

`Get-RdsAppGroupUser -TenantName "contoso" -HostPoolName "contosoHostPool" -AppGroupName "Desktop Application Group"`

**Get all session hosts in the host pool**

`Get-RdsSessionHost -TenantName "contoso" -HostPoolName "contosoHostPool"`




