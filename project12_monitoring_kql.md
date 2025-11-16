# Chapter 12: Advanced Monitoring & KQL Analytics

## Overview
This chapter focuses on advanced monitoring concepts in Azure, including log collection, diagnostic settings, KQL queries, workbooks, and alert rules.

## What You Will Learn
- Enable diagnostic settings  
- Create alert rules  
- Build KQL queries  
- Visualize logs using workbooks  

## Scenario
A company needs to centralize logs from multiple Azure resources and build operational dashboards.

---

## Lab Activities

### 1. Enable Diagnostic Settings
Enable for:
- Virtual machines  
- Storage accounts  
- VNets  
- Key Vault  

Send to a Log Analytics workspace.

---

### 2. Create Alerts
Examples:
- VM CPU > 80%  
- Storage account throttling  
- Key Vault access denied events  

---

### 3. Sample KQL Queries

VM heartbeat:
```kusto
Heartbeat
| summarize last_seen=max(TimeGenerated) by Computer
```

Firewall events:
```kusto
AzureDiagnostics
| where Category == "FirewallNetworkRule"
| summarize count() by bin(TimeGenerated, 5m)
```

---

### 4. Create a Workbook
- Add multiple KQL visualizations  
- Use charts + grids  
- Export as ARM template  

