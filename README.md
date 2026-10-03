**Microsoft Entra ID Groups, Membership, and Role-Based Access Control (RBAC) Lab**

Introduction

This lab demonstrates how Microsoft Entra ID security groups and Azure Role-Based Access Control (RBAC) are used to manage access to security resources. A SOC-Analysts security group was created, a test analyst account was added as a member, and the Microsoft Sentinel Reader role was assigned to the group at the resource-group level.

The lab then validated the configuration by signing in as the test analyst and confirming access to the Microsoft Sentinel workspace, data connectors, Microsoft Entra ID connector information, and workspace settings. The exercise demonstrates the principle of least privilege, where SOC analysts receive the permissions necessary to monitor and investigate security information without being granted unnecessary administrative privileges.

Step 1 — Checking the existing security group

This image confirms the setup.

Total groups: 1\
Security groups: 1\
Cloud groups: 1 (Image 1)\

Step 2 — Opening the existing group\
This image confirms the existing group:

### Group: SOC-Analysts
Group type: Security\
Membership type: Assigned (Image 2)\

Step 3 — Opening the SOC-Analysts group (Overview )

Trying to verify which user is a member of this security group before moving to the Azure RBAC permission itself.\
This images confirms the SOC-Analysts security group has:

Total direct members: 1
Users: 1\
Type: Security\
Membership type: Assigned\
Source: Cloud (Images 3 and 4)\

Step 4 — Verifying the member

I want to confirm that the existing test/SOC user is the one inside SOC-Analysts.
This image confirms the membership is correct:\
SOC-Analysts security group → SOC Test User\
And the identity side of the RBAC chain is verified:\
SOC Test User → SOC-Analysts → Azure RBAC role → Azure resource (Image 5)\

Step 5 — Check the Azure role assignment

Azure role assignments

This is important because Entra group membership and Azure RBAC permissions are two different things:

Group membership = who belongs to SOC-Analysts\
Azure RBAC = what that group is permitted to do with Azure resources

This image confirms the Azure RBAC configuration is working exactly as intended.

The important chain is now visible:

SOC Test User\
↓ member of\
SOC-Analysts\
↓ assigned Azure RBAC role\
Microsoft Sentinel Reader\
↓ scope\
SOC-Lab-RG (Resource Group)

That means the SOC Test User inherits the Microsoft Sentinel Reader permission through membership in the SOC-Analysts group. (Image 6)

Step 6 — Testing the RBAC permission as the SOC Test User

I am going to verify what the SOC Test User can actually access, rather than only looking at the administrator configuration.

I open a new browser window. I will use that separate window to sign in as SOC Test User, so the administrator session remains untouched.

Step 7 — Opening Azure as the SOC Test User\
I open: [Microsoft Azure Portal](https://portal.azure.com/?utm_source=chatgpt.com)\
The next login will be the SOC Test User so I can test the inherited Microsoft Sentinel Reader RBAC permission.\
\
Step 8 — Enter the SOC Test User account

In Email, phone, or Skype, I enter my username for the SOC Test User that I created previously: <testuser01@liberty566yahoo.onmicrosoft.com> and I entered my password.\
The image confirms I am signing in as the correct account:testuser01@liberty566yahoo.onmicrosoft.com\
\
Then, the SOC Test User successfully signed in to Azure.

The image already demonstrates an important RBAC principle. Notice the red message:

“You don't have permission to view credits.”

The test account does not have broad administrative/subscription permissions.

(Images 7 and 8)

Step 9 — Test the Sentinel Reader permission

Now I need to see whether the permission I deliberately granted works.

In the top Azure search bar, I type:\
Microsoft Sentinel\
This is the key RBAC access test:\
SOC Test User → SOC-Analysts → Microsoft Sentinel Reader → SOC-Lab-RG.\
The RBAC test is successful. The image shows that the SOC Test User can see the Microsoft Sentinel workspace:

Workspace: soc-sentinel-workspace\
Resource Group: soc-lab-rg\
Subscription: Azure subscription 1

This is exactly what I was expected from the Microsoft Sentinel Reader role. (Image 9)\
\
\
\
Step 10 — Open the Sentinel workspace\
I click on: soc-sentinel-workspace\
These images give me the evidence I needed. The RBAC permission is working.

As the SOC Test User, I can read the Sentinel workspace and see operational information including:

Incidents: 0 in the last 24 hours\
Data: events are being received

Data connectors: 1 active connector\
Automation: no automation rules currently\
Analytics: 1 analytics rule enabled\
Workspace: soc-sentinel-workspace

This demonstrates the purpose of the Microsoft Sentinel Reader role: The SOC user can view Sentinel security information without being given broad Azure administrative privileges. Earlier, Azure also showed that this account could not view subscription credits, which is useful evidence of restricted access. (Images 10, 11, 12, and 13)

Step 11 — Final RBAC restriction tests\
I click on: Configuration, Then Data connectors

I already know the Reader account can *view* the connector information. Now I inspect the Data Connectors page and see what management capabilities Azure permits for this account.

The images show SOC Test User can successfully view the Sentinel Data connectors. The images also show 8 connected connectors, including Microsoft Defender XDR and Microsoft Entra ID.

This demonstrates the Microsoft Sentinel Reader role working as intended: the analyst can inspect Sentinel security information but should not have administrative control over the environment.\
(Images 14 and 15)\

Step 12 — One final RBAC test

I Click on Microsoft Entra ID in the connector list.

I am only going to inspect its connector page.\
Yes. This is exactly the result I wanted.

The image confirms that, while signed in as SOC Test User, you can:

Open the Microsoft Sentinel workspace.\
Access Data connectors.\
See Microsoft Entra ID.\
See its status as Connected.\
Read the connector information.

This confirms the SOC-Analysts → Microsoft Sentinel Reader RBAC assignment is functioning for read access. The test account can inspect Sentinel resources without being given full administrative control. (Image 16)

Step 13 — Final RBAC Verification

I want to prove that the SOC Test User can read Sentinel but cannot administer it.

While I am still signed in as SOC Test User:

Under Configuration, I click Settings.

This will help verify the boundary between Reader access and administrative access.

After that, if the permission boundary is confirmed, I can mark the Entra ID Groups, Membership & RBAC Lab as completed.\
\
The images confirm that SOC Test User can open Sentinel Settings and view workspace usage/pricing information. That is consistent with the read-only access we assigned.

I successfully demonstrated the important workflow:

User → Security Group → RBAC Role → Resource Group → Sentinel access

Specifically, SOC Test User → SOC-Analysts → Microsoft Sentinel Reader → SOC-Lab-RG → Sentinel workspace.

### 

### 
