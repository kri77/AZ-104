# Project 3: Monitoring, Backup, and Recovery

**Topics Covered**: Azure Monitor, Alerts, Backup, Encryption, Protocols, Log Queries  

**Time**: ~0.5 hours

---

## Summary

This project focuses on setting up Azure Monitor for performance monitoring, enabling backups for data recovery, configuring disaster recovery with Site Recovery, and analyzing logs with Log Analytics.

## Scenario

A company wants to monitor their infrastructure for performance issues, ensure data recovery in case of failure, enforce secure data transfer, and analyze logs for insights.

> 💡 Note: You can reuse the VM from **Project 1**.

## Steps

### 1. Configure Azure Monitor for VMs

- Go to your VM > **Monitoring** > **Insights**.
- If not enabled, click **Enable** to onboard the VM to Azure Monitor.
- Go to the VM > **Monitoring** > **Alerts** > **View + set up**.
  - Turn on a **High CPU** alert rule:
    - Threshold: e.g., 80% CPU for 5 or 10 minutes.
    - Action group: email or other notification methods.

### 2. Set Up Azure Backup

- Navigate to **Recovery Services vaults** > **Create**.
- Configure:
  - **Backup Policy**:
    - Daily backups with custom retention (e.g., 7 days).
    - Retention for weekly/monthly backups as needed.
  - Enable encryption for data at rest (default is Microsoft-managed keys).
- After association:
  - Perform a **manual backup** of the VM.
  - Verify backup job status in the vault > **Backup items** > **Azure Virtual Machine**.

### 3. Implement Azure Site Recovery

- On the VM blade, go to **Disaster recovery**.
- Enable replication to a secondary region.
- Configure:
  - Target subscription, resource group, VNet, subnet, and storage.
  - Replication policy (RPO, app-consistent snapshots if needed).
- Run a **test failover**:
  - In the Recovery Services vault > **Site Recovery** > your replicated item.
  - Select **Test failover** to verify the failover process without affecting production.
- Monitor replication health using **Azure Monitor** and the Site Recovery blades.

### 4. Query Logs in Azure Monitor

- Navigate to your **Log Analytics workspace** > **Logs**.
- Run a query to analyze CPU usage, for example:

```kusto
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| summarize Avg_CPU = avg(CounterValue) by bin(TimeGenerated, 5m)
```

- Use the **Chart** button to visualize the results (e.g., time chart).

### 5. Secure Backup and Recovery

- Go to your **Recovery Services vault** > **Properties** and verify:
  - Encryption settings (default is using platform-managed keys).
- Ensure secure communication:
  - Confirm TLS is used for all backup traffic (this is default when using the Azure Backup agent and VM extension).
- Optionally, configure:
  - **Soft delete** for backups to prevent accidental deletion of backup data.
