# Chapter 10: Advanced Azure Storage

## Overview
This chapter dives into advanced Storage Account capabilities, including immutability, lifecycle management, versioning, and static website hosting.

## What You Will Learn
- Enable static website hosting  
- Configure immutability (WORM)  
- Manage lifecycle rules  
- Enable versioning & soft delete  

## Scenario
The finance team needs immutable archived logs and an internal static dashboard hosted directly in Storage.

---

## Lab Activities

### 1. Enable Static Website Hosting
Storage Account → **Static Website**  
Upload:
- index.html  
- error.html  

### 2. Configure Immutability
Container → **Access Policy**  
- Mode: **Compliance**  
- Retention: 30 days  

### 3. Enable Versioning + Soft Delete
Storage Account → **Data protection**
- Enable: *Blob versioning*  
- Enable: *Soft delete for blobs and containers*  

### 4. Create Lifecycle Rules
Create:
- Move to Cool after 30 days  
- Move to Archive after 90 days  
- Delete after 365 days  

