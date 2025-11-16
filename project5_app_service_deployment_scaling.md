# Project 5: Azure App Service Deployment and Scaling

**Topics Covered**: App Service, Deployment Slots, Autoscaling, Backup, Monitoring  

**Time**: ~0.75 hours

---

## Summary

This project focuses on deploying a web application using Azure App Service, utilizing deployment slots for testing, configuring autoscaling based on metrics, and setting up backups.

## Scenario

A company wants to deploy a web application using Azure App Service, set up deployment slots for testing, enable autoscaling based on demand, and configure backups for disaster recovery.

## Steps

### 1. Create an App Service

- Navigate to **App Services** > **Create**.
- Select the subscription and resource group.
- Enter a unique name for the app (e.g., `az104-lab-webapp-<yourname>`).
- Choose a **Runtime stack** (e.g., .NET, Node.js, Python).
- Select a **Region** close to your users.
- Choose a suitable **App Service plan / pricing tier** (e.g., `B1` or `S1`).
- Review and create the App Service.

### 2. Deploy the Web Application

- Use **Azure DevOps** or **GitHub Actions** (or even local Git/ZIP deploy) to deploy a sample web application:
  - Example: a simple “Hello from Azure App Service” web app.
- Once deployed, open the app’s **URL** in your browser and verify it responds.

### 3. Set Up Deployment Slots

- Go to your **App Service** > **Deployment slots**.
- Click **+ Add Slot**.
  - Name: `staging`.
  - Optionally clone configuration from the production slot.
- Deploy a **new version** of the application to the `staging` slot (e.g., with different UI text).
- Verify the `staging` slot’s URL shows the new version.
- Perform a **Swap**:
  - From `staging` to `production`.
  - Test that the production URL now shows the updated version from staging.

### 4. Configure Autoscaling

- Navigate to the **App Service Plan** that hosts your app.
- Go to **Scale out (App Service plan)**.
- Enable **Custom autoscale**:
  - Minimum instance count: `1`.
  - Maximum instance count: e.g. `3`.
  - Add a rule:
    - Scale out by 1 instance when **CPU percentage > 70%** for 10 minutes.
    - Optionally, scale in when CPU < 40% for a period.
- Save the autoscale settings.
- Optionally generate load (e.g., via load testing tools) to observe scaling behavior.

### 5. Backup and Restore

- Go to your **App Service** > **Backups**.
- Click **Configure**:
  - Select a **Storage account** and **container** for storing backups.
  - Choose the database (if any) linked to your app.
  - Set a **backup schedule** (e.g., daily at a certain time).
- Run a **manual backup** to validate the configuration.
- Test restore:
  - Restore to a new App Service or to a deployment slot (recommended for testing).

### 6. Monitor and Configure Alerts

- Go to **App Service** > **Monitoring** > **Metrics**.
- View metrics such as:
  - CPU percentage.
  - HTTP server errors.
  - Requests.
- Click **New alert rule**:
  - Scope: your App Service.
  - Condition: e.g., HTTP 5xx errors > 5 over 5 minutes.
  - Action group: email, SMS, or other notifications.
- Save the alert rule and validate it by simulating failures (if possible in a safe test environment).
