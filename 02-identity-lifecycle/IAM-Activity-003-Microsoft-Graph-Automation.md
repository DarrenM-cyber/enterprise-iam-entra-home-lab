# IAM Activity 003 — Microsoft Graph PowerShell Automation

**Project:** Horizon Financial Services — Enterprise IAM Home Lab  
**Platform:** Microsoft Entra ID / Microsoft Graph PowerShell  
**Status:** In Progress  
**Date Started:** November 2, 2026

## 1. Objective

Implement Microsoft Graph PowerShell to automate employee identity provisioning and group membership management in a simulated enterprise environment.

The goal is to reduce repetitive manual administration, improve consistency, and apply least-privilege principles throughout the provisioning process.

## 2. Business Scenario

Horizon Financial Services is expanding its workforce to 20 fictional employees across five departments.

Two employee identities were initially created manually. The remaining 18 employees will be provisioned using a CSV-driven PowerShell automation workflow.

## 3. Environment Preparation

- Installed and verified PowerShell 7.6.9.
- Installed Microsoft Graph Authentication, Users, and Groups modules.
- Configured a tenant-specific Microsoft Graph connection.
- Used delegated authentication with the dedicated HFS Lab Administrator account.
- Verified the connected identity using the Microsoft Graph `/me` endpoint.

## 4. Authentication and Administrator Consent

During initial connection attempts, Microsoft Graph Command Line Tools required administrator consent.

The original Global Administrator account was used to grant consent for the initial read permissions.

Verified delegated permissions:

- `User.Read.All`
- `Group.Read.All`
- `offline_access`
- `openid`
- `profile`

No user or group modification permissions have been granted as part of this initial setup.

## 5. Troubleshooting and Resolution

### Issue 1 — Administrator Approval Required

**Problem:** Microsoft Graph requested administrator approval.

**Investigation:** The dedicated HFS Lab Administrator held the User Administrator role but lacked permission to grant the requested organization-wide application consent.

**Resolution:** Used the original Global Administrator account to authorize the initial read permissions.

### Issue 2 — Security Defaults Blocked Authentication

**Problem:** A Microsoft Graph sign-in attempt failed with error code `530035`.

**Investigation:** Reviewed Microsoft Entra sign-in logs and identified Security Defaults as the blocking control.

**Resolution:** Used an interactive browser sign-in instead of the unsuccessful device-code authentication flow, without disabling Security Defaults.

### Issue 3 — Passkey Authentication Failure

**Problem:** Microsoft sign-in attempted to use a passkey that was unavailable or unsuccessful.

**Resolution:** Selected password authentication and used Microsoft Authenticator for additional verification.

### Final Validation

Successfully established a Microsoft Graph PowerShell session using the dedicated HFS Lab Administrator account.

Verified the connected identity through the Microsoft Graph `/me` endpoint.

## 6. What I Learned

### IAM Automation and Operational Efficiency

I learned that Microsoft Graph PowerShell can streamline identity management by automating repetitive tasks such as provisioning large batches of employee accounts.

Automation can reduce manual work and improve consistency, but poorly designed scripts can introduce security risks. Validation, least-privilege access, and error handling are essential.

### Authentication vs. Authorization

I learned that successfully authenticating an account does not automatically authorize it to access every application or resource.

Microsoft Graph requires appropriate delegated permissions, and some permissions require administrator consent.

### Security Defaults and Troubleshooting

While troubleshooting Microsoft Graph authentication, I discovered that Security Defaults was blocking a sign-in attempt.

Instead of disabling Security Defaults, I investigated the sign-in logs and tested an alternative authentication method.

I learned that troubleshooting should begin with solutions that have the least impact on organizational security.

### Least Privilege

I learned the importance of separating routine IAM administration from higher-privileged tenant administration.

Using a dedicated administrator account helps reduce reliance on Global Administrator privileges for everyday tasks.

### CSV Validation and Identity Provisioning

While preparing employee data for automated provisioning, I learned the importance of validating information before creating identities in Microsoft Entra ID.

Using PowerShell, I successfully imported 18 fictional employee records and verified that the CSV contained no duplicate Employee IDs or missing required fields. I also reviewed departmental security group assignments for accuracy.

I learned that automation should not blindly process incomplete or incorrect employee information.

If HR provided a spreadsheet containing duplicate Employee IDs or missing department information, I would exclude the affected records from provisioning and contact HR to obtain the correct information.

Once the errors were resolved, I would update the CSV, validate the corrected records, and provision the remaining employees through the established automation process.

I also learned the importance of documenting provisioning exceptions and communicating potential onboarding delays to the appropriate stakeholders.

**Key takeaway:** Reliable IAM automation requires accurate source data, validation controls, and a documented process for handling exceptions before accounts are created.

### Automated Access Validation

While preparing the employee provisioning workflow, I learned how to use PowerShell to validate department-to-security-group assignments against predefined organizational rules.

I created a mapping between five Horizon Financial Services departments and their corresponding Microsoft Entra security groups.

Using PowerShell, I checked all 18 employee records and confirmed that none contained an incorrect departmental group assignment.

I also used Microsoft Graph PowerShell to verify that all five security groups existed in Microsoft Entra ID.

During testing, I encountered a PowerShell `if/else` syntax error. I resolved the issue and learned why it is important to verify that validation logic actually executes before trusting a successful result.

**Key takeaway:** IAM automation should verify both the accuracy of employee information and the existence of required resources before provisioning accounts or assigning access.

## 7. Testing and Validation

| Test | Result |
|---|---|
| PowerShell 7 installation | PASS |
| Microsoft Graph modules installed | PASS |
| Read permissions verified | PASS |
| Sign-in error identified in Entra logs | PASS |
| Successful dedicated administrator connection | PASS |
| Imported 18 employee records from CSV |	PASS |
| Verified employee IDs HFS-003 through HFS-020 |	PASS |
| Reviewed departmental security group mappings |	PASS — Visual inspection |
| Checked for duplicate Employee IDs |	PASS |
| Checked for missing required employee fields | PASS |
| Automated department-to-group mapping validation | PASS — 18 checked, 0 invalid |
| Verified departmental security groups in Entra ID | PASS — 5 of 5 groups found |
| Automated employee provisioning | PENDING |
| Departmental group assignment automation | PENDING |
| Provisioning validation and error handling | PENDING |

## 8. Next Steps

1. Prepare a CSV containing the remaining 18 fictional employees.
2. Develop a Microsoft Graph PowerShell provisioning script.
3. Validate employee attributes and group mappings.
4. Test provisioning with a single employee.
5. Complete provisioning and verify the results.
6. Document technical evidence and lessons learned.

## 9. Evidence

Evidence screenshots will be stored in the `08-Evidence` directory and referenced through the IAM Evidence Register.

**Evidence pending preparation and redaction.**
