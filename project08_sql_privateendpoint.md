# Chapter 8: Azure SQL with Private Endpoints

## Overview
This chapter teaches how to deploy a secure Azure SQL environment using Private Endpoints and private DNS. You will disable public access and test connectivity from a VM inside a VNet.

## What You Will Learn
- Deploy Azure SQL Server + database  
- Configure Private Endpoints  
- Set up Private DNS zones  
- Test private connectivity  

## Scenario
A company requires their SQL Database to be accessible only through internal networks. You will lock down public access and ensure private-only connectivity.


## Lab Activities

### 1. Create Azure SQL Server and Database
Use the portal:
- Networking → Public access = **Disabled**  
- Minimum TLS = 1.2  
- Backup retention enabled  

### 2. Create the Private Endpoint
1. SQL Server → **Networking** → Private access  
2. Create endpoint:
   - Subnet: *AppSubnet*
   - DNS zone: `privatelink.database.windows.net`  

### 3. Deploy VM for Testing
- Place VM in same VNet  
- Install SQL tools:
```bash
sudo apt-get update
sudo apt-get install mssql-tools unixodbc-dev
```

### 4. Validate DNS & Connectivity
```bash
nslookup <servername>.database.windows.net
# Should resolve to private IP
```

Connect:
```bash
sqlcmd -S <servername>.database.windows.net -U <user> -P <password>
```

