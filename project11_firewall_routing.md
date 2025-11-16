# Chapter 11: Azure Firewall & Routing

## Overview
This chapter teaches how to build a hub-spoke architecture, route outbound traffic through Azure Firewall, and configure application/network rules.

## What You Will Learn
- Deploy Azure Firewall  
- Hub-spoke VNet design  
- User-Defined Routes (UDRs)  
- Firewall rules  

## Scenario
A company wants full control over outbound traffic and requires all traffic to pass through a central firewall.

---

## Lab Activities

### 1. Create Hub VNet
Subnets:
- AzureFirewallSubnet  
- MgmtSubnet  

### 2. Deploy Azure Firewall
- Public IP  
- Firewall Policy  

### 3. Create Spoke VNet
Subnets:
- AppSubnet  
- DataSubnet  

### 4. Configure VNet Peering
Enable:
- Allow forwarded traffic  
- Allow gateway transit  

### 5. Create UDR for Outbound Traffic
Route table:
```
0.0.0.0/0 → Firewall Private IP
```

### 6. Add Firewall Rules
#### Application Rule
- Allow: `https://microsoft.com`

#### Network Rule
- Allow: DNS (UDP 53)

