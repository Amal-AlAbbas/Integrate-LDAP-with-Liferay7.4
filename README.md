# Integrate LDAP with Liferay

## Overview
This guide provides step-by-step instructions to integrate **Liferay DXP 7.4** with an **LDAP server** for user authentication and synchronization.

---

## Prerequisites
- Access to Liferay Control Panel as an administrator.
- LDAP server details (e.g., `Base DN`, `Provider URL`, and credentials).
- Liferay instance (DXP 7.4).

---

## Steps

### 1. Configure LDAP in Liferay
1. Go to **Control Panel > Instance Settings > LDAP**.
2. Enable the **LDAP Configuration**.
3. Add a new LDAP server with the following details:
   - **Base Provider URL**: `ldap://your-server:389`
   - **Base DN**: `dc=yourdomain,dc=com`
   - **Principal**: `cn=admin,dc=yourdomain,dc=com`
   - **Credentials**: Your admin password.

### 2. User Mapping
1. Set the user mappings as follows:
   - **Screen Name**: `sAMAccountName`
   - **Email Address**: `userPrincipalName`
   - **Password**: `unicodePwd`
   - **Full Name**: `cn`

### 3. Group Mapping
1. Configure group mappings as needed:
   - **Group Name**: `cn`
   - **Import Filter**: `(objectClass=group)`

### 4. Test the Connection
1. Use the **Test LDAP Connection** button to verify connectivity.
2. Test **LDAP Users** and **LDAP Groups** to ensure proper synchronization.

---

## Troubleshooting
- Check the logs in Liferay for any LDAP-related errors.
- Ensure the LDAP server is accessible and the credentials are correct.

---

## License
This guide is open-sourced under the MIT license.
