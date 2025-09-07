# Azure-RBAC
# Implementing Role-Based Access Control in Azure
This project demonstrates how to secure Azure resources by assigning roles to users, groups, and service principals using Azure Role-Based Access Control (RBAC). It showcases the practical application of least privilege access, one of the core principles of cloud security in Microsoft Azure.

I chose this project because RBAC is fundamental to Azure security and a key part of the AZ-500 Microsoft Certified: Azure Security Engineer Associate exam. It allowed me to implement real-world security best practices for identity and resource management.

Why This Project?

Organizations need to enforce granular access control to reduce security risks. RBAC allows assigning permissions at the right scope—such as subscription, resource group, or individual resource level—ensuring users have only the access they need.

This lab is crucial because:
✔ Prevents privilege escalation
✔ Demonstrates compliance with Zero Trust and least privilege principles
✔ Helps secure sensitive Azure resources

✅ Step 1: Create an Azure Active Directory User & Group
Sign in to the Azure Portal.
Navigate to Azure Active Directory → Users → + New User
Name: RBACUser1
Username: rbacuser1@yourdomain.onmicrosoft.com
Assign a temporary password
Create a Security Group:
Go to Azure Active Directory → Groups → + New Group
Group type: Security
Group name: RBAC-Security-Group
Membership type: Assigned
Add RBACUser1 as a member

✔ Why?
RBAC roles can be assigned to users, groups, service principals, and managed identities. Assigning roles to groups simplifies administration for multiple users.

✅ Step 2: Create a Resource Group & Resource
In the Azure Portal, click + Create a Resource → Resource Group
Name: RBAC-Demo-RG
Region: East US
Click Review + Create → Create
Add a Virtual Machine for demonstration:
VM name: RBAC-Demo-VM
Resource group: RBAC-Demo-RG
Select basic configuration for cost efficiency

✔ Why?
We need resources to apply and test RBAC permissions.

✅ Step 3: Assign Built-in Roles to the Group

Azure RBAC uses Role Definitions (built-in or custom) that define permissions.

Go to RBAC-Demo-RG → Access Control (IAM) → + Add → Add role assignment

Choose Role:

Reader → Allows viewing resources (no modification)

Assign access to: User, group, or service principal

Select RBAC-Security-Group

Click Review + Assign

✔ Why Reader Role?
This demonstrates read-only access for least privilege testing.

✅ Validation:
Log in as RBACUser1 → They should view the resource group but cannot make changes.

✅ Step 4: Test RBAC Scope & Privilege Escalation Prevention
Try to start/stop the VM as RBACUser1
Result: Access Denied
Change role to Contributor at the resource group level
Contributor allows create and manage resources, but not role assignments
Test again → Now start/stop works, but IAM changes are still blocked
✔ Why?
RBAC enforces separation of duties. Even with Contributor, role assignment requires Owner or User Access Administrator.

✅ Step 5: Assign Role at Different Scope Levels

RBAC roles can be assigned at:
Management Group → Subscription → Resource Group → Resource
Example:
Assign Reader at Subscription level → user sees all RGs but cannot modify
Assign Contributor at Resource level → user can only manage that specific VM

✔ Why?
This demonstrates hierarchical inheritance: roles assigned at a higher scope apply to all lower scopes.

✅ Step 6: Create a Custom Role (Optional but Important)
Custom roles allow fine-grained control.
Example: Create a role that can restart VMs but cannot delete them:
Navigate to Azure AD → Roles and Administrators → + Add Custom Role
Define permissions JSON:

{
  "Name": "VM Restart Operator",
  "Description": "Can restart VMs but not delete",
  "Actions": [
    "Microsoft.Compute/virtualMachines/restart/action"
  ],
  "NotActions": [
    "Microsoft.Compute/virtualMachines/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/{subscription-id}"
  ]
}


Assign this role to RBAC-Security-Group

✅ Step 7: Clean Up Resources
To avoid unnecessary costs:
Remove role assignments
Delete RBAC-Demo-RG
Delete users and groups from Azure AD
