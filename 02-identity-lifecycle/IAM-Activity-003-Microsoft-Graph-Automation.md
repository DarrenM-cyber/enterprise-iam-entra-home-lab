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

### PowerShell Pre-Provisioning Validation

I developed and successfully executed the first three validation checks in my reusable PowerShell employee provisioning script.

The script imports 18 employee records from a CSV file, checks for duplicate Employee IDs, and verifies that all required employee information is present.

I learned how to use PowerShell commands such as `Import-Csv`, `Group-Object`, `Where-Object`, and `throw` to validate employee data and stop automation when errors are detected.

Most importantly, I learned that inaccurate employee information can create identity conflicts, incorrect authorization, security risks, and unreliable audit records.

**Key takeaway:** IAM automation must validate identity data before provisioning accounts or assigning access to help enforce least privilege and maintain accurate identity records.

**Validation results:** 18 employees imported, zero duplicate Employee IDs, and all required fields validated successfully.

### Identity Uniqueness and Directory Validation

I expanded my PowerShell provisioning script to perform six pre-provisioning validation checks against employee CSV data and Microsoft Entra ID.

Using Microsoft Graph PowerShell, I retrieved existing directory users and compared their Employee IDs and User Principal Names (UPNs) against 18 incoming employee records.

I learned that validating unique Employee IDs within a CSV is not sufficient. Automation must also check for existing directory identities and username conflicts to reduce the risk of duplicate accounts and incorrect access assignments.

I implemented validation logic that stops the provisioning process when identity conflicts are detected rather than automatically modifying usernames or overwriting existing accounts.

**Validation results:** All six checks passed, with 18 employee records validated and no existing Employee ID or username conflicts detected.

**Key takeaway:** Reliable IAM onboarding requires both accurate source data and verification against existing directory identities before provisioning begins.

### Provisioning Dry Runs and Security Group Validation

I developed a dry-run provisioning preview using PowerShell to review 18 proposed employee accounts, their departments, and their security group assignments before making changes to Microsoft Entra ID.

I expanded the automation to validate five departmental security groups against the live Entra directory, confirming their existence, unique names, and security-enabled configuration.

I also verified that all five groups use Assigned membership and are not configured to receive Microsoft Entra administrative roles.

During testing, I learned the difference between Assigned and Dynamic group membership and why direct membership automation must account for these configurations.

I gained a better understanding of the risks associated with role-assignable groups, particularly how an incorrect group assignment could grant excessive administrative privileges and violate least-privilege principles.

**Validation results:**
- 18 employee accounts successfully previewed.
- Five departmental security groups verified.
- Dynamic membership safeguard implemented.
- All five groups manually confirmed as non-role-assignable.
- Zero accounts created during testing.

**Key takeaway:** Secure IAM automation requires validating both identities and destination groups, reviewing proposed changes, and preventing unintended privileged access before executing provisioning operations.

### Microsoft Graph Troubleshooting and Fail-Closed Security

I investigated an issue where Microsoft Graph PowerShell returned a blank value for the `isAssignableToRole` property on five departmental security groups.

I used `Get-MgGroup`, `Invoke-MgGraphRequest`, and JSON output to investigate the property and discovered that Microsoft Graph returned `null` rather than an explicit `False` value.

During troubleshooting, I also corrected a PowerShell string-interpolation issue that caused an incorrectly constructed Microsoft Graph API URL.

I reviewed my delegated Microsoft Graph permissions and compared the API results with the Microsoft Entra admin center, where all five departmental groups were manually confirmed as non-role-assignable.

This exercise reinforced the importance of fail-closed security. When automation cannot verify a security-sensitive configuration, it should stop the affected operation rather than assume the configuration is safe.

**Key takeaway:** Secure IAM automation requires accurate interpretation of API responses, effective troubleshooting, and independent verification when security-critical information is unavailable.

**Current status:** All five groups manually verified as non-role-assignable. Automated role-assignability verification remains unresolved and must be addressed before automated group membership assignments are enabled.

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

## 10. Microsoft Graph Authentication & Troubleshooting

**Organization:** Horizon Financial Services (HFS)  
**Activity:** IAM-003 — Microsoft Graph Automation  
**Status:** Authentication and API permission configuration completed; automated employee provisioning in progress.

### Objective

Configure a dedicated Microsoft Entra application to support PowerShell-based identity provisioning while applying least-privilege access controls, delegated authentication, and administrator consent.

### Challenges Encountered

During implementation, several authentication and authorization issues prevented Microsoft Graph PowerShell from establishing a usable connection.

