# Project 2: Azure Networking and Storage

**Topics Covered**: Virtual Networks, NSGs, Storage Accounts, Encryption, Protocols, ARM Templates  

**Time**: ~0.75 hours

---

## Summary

This project covers deploying a secure storage account, creating VNets and NSGs for isolation, enabling encryption, and automating resource deployment using ARM templates. It demonstrates networking and storage fundamentals.

## Scenario

A company wants to deploy a secure storage account, ensure network isolation for its resources, enforce data encryption, and use templates to automate the deployment of resources.

## Steps

### 1. Create a Virtual Network (VNet)

- Go to **Azure Portal** > **Virtual Networks** > **Create**.
- Add two subnets: **Web** and **Database**.
  - **+ Subnet** > Name: `Database` > IPv4: `10.0.0.0` > `/27` size > enable **Private subnet** > enable **Service endpoints** for `Microsoft.Storage` for secure access.
  - **+ Add subnet** > Name: `Web` > IPv4: `10.0.1.0` > `/27` size.
- Review and create.
- Azure automatically sets up **intra-VNet communication**:  
  Subnets within the same VNet can communicate directly by default.

### 2. Deploy a Network Security Group (NSG)

- Navigate to **Network security groups** > **Create**.
- Associate it with the **Web** subnet:
  - NSG resource > **Subnets** > **Associate** > choose `Web`.
- Add inbound rules:
  - Allow HTTPS (443) traffic from the internet:
    - **Inbound security rules** > **Add** > Destination port `443` / Service `HTTPS` > **Add**.
  - Block all other inbound internet traffic (except Azure services if needed):
    - **Add** > Destination port `*` > set **Action** to `Deny` > **Add**.
  - If you need to allow specific ports like SSH/RDP, add them as rules with a **lower priority number** (higher priority).
- Note: You can’t modify the default rules (`AllowVnetInBound`, `AllowAzureLoadBalancerInBound`, `DenyAllInBound`), but you can override them with rules that have a higher priority (smaller number).

### 3. Create and Configure a Storage Account

- Navigate to **Storage accounts** > **Create**.
- Configure:
  - **Replication**: Choose LRS (locally redundant) or GRS (geo-redundant), depending on your scenario.
  - **Networking**:
    - On **Networking** tab, choose **Disable public access**.
    - Select **Private endpoint** for secure access from the database subnet.
      - Permitted scope: from storage accounts that have a private endpoint.
      - Add private endpoint:
        - Select the `Database` subnet.
- **Encryption**:
  - Use Microsoft-managed keys (default) or customer-managed keys if desired.

### 4. Deploy the VNet and Storage Account Using ARM Templates

- For both resources (VNet and Storage Account):
  - Go to the resource > **Automation** > **Export template**.
  - Download the template (`template.json`).

- Navigate to **Template specs**:
  - **Create** (or **Import template**) > select the downloaded `template.json` file > **Create**.
- Then:
  - Click the three dots (**…**) on your new template spec > **Deploy**.
  - Follow the wizard to deploy the same setup to another resource group or region.

### 5. Test Access and Encryption

- Generate a **Shared Access Signature (SAS)** token to test secure access:
  - Go to **Storage accounts** > your storage account > **Security + networking** > **Shared access signature**.
  - Select allowed services and resource types; set permissions and expiry.
  - Click **Generate SAS and connection string**.

#### Test with Azure Storage Explorer

- Download and install **Azure Storage Explorer**.
- Open it and choose **Connect to Azure resources** > **Use a shared access signature (SAS) URI**.
- Paste the **connection string** or SAS URI.
- Test uploading/downloading files (e.g., create a blob container and upload).

#### Test with AzCopy

- Install AzCopy (see official docs).  
- Run a test upload/download, for example:

```bash
azcopy copy "file-to-upload.txt" "https://<yourstorage>.blob.core.windows.net/<container>/file-to-upload.txt?<SAS_TOKEN>"
```

- Replace `<SAS_TOKEN>` with the actual SAS token.
