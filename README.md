# How to Integrate LDAP with Liferay DXP 7.4

## Overview
This guide provides a beginner-friendly and detailed explanation of how to integrate **Liferay DXP 7.4** with an **LDAP server** for user authentication and user/group synchronization. The guide includes a comparison of LDAP filters and troubleshooting tips to ensure smooth integration.

---

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Step-by-Step Configuration](#step-by-step-configuration)
   - [1. Connect Liferay to LDAP](#1-connect-liferay-to-ldap)
   - [2. Setting Up User Mapping](#2-setting-up-user-mapping)
   - [3. Configuring Group Mapping](#3-configuring-group-mapping)
   - [4. Testing the Connection](#4-testing-the-connection)
3. [Understanding LDAP Filters](#understanding-ldap-filters)
4. [Troubleshooting Common Issues](#troubleshooting-common-issues)
5. [Example Use Cases](#example-use-cases)
6. [License](#license)

---

## Prerequisites
Before starting, ensure you have:
- Access to the Liferay Control Panel as an administrator.
- LDAP server details:
  - **Base Provider URL**: Address of your LDAP server (e.g., `ldap://your-ldap-server:389`).
  - **Base DN**: Base Distinguished Name (e.g., `dc=example,dc=com`).
  - **Principal**: A user with access to query LDAP (e.g., `cn=admin,dc=example,dc=com`).
  - **Credentials**: Password for the principal user.
- Knowledge of LDAP object classes and attributes (e.g., `sAMAccountName`, `userPrincipalName`, `memberOf`, etc.).

---

## Step-by-Step Configuration

### **1. Connect Liferay to LDAP**
1. Log in to Liferay as an administrator.
2. Navigate to **Control Panel > Instance Settings > LDAP**.
3. Enable the LDAP configuration.
4. Add a new LDAP server with the following fields:
   - **Base Provider URL**: 
     ```plaintext
     ldap://your-ldap-server:389
     ```
   - **Base DN**:
     ```plaintext
     dc=example,dc=com
     ```
   - **Principal**:
     ```plaintext
     cn=admin,dc=example,dc=com
     ```
   - **Credentials**: The password for the principal.

5. Save your changes.

---

### **2. Setting Up User Mapping**
User mapping ensures that Liferay fields match LDAP attributes. To configure:
1. Navigate to the **User Mapping** section in the LDAP settings.
2. Map the following attributes:
   - **Screen Name**: `sAMAccountName`
   - **Email Address**: `userPrincipalName` or `mail`
   - **Password**: `unicodePwd` (if supported by your LDAP server).
   - **Full Name**: `cn`
   - **First Name**: `givenName`
   - **Last Name**: `sn`

3. Save your changes.

---

### **3. Configuring Group Mapping**
Group mapping ensures that Liferay syncs user groups correctly:
1. Navigate to the **Group Mapping** section in the LDAP settings.
2. Map the following attributes:
   - **Group Name**: `cn`
   - **Description**: `description` (optional)
   - **Users**: `member`

3. Add a **Group Import Filter** to narrow down the groups you want to sync:
   ```plaintext
   (objectClass=group)