| Error / Issue | Root Cause | Resolution |
|---|---|---|
| Administrator consent required | Microsoft Graph Command Line Tools lacked the required user-creation permission | Registered a dedicated HFS automation application and obtained administrator consent |
| AADSTS50011 | Authentication redirect URI did not match the application's registered redirect URIs | Configured the appropriate desktop and localhost redirect URIs |
| AADSTS7000218 | The localhost redirect URI was configured as a Web application redirect, causing confidential-client credential requirements | Reconfigured localhost under Mobile and desktop applications |
| Windows authentication broker incompatibility | WAM authentication used a redirect URI not configured for the application | Used `Set-MgGraphOption -DisableLoginByWAM $true` to switch to browser authentication |
| 403 Authorization_RequestDenied | `User.Create` did not authorize the existing-user lookup required for duplicate detection | Added and obtained administrator consent for delegated `User.Read.All` |

### Application Registration and Security Configuration

Created a dedicated application registration named `HFS-IAM-Automation`.

Configured the following Microsoft Graph delegated permissions:

- `User.Create` — Supports creation of standard employee accounts.
- `User.Read.All` — Supports existing-user lookups and duplicate-account validation.

Both permissions received administrator consent.

Configured interactive public-client authentication with the appropriate desktop redirect URI settings. No client secret was created.

The operational HFS Lab Administrator retained its existing User Administrator role. No permanent elevation to Global Administrator was required.

### Authentication Verification

Successfully established a Microsoft Graph PowerShell session using the dedicated HFS application.

Verification command:

```powershell
Get-MgContext | Select-Object Account, AppName, ClientId, TenantId, AuthType, Scopes
```

Verified results:

- **Account:** HFS Lab Administrator
- **Application:** HFS-IAM-Automation
- **Authentication type:** Delegated
- **Verified scope:** `User.Create`
- **Tenant:** HFS Microsoft Entra tenant

After successful authentication, a user lookup returned HTTP 403 because `User.Read.All` had not yet been granted. The missing permission was subsequently configured and approved.

**Pending verification:** Reconnect with both `User.Create` and `User.Read.All`, then rerun the duplicate-account check.

### Security Principles Demonstrated

1. **Least privilege:** Selected `User.Create` rather than requesting broad user-write permissions.
2. **Separation of duties:** Kept application consent administration separate from routine user provisioning.
3. **Secure authentication:** Used interactive delegated authentication without storing administrator credentials or client secrets.
4. **Troubleshooting methodology:** Used Microsoft Entra error codes and PowerShell exception details to identify configuration problems.
5. **Provisioning safeguards:** Required duplicate-account verification before attempting account creation.

### Current Status and Next Steps

Authentication through the dedicated HFS application has been successfully verified.

Remaining work:

- Validate security-group membership requirements separately.
- Expand automation to the remaining employee records after successful testing.
## 11. Automated Employee Provisioning — HFS-003 Michael Brooks

**Organization:** Horizon Financial Services (HFS)  
**Activity:** IAM-003 — Microsoft Graph Automation  
**Provisioning Method:** PowerShell 7 and Microsoft Graph API  
**Status:** Employee account created and verified; security-group assignment pending.

### 11.1 Objective

Demonstrate automated employee onboarding using a structured CSV data source, Microsoft Graph PowerShell, delegated API permissions, and identity validation controls.

The objective was to securely provision a new employee account while preventing duplicate identities, enforcing initial password security, and verifying that employee attributes were correctly written to Microsoft Entra ID.

### 11.2 Employee Provisioning Record

| Attribute | Value |
|---|---|
| Employee ID | HFS-003 |
| Display Name | Michael Brooks |
| User Principal Name | `michael.brooks@mooredarrenc87yahoo.onmicrosoft.com` |
| Department | Human Resources |
| Job Title | Recruiter |
| Intended Security Group | `SG-HFS-HR` |
| Account Status | Enabled |
| Group Membership | Pending verification and assignment |

Employee attributes were imported from `HFS-Employees.csv`, reducing manual data entry and supporting a repeatable onboarding workflow.

### 11.3 Pre-Provisioning Security Controls

The following controls were completed before account creation:

1. **Username uniqueness:** Queried Microsoft Graph for the intended User Principal Name. The API returned HTTP 404, indicating that the requested username was not found.
2. **Employee ID uniqueness:** Queried the directory for `HFS-003`. No matching user was returned.
3. **CSV validation:** Confirmed the employee's name, Employee ID, department, job title, and intended security group.
4. **Required attribute validation:** Verified that all required account attributes were populated.
5. **Password validation:** Generated a temporary password using a cryptographic random number generator and validated its length and character composition.
6. **First-sign-in protection:** Configured `ForceChangePasswordNextSignIn` as `true`.
7. **Least-privilege authorization:** Used the dedicated `HFS-IAM-Automation` application with delegated `User.Create` and `User.Read.All` permissions.

### 11.4 Account Creation

Provisioned the employee using the Microsoft Graph PowerShell `New-MgUser` cmdlet and account parameters derived from the employee CSV.

