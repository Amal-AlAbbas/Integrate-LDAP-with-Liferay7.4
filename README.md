# How to Integrate LDAP with Liferay DXP 7.4

## Overview
This guide provides a detailed, beginner-friendly approach to integrating **Liferay DXP 7.4** with an **LDAP server** for authentication and user management. It explains each step, highlights common pitfalls, and compares LDAP filters to help you optimize your configuration.

---

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Step-by-Step Configuration](#step-by-step-configuration)
   - [Connecting Liferay to LDAP](#1-connect-liferay-to-ldap)
   - [Setting Up User Mapping](#2-user-mapping)
   - [Configuring Group Mapping](#3-group-mapping)
   - [Testing the Connection](#4-testing-the-connection)
3. [LDAP Filters Explained](#ldap-filters-explained)
4. [Troubleshooting](#troubleshooting)

---

## Prerequisites
Before you begin, ensure you have the following:
- Access to the Liferay Control Panel as an administrator.
- LDAP server details:
  - **Base Provider URL**: Address of your LDAP server (e.g., `ldap://your-ldap-server:389`).
  - **Base DN**: The distinguished name to use as the base (e.g., `dc=example,dc=com`).
  - **Principal**: A user with access to query LDAP (e.g., `cn=admin,dc=example,dc=com`).
  - **Credentials**: The password for the principal user.
- Knowledge of LDAP object classes and attributes (e.g., `sAMAccountName`, `userPrincipalName`, etc.).

---

## Step-by-Step Configuration

### **1. Connect Liferay to LDAP**
1. Log in to Liferay as an administrator.
2. Navigate to **Control Panel > Instance Settings > LDAP**.
3. Enable the LDAP configuration.
4. Add a new LDAP server and fill in the following details:
   - **Base Provider URL**: 
     ```
     ldap://your-ldap-server:389
     ```
   - **Base DN**:
     ```
     dc=example,dc=com
     ```
   - **Principal**: 
     ```
     cn=admin,dc=example,dc=com
     ```
   - **Credentials**: The password for the principal.

5. Save the configuration and proceed to testing.

---

### **2. User Mapping**
Configure how Liferay maps its users to LDAP attributes:
1. Go to **User Mapping**.
2. Map the following attributes:
   - **Screen Name**: `sAMAccountName` (or another unique identifier).
   - **Email Address**: `userPrincipalName` or `mail`.
   - **Password**: `unicodePwd` (ensure your LDAP supports this).
   - **Full Name**: `cn`.
   - **First Name**: `givenName`.
   - **Last Name**: `sn`.

3. Save the configuration.

---

### **3. Group Mapping**
Configure group synchronization if required:
1. Go to **Group Mapping**.
2. Map the following attributes:
   - **Group Name**: `cn`.
   - **Description**: `description` (optional).
   - **Users**: `member`.

3. Add an **Import Search Filter** to filter specific groups:
   ```plaintext
   (objectClass=group)
