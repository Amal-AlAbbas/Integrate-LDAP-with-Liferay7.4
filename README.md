# How to Integrate LDAP with Liferay DXP 7.4

## Overview
This guide provides a detailed, beginner-friendly approach to integrating **Liferay DXP 7.4** with an **LDAP server** for authentication and user management. It explains each step, highlights common pitfalls, and compares LDAP filters to help you optimize your configuration.

---

## Table of Contents
1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Step-by-Step Configuration](#step-by-step-configuration)
   - [Connect Liferay to LDAP](#connect-liferay-to-ldap)
   - [User Mapping](#user-mapping)
   - [Group Mapping](#group-mapping)
   - [Testing the Connection](#testing-the-connection)
4. [Understanding LDAP Filters](#understanding-ldap-filters)
   - [Import All Users](#to-import-all-users)
   - [Import Specific User by sAMAccountName](#to-import-a-specific-user-by-their-samaccountname)
   - [Optimize User Import for Active Directory](#to-optimize-user-import-for-active-directory)
   - [Import All Groups](#to-import-all-groups)
   - [Import Specific Group Named Employees](#to-import-a-specific-group-named-employees)
5. [Troubleshooting](#troubleshooting)
   - [Connection Fails](#connection-fails)
   - [Users or Groups Not Syncing](#users-or-groups-not-syncing)
   - [Authentication Issues](#authentication-issues)
6. [Example Use Cases](#example-use-cases)
   - [Sync All Users](#scenario-1-sync-all-users)
   - [Sync Users in a Specific Group](#scenario-2-sync-users-in-a-specific-group)
7. [License](#license)

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

### Connect Liferay to LDAP
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

### User Mapping
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

### Group Mapping
Configure group synchronization if required:
1. Go to **Group Mapping**.
2. Map the following attributes:
   - **Group Name**: `cn`.
   - **Description**: `description` (optional).
   - **Users**: `member`.

3. Add an **Import Search Filter** to filter specific groups:
   ```plaintext
   (objectClass=group)
   ```

---

### Testing the Connection

#### Test LDAP Connection
To verify the connection between Liferay and your LDAP server:
1. Click the **Test LDAP Connection** button in the Liferay LDAP settings.
2. If the connection fails:
   - Check the LDAP server address, credentials, and Base DN for accuracy.
   - Ensure the LDAP server is reachable from the Liferay server (e.g., no firewalls blocking the connection).

#### Test LDAP Users
1. Use the **Test LDAP Users** button to verify that user synchronization settings are correct.
2. If users are not being imported, adjust the **User Import Filter**. A basic filter example is:
   ```plaintext
   (objectClass=person)
   ```

#### Test LDAP Groups
1. Use the **Test LDAP Groups** button to verify that group synchronization settings are correct.
2. If groups are not being imported, adjust the **Group Import Filter**. A common filter example is:
   ```plaintext
   (objectClass=group)
   ```

---

## Understanding LDAP Filters

LDAP filters allow you to control which users and groups are imported into Liferay. Here are some common filter examples:

### To Import All Users:
```plaintext
(objectClass=person)
```

### To Import a Specific User by Their sAMAccountName:
```plaintext
(&(objectClass=person)(sAMAccountName=username))
```

### To Optimize User Import for Active Directory:
```plaintext
(objectCategory=person)
```

### To Import All Groups:
```plaintext
(objectClass=group)
```

### To Import a Specific Group Named Employees:
```plaintext
(&(objectClass=group)(cn=employees))
```

---

## Troubleshooting

### Connection Fails
- Verify the **Base Provider URL**, **Base DN**, and credentials.
- Ensure there are no firewall rules blocking the connection.
- Check the Liferay logs for specific error messages.

### Users or Groups Not Syncing
- Double-check your filters to ensure they are not too restrictive.
- Ensure that attributes like `sAMAccountName` or `cn` exist in your LDAP schema.

### Authentication Issues
- Confirm that the **Authentication Search Filter** is correctly configured. A common example:
   ```plaintext
   (&(objectClass=person)(sAMAccountName=@user_id@))
   ```

---

## Example Use Cases

### Scenario 1: Sync All Users
**User Filter:**
```plaintext
(objectClass=person)
```

### Scenario 2: Sync Users in a Specific Group
**User Filter:**
```plaintext
(&(objectClass=person)(memberOf=cn=employees,dc=example,dc=com))
```
**Group Filter:**
```plaintext
(objectClass=group)
```

---

## License
This guide is open-sourced under the MIT license. Feel free to contribute or provide suggestions!