```powershell
$createdUser = New-MgUser @newUserParams -ErrorAction Stop
```

The operation completed successfully and returned the new Microsoft Entra user object's identifier.

**Security note:** The password-bearing parameter object and temporary password were not displayed or recorded in the repository.

### 11.5 Independent Post-Provisioning Verification

Retrieved the newly created user from Microsoft Entra ID using Microsoft Graph.

```powershell
Get-MgUser -UserId $createdUser.Id `
    -Property "displayName,userPrincipalName,employeeId,department,jobTitle,accountEnabled" |
    Select-Object DisplayName, UserPrincipalName, EmployeeId, Department, JobTitle, AccountEnabled |
    Format-List
```

| Verified Attribute | Result |
|---|---|
| Display Name | Michael Brooks — PASS |
| User Principal Name | Expected username — PASS |
| Employee ID | HFS-003 — PASS |
| Department | Human Resources — PASS |
| Job Title | Recruiter — PASS |
| Account Enabled | True — PASS |

**Verification result: 6 of 6 account attributes matched expected values.**

### 11.6 Credential Handling

The temporary password was generated locally and used only for the provisioning operation.

After successful account verification, the temporary password and password-bearing parameter variables were removed from the active PowerShell session.

No passwords, access tokens, or authentication secrets were committed to GitHub.

### 11.7 Evidence References

- **IAM-003-E08:** Duplicate username and Employee ID validation.
- **IAM-003-E09:** Successful account creation and independent identity attribute verification.

Supporting screenshots will be stored in the `08-Evidence` directory and tracked in the IAM Evidence Register.

### 11.8 Lessons Learned

- Identity provisioning should validate existing directory records before creating new accounts.
- Employee IDs and usernames provide complementary duplicate-detection controls.
- CSV-driven provisioning improves consistency and reduces manual entry errors.
- Successful API responses should be followed by independent verification of stored directory attributes.
- Temporary credentials require careful handling throughout the provisioning lifecycle.
- Account creation and security-group assignment should be validated as separate operations.
- A provisioning script should support safe reruns and avoid duplicating accounts after partial completion.

### 11.9 Remaining Work

- Verify the intended HR security group's eligibility for automated membership management.
- Obtain and validate appropriate Microsoft Graph group-management permissions.
- Assign Michael Brooks to `SG-HFS-HR` and independently verify membership.
- Implement safe, repeatable provisioning for the remaining employee records.
- Publish sanitized provisioning evidence and update the IAM Evidence Register.

**Milestone achieved:** Successfully created and independently verified the first CSV-driven employee identity in the Horizon Financial Services Microsoft Entra IAM lab.

**Portfolio outcome:** Demonstrated the configuration and troubleshooting of Microsoft Entra application authentication, delegated Microsoft Graph permissions, administrator consent, and least-privilege access for an enterprise IAM automation workflow.
## 12. Automated Security Group Assignment — HFS-003 Michael Brooks

**Organization:** Horizon Financial Services (HFS)  
**Activity:** IAM-003 — Microsoft Graph Automation  
**Employee:** HFS-003 — Michael Brooks  
**Target Security Group:** `SG-HFS-HR`  
**Status:** Successfully assigned and independently verified.

### 12.1 Objective

Extend the employee onboarding process beyond account creation by assigning the employee to the appropriate departmental security group using Microsoft Graph PowerShell.

Demonstrate identity lifecycle management, role-based access control (RBAC) principles, least-privilege API permissions, duplicate membership prevention, and independent verification of access assignments.

### 12.2 Microsoft Graph Permission Configuration

Configured the dedicated `HFS-IAM-Automation` application with the delegated Microsoft Graph permission `GroupMember.ReadWrite.All`.

Administrator consent was granted for the Microsoft Entra tenant.

Verified the authenticated Microsoft Graph session using `Get-MgContext`.

| Configuration | Verified Value |
|---|---|
| Application | HFS-IAM-Automation |
| Authentication Type | Delegated |
| Administrator Account | HFS Lab Administrator |
| Group Management Permission | `GroupMember.ReadWrite.All` |
| Administrator Consent | Granted |

**Security consideration:** `GroupMember.ReadWrite.All` permits group membership operations beyond the HR group, subject to the signed-in user's privileges. Production implementations should enforce additional authorization boundaries and approved-group restrictions.

### 12.3 Security Group Validation

Before performing the membership assignment, verified the target security group in Microsoft Entra ID.

| Property | Verified Value |
|---|---|
| Group Name | SG-HFS-HR |
| Group Type | Security |
| Membership Type | Assigned |
| Source | Cloud |
| Microsoft Entra Role Assignable | No — verified in Entra admin center |
| Existing Direct Members | 2 before assignment |

The group was also retrieved through Microsoft Graph to validate its identity and security properties.

**Technical limitation:** Microsoft Graph returned a null value for `IsAssignableToRole`. The group was independently verified as non-role-assignable through the Microsoft Entra admin center.

For this controlled single-user lab operation, the assignment was restricted to the previously verified HR group's exact object ID.

A future bulk provisioning script must fail closed when privileged-group eligibility cannot be reliably established.

### 12.4 Duplicate Membership Prevention

Before assigning the employee, queried the target group's direct membership using Microsoft Graph.

```powershell
Get-MgGroupMember -GroupId $hrGroups[0].Id -All -ErrorAction Stop |
    Where-Object { $_.Id -eq $createdUser.Id } |
    Select-Object Id
