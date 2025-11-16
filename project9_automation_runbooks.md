# Chapter 9: Automation Accounts & Runbooks

## Overview
This chapter covers Azure Automation, runbooks, Managed Identity, and scheduled automation tasks. You will automate shutting down virtual machines to reduce cost.

## What You Will Learn
- Create Automation Accounts  
- Develop PowerShell runbooks  
- Assign RBAC to Managed Identity  
- Schedule automated tasks  

## Scenario
A company wants to reduce cost by automatically shutting down non-production VMs at night. You will create a runbook and schedule it.


## Lab Activities

### 1. Create an Automation Account
- Enable **System-Assigned Managed Identity**
- Link a Log Analytics workspace  

### 2. Assign Required RBAC Role
```bash
az role assignment create   --assignee <principalId>   --role "Virtual Machine Contributor"   --scope /subscriptions/<subId>/resourceGroups/<rg>
```

### 3. Create a Runbook
PowerShell example:
```powershell
$rg = "dev-rg"
$vms = Get-AzVM -ResourceGroupName $rg
foreach ($vm in $vms) {
    Stop-AzVM -ResourceGroupName $rg -Name $vm.Name -Force
}
```

### 4. Publish the Runbook
- Test it manually  
- Check job output in Automation Account  

### 5. Create a Schedule
- Daily at *01:00*  
- Link to runbook  

