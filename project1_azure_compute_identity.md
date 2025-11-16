# Project 1: Azure Compute and Identity Management

**Topics Covered**: VMs, RBAC, Azure AD, Policies, Encryption, Cost Management  

**Time**: ~0.75 hours

---

## Summary

This project involves deploying a virtual machine, securing it using RBAC, applying Azure Policies for resource governance, encrypting sensitive data, and monitoring costs. It teaches foundational concepts for managing Azure identities, governance, and compute resources.

## Scenario

A company wants to deploy a virtual machine for their web application, secure it with role-based access control, enforce a policy for naming conventions, encrypt sensitive data, and monitor the cost of the deployed resources.

## Steps

### 1. Create a Virtual Machine

- Go to **Azure Portal** > **Virtual Machines** > **Create**.
- Select subscription, resource group, region, VM size, and OS (Windows/Linux).
- Configure an admin username/password or SSH key.
- Add a **data disk** during creation.
  - Create a new disk > None.
  - If you select Storage Blob, this allows you to reuse disks from outside Azure or custom configurations you uploaded.
- Configure **Networking**:
  - Assign a static private IP via the **Advanced Networking** section.
  - Use a **Network Security Group (NSG)** to allow or block traffic.
    - **None:** Use if you're managing the NIC setup manually or attaching an existing NIC.
    - **Basic:** Use for quick and simple deployments, such as test environments or small-scale applications.
    - **Advanced:** Use when you need fine-grained control, such as for production workloads or VMs requiring custom network setups.
- Review and create the VM.
- Download private key.

### 2. Configure Role-Based Access Control (RBAC)

- Now create a Key Vault and store the private key (follow least privilege principle):
  - Go to **Key vaults** > **Create** > use same resource group and region (region must match) > **Review + create**.
  - Go to **Entra ID** > **Groups** > **New group** > create a group and select yourself as **Owner** and **Member**.
  - Go to **Key vaults** > your vault > **Access control (IAM)** > **Add role assignment** > choose **Key Vault Administrator** > **Members** > select your new group > **Review + assign**.  
    - Note: It can take a while to propagate.
    - You may need to **sign out and sign in** to Azure Portal to refresh your token (or temporarily assign it directly to yourself).
  - Go to **Key vaults** > your vault > **Objects** > **Keys** > **Generate/Import** > **Import** > upload key file.
- Navigate to the VM > **Access Control (IAM)** > **Add role assignment**.
- Assign the **Virtual Machine Contributor** (or a custom role) to a user or group.
- Verify permissions:
  - Go to **Entra ID** > **Groups** > **All groups** > your group name.

### 3. Log into the VM

- Go to **Virtual Machines** > your VM > **Connect** > **SSH using Azure CLI** > **Configure**.
- In the new Cloud Shell/CLI that opens, run:

```bash
az network public-ip list
```

- Test the public IP in the browser to confirm that only SSH is allowed (not HTTPS) due to NSG rules.

### 4. Apply Azure Policy

- Navigate to **Azure Policy** > **Authoring** > **Definitions** > **+ Initiative definition**.
- Fill in:
  - Name
  - Category
- Click **Next**.
- **Add policy definitions**:
  - Search for `tag`.
    - Select some **“Inherit a tag from resource group…”** and **“Inherit a tag from subscription_name”** policies.
  - Search for `allow`.
    - Select **Allowed resource types**.
- Click **Review + create** > **Next**.
- Optionally, create a dedicated resource group or management group for these policies.
- In **Initiative parameters**:
  - Create an initiative parameter for tag **Name** and for **Allowed resource types**.
  - Set **Value Type** to **Use Initiative Parameter** and use the parameter you just created.
  - For **Allowed resource types**, set `Microsoft.Compute/virtualMachines`.
- **Assign** the initiative to the subscription and test:
  - Scope: select your subscription.
  - Parameters: choose values for tag name and allowed resource types.
  - Click **Review + create** > **Create**.
- To view policy and compliance:
  - Go to your VM > **Operations** > **Policies**.

### 5. Monitor and Manage Costs

- Navigate to **Cost Management + Billing** > set correct **Billing scope**.
- Go to **Cost Management** > **Budgets** > **Add**:
  - Set a monthly spending limit for the subscription.
  - Configure email or action group notifications for thresholds (e.g., 50%, 80%, 100%).
- Use **Cost analysis** to identify trends and optimize spending.
- Review **Azure Advisor** recommendations for cost optimizations.
