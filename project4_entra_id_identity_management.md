# Project 4: Entra ID Integration and Identity Management

**Topics Covered**: Entra ID, User and Group Management, App Registration, Conditional Access, MFA  

**Time**: ~0.75 hours

---

## Summary

This project demonstrates integrating applications with Entra ID, managing users and groups, setting up conditional access policies, enabling multi-factor authentication (MFA), and monitoring authentication activity.

## Scenario

A company wants to integrate their web application with Entra ID for authentication, manage user and group permissions, enforce conditional access policies, and enable MFA for added security.

## Steps

### 1. Create Users and Groups in Entra ID

- Navigate to **Entra ID** > **Users** > **New user**.
- Create a few users with different intended roles (e.g., Admin, Developer, Reader).
- Navigate to **Entra ID** > **Groups** > **New group**:
  - Create security groups (e.g., `App-Admins`, `App-Developers`, `App-Readers`).
  - Add the newly created users to the relevant groups.

### 2. Register an Application in Entra ID

- Go to **Entra ID** > **App registrations** > **New registration**.
- Fill in:
  - Name (e.g. `WebApp-AZ104-Lab`).
  - Supported account types (e.g., single tenant).
  - Redirect URI (e.g., `https://localhost:5001/signin-oidc` or as required by your app).
- After creation:
  - Go to **API permissions**:
    - Add permission: **Microsoft Graph** > **Delegated permissions** > select `User.Read` (at minimum).
  - Go to **Certificates & secrets**:
    - Create a **client secret**; copy the value and store it securely.

### 3. Assign Users and Groups to the Application

- Go to **Entra ID** > **Enterprise applications** > find your newly registered app.
- Navigate to **Users and groups** > **Add user/group**.
- Assign the previously created groups (`App-Admins`, etc.) to the application.
- This allows you to use group claims or role-based access in the application code.

### 4. Configure Conditional Access

- Navigate to **Entra ID** > **Security** > **Conditional access**.
- Click **+ New policy**:
  - Name: `Require-MFA-For-App`.
  - **Assignments**:
    - Users: select all users or a pilot group.
    - Cloud apps: select the newly registered application.
  - **Conditions** (optional):
    - Sign-in risk, location, device platform.
  - **Access controls**:
    - Grant: **Require multi-factor authentication**.
- Enable the policy and test by logging in with different lab users.

### 5. Enable Multi-Factor Authentication (MFA)

- Go to **Entra ID** > **Users** > **Per-user MFA** (or **Security** > **Authentication methods**, depending on UI version).
- Enable MFA for the lab users you created.
- Log in as one of these users and complete MFA registration (e.g., Microsoft Authenticator app).
- Verify that access to the application requires MFA according to your conditional access rules.

### 6. Monitor Sign-In Activity

- Navigate to **Entra ID** > **Sign-ins**.
- Filter by your lab users or by the application.
- Review:
  - Successful and failed sign-ins.
  - Conditional Access status (e.g., whether MFA was required).
- Use this data to validate that your policies work as expected.