```

The query returned no matching records, confirming that Michael Brooks was not already a direct member of the HR security group.

This check helps prevent unnecessary membership requests and supports repeatable onboarding operations.

### 12.5 Pre-Assignment Safety Validation

Validated the target group's identity and configuration before executing the membership change.

The checks included:

1. Confirming that the employee's Microsoft Entra object ID was available.
2. Confirming that exactly one matching HR security group was returned.
3. Verifying the expected group display name.
4. Confirming that the group was security-enabled.
5. Confirming that the group did not use dynamic membership.
6. Comparing the group's object ID against the independently verified HR group identifier.
7. Confirming the group's non-role-assignable status through the Microsoft Entra admin center.

**Validation result:** `READY: HR group assignment target validated.`

### 12.6 Automated Security Group Assignment

Executed the Microsoft Graph PowerShell `New-MgGroupMemberByRef` cmdlet to add Michael Brooks to the HR security group.

```powershell
New-MgGroupMemberByRef `
    -GroupId $hrGroups[0].Id `
    -BodyParameter @{
        "@odata.id" = "https://graph.microsoft.com/v1.0/directoryObjects/$($createdUser.Id)"
    } `
    -ErrorAction Stop
```

**Execution result:** Microsoft Graph completed the membership request without an error.

### 12.7 Independent Membership Verification

After the assignment, queried the group's direct membership again.

```powershell
$membership = @(
    Get-MgGroupMember -GroupId $hrGroups[0].Id -All -ErrorAction Stop |
        Where-Object { $_.Id -eq $createdUser.Id }
)

if ($membership.Count -eq 1) {
    Write-Host "PASS: Michael Brooks is a direct member of SG-HFS-HR."
} else {
    Write-Host "FAIL: Expected HR group membership not confirmed."
}
```

**Verification output:**

```text
PASS: Michael Brooks is a direct member of SG-HFS-HR.
```

This independently confirmed that the intended access assignment was saved in Microsoft Entra ID.

### 12.8 Security Controls Demonstrated

- **Least-privilege principles:** Used delegated Microsoft Graph permissions and a dedicated automation application.
- **Group eligibility validation:** Verified security-group type, assigned membership, and non-role-assignable status.
- **Duplicate prevention:** Checked existing direct membership before attempting the assignment.
- **Target restriction:** Used the exact verified HR group object ID for the controlled operation.
- **Independent verification:** Queried Microsoft Entra after the change to confirm membership.
- **Separation of operations:** Validated account creation and group assignment independently.

### 12.9 Evidence

**IAM-003-E10 — Automated HR Security Group Assignment**

Evidence includes:

- Microsoft Graph delegated group-management permission and administrator consent.
- HR security-group configuration verification.
- Successful group-membership assignment.
- Independent PowerShell verification confirming Michael Brooks's direct membership.

Supporting screenshots are tracked in the IAM Evidence Register.

### 12.10 Lessons Learned

1. Employee onboarding requires both identity creation and appropriate access assignment.
2. Group membership should be checked before attempting to add a user.
3. API permissions and directory administrator roles are separate authorization controls.
4. Role-assignable groups require additional safeguards because membership can convey administrative privileges.
5. A successful API request should be followed by an independent read operation.
6. A reusable provisioning workflow should handle partial failures and safely resume without creating duplicate accounts or memberships.

### 12.11 Current Status and Next Steps

**Completed:**

- Created HFS-003 Michael Brooks using Microsoft Graph PowerShell.
- Verified six employee account attributes.
- Assigned Michael Brooks to `SG-HFS-HR`.
- Independently verified his direct HR group membership.

**Remaining:**

- Consolidate employee creation and group assignment into a reusable PowerShell workflow.
- Implement reliable fail-closed privileged-group checks for bulk automation.
- Add idempotency and error handling for partially completed onboarding operations.
- Test and provision the remaining 17 employees.
- Publish the IAM-003-E10 evidence and update the IAM Evidence Register.

**Milestone achieved:** Successfully completed and verified the first Microsoft Graph PowerShell employee onboarding workflow, including identity provisioning and departmental security-group assignment.
