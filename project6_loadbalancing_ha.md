# Chapter 6: Load Balancing & High Availability

## Overview
This chapter explains how to design and deploy highly available solutions using Azure Load Balancer, Virtual Machine Scale Sets, Availability Zones, and failover validation. You will learn how Azure distributes traffic, monitors instance health, and maintains application availability during failures.

## What You Will Learn
- Standard vs Basic Load Balancer  
- VM Scale Sets (VMSS) with autoscaling  
- Availability Zones + redundancy  
- Health probes & load balancing rules  
- Validating failover and high availability  

## Scenario
A company needs a multi-VM web application that remains online even when a VM or an entire availability zone goes down. You will deploy a scale set, configure a load balancer, set autoscaling rules, and perform a failover test.

---

## Lab Activities

### 1. Create a Virtual Machine Scale Set (VMSS)
1. Azure Portal → **Create a resource** → **Virtual Machine Scale Set**  
2. Select:
   - Region: *Norway East*
   - Availability Zones: *1, 2, 3*
   - Image: *Ubuntu LTS*
3. Authentication: SSH key  
4. Under **Networking**:
   - Enable **Standard Load Balancer**
   - Create or select a VNet & subnet  
5. Create the VMSS.

---

### 2. Configure Standard Load Balancer
1. Open your Load Balancer → **Backend pools** → **Add**  
2. Attach your VM Scale Set as the backend.  
3. Create **HTTP Health Probe**:
   - Protocol: TCP  
   - Port: 80  
   - Interval: 5 seconds  
4. Create **Load Balancing Rule**:
   - Frontend Port: 80  
   - Backend Port: 80  
   - Health Probe: select your HTTP probe  
   - Session persistence: None  

---

### 3. Configure Autoscaling
Go to VMSS → **Scaling**  
- Scale out when **CPU > 70%** for 10 minutes  
- Scale in when **CPU < 30%**  
- Minimum instances: 2  
- Maximum instances: 10  

---

### 4. Failover & HA Validation
1. Stop one instance inside your scale set.  
2. Go to Load Balancer → **Metrics** → observe health probe results.  
3. Confirm:
   - Traffic continues to flow to healthy VMs  
   - Autoscaling triggers if load increases  

