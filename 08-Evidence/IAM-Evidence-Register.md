# IAM Evidence Register

**Project:** Horizon Financial Services – Enterprise IAM Home Lab  
**Owner:** Darren M.  
**Platform:** Microsoft Entra ID

## Evidence Tracking

| Evidence ID | Phase | Evidence Description | File Name | Date | Status |
|---|---|---|---|---|---|
| E-001 | Project Overview | IAM architecture diagram | TBD | TBD | Pending |
| E-002 | Identity Lifecycle | Employee onboarding configuration | TBD | TBD | Pending |
| E-003 | RBAC | Role and permissions matrix | TBD | TBD | Pending |
| E-004 | Authentication | MFA configuration | TBD | TBD | Pending |
| E-005 | Authentication | Conditional Access policy | TBD | TBD | Pending |
| E-006 | Authentication | PIM configuration | TBD | TBD | Pending |
| E-007 | Monitoring | Sign-in investigation | TBD | TBD | Pending |
| E-008 | Governance | Access review results | TBD | TBD | Pending |
| E-009 | Governance | Audit documentation | TBD | TBD | Pending |
| E-010 | Administrative Access | User Administrator role assignment | E-010-User-Administrator-Role.png | 2026-10-31 | Published |
| E-011 | Identity Lifecycle | User creation interface access verified | E-011-User-Creation-Permission.png | 2026-10-31 | Published |
| E-012 | Authentication Security | Microsoft Authenticator registration verified | E-012-Authenticator-Registration.png | 2026-10-31 | Published |
| E-013 | RBAC | Five departmental security groups created | E-013-Department-Security-Groups.png | 2026-10-31 | Published |
| E-014A | Identity Lifecycle | Sarah Mitchell employee identity properties verified | E-014A-Employee-Identity-Properties.png | 2026-10-31 | Published |
| E-014B | Identity Lifecycle | Sarah Mitchell HR group membership verified | E-014B-Employee-Group-Membership.png | 2026-10-31 | Published |
| E-015 | Identity Lifecycle | Employee manager relationship configured and verified | E-015-Employee-Manager-Relationship.png | 2026-10-31 | Published |
| E-016 | Microsoft Graph Automation | Verified PowerShell 7.6.9 and Microsoft Graph Authentication, Users, and Groups modules v2.41.2 | E-016-PowerShell-Graph-Modules-Verified.png | 2026-11-02 | Published |
| E-017 | Microsoft Graph Automation | Verified delegated Microsoft Graph permissions and administrator consent | E-017-Microsoft-Graph-Admin-Consent.png | 2026-11-02 | Published |
| E-018 | Microsoft Graph Automation | Verified dedicated HFS Lab Administrator identity through Microsoft Graph /me endpoint | E-018-Microsoft-Graph-Connection-Verified.png | 2026-11-02 | Published |
| E-019 | Microsoft Graph Automation | Investigated Microsoft Graph sign-in failure and identified Security Defaults as the blocking control | E-019-Security-Defaults-Authentication-Troubleshooting.png | 2026-11-02 | Published |

## IAM Activity 003 — Microsoft Graph Authentication & Troubleshooting

**Organization:** Horizon Financial Services (HFS)  
**Activity:** IAM-003 — Microsoft Graph Automation  
**Status:** Authentication completed; employee provisioning in progress.

### Evidence Tracking

| Evidence ID | Description | Verification Status | Date | Screenshot Status |
|---|---|---|---|
| IAM-003-E01 | HFS-IAM-Automation application registration created in Microsoft Entra ID | Verified | 2026-11-04 | Published |
| IAM-003-E02 | Delegated `User.Create` permission granted administrator consent | Verified | 2026-11-04 | Published |
| IAM-003-E03 | Redirect URI mismatch (`AADSTS50011`) investigated and corrected | Verified | 2026-11-04 | Published |
| IAM-003-E04 | Public-client authentication error (`AADSTS7000218`) investigated and corrected | Verified | 2026-11-04 | Published |
| IAM-003-E05 | Microsoft Graph PowerShell authenticated using HFS-IAM-Automation | Verified | 2026-11-04 | Published |
| IAM-003-E06 | `Get-MgContext` confirmed HFS Lab Administrator, delegated authentication, and `User.Create` scope | Verified | 2026-11-04 | Published |
| IAM-003-E07 | `User.Read.All` configured and granted administrator consent following a 403 authorization error | Verified | 2026-11-04 | Published |
| IAM-003-E08 | Reconnect with both scopes and validate existing-user lookup | Verified | 2026-11-05 | Published |
| IAM-003-E09 | Provision and verify HFS-003 Michael Brooks | Verified | 2026-11-05 | Published |
| IAM-003-E10 | Automated HR security-group assignment and independent verification of Michael Brooks's direct membership in SG-HFS-HR | Verified | 2026-11-05 | Published |

### Evidence Handling Requirements

- Redact authentication tokens, authorization codes, passwords, and other credentials before uploading screenshots.
- Avoid exposing browser callback URLs or sensitive authentication details.
- Verify that screenshots do not contain confidential information.
- Store approved evidence files in the `08-Evidence` directory.
- Update screenshot statuses and file references after evidence is uploaded.

### Key Security Controls Demonstrated

- Least-privilege Microsoft Graph delegated permissions
- Administrator consent management
- Dedicated application registration
- Interactive authentication without stored client secrets
- Troubleshooting of authentication and authorization failures
- Duplicate-account checks before automated provisioning

**Note:** Verification status reflects completed lab actions. Screenshot status remains pending until sanitized evidence files are uploaded to GitHub.

## Evidence Standards

Every evidence item should:

1. Have a unique evidence ID.
2. Identify the IAM phase it supports.
3. Include a descriptive filename.
4. Show the relevant configuration, test, or result.
5. Be associated with an activity report when appropriate.

## Security and Privacy

- Never include passwords, access tokens, or client secrets.
- Never expose real personal information.
- Use fictional employee identities.
- Review screenshots before publishing.
- Remove sensitive tenant identifiers where appropriate.

## Evidence Status Definitions

- Pending: Evidence has not been collected.
- Collected: Evidence has been captured.
- Reviewed: Evidence has been checked for accuracy and sensitive information.
- Published: Evidence has been added to the portfolio.

---

**Last Updated:** [Date]
