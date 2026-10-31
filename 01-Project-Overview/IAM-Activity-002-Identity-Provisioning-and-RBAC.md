# IAM Activity 002 – Identity Provisioning, RBAC, and Organizational Structure

**Project:** Horizon Financial Services – Enterprise IAM Home Lab  
**Platform:** Microsoft Entra ID  
**Phase:** Identity Lifecycle Management and RBAC  
**Status:** Initial Configuration Complete  
**Date:** October 31, 2026

## 1. Objective

Build the initial identity management structure for a simulated financial services organization by creating departmental security groups, provisioning employee accounts, configuring identity attributes, and establishing employee reporting relationships.

## 2. Business Scenario

Horizon Financial Services requires a centralized identity management system to support employee onboarding, departmental access assignments, and organizational accountability.

The IAM administrator must ensure that employees are provisioned with accurate identity information and assigned to the appropriate security groups based on their business roles.

## 3. Implementation

### Administrative Provisioning Access – E-011

Accessed the Microsoft Entra user creation interface using the HFS Lab Administrator account with the User Administrator role.

**Result:** Confirmed access to the user provisioning interface.

### Authentication Method Registration – E-012

Registered Microsoft Authenticator for the HFS Lab Administrator account and verified its presence in Security Info.

**Result:** Confirmed successful authentication method registration. MFA enforcement through Conditional Access has not yet been tested.

### Departmental Security Groups – E-013

Created five cloud-based security groups:

- SG-HFS-EXEC
- SG-HFS-FIN
- SG-HFS-HR
- SG-HFS-IT
- SG-HFS-OPS

Configured each group with assigned membership.

**Result:** Established the initial departmental structure for future role-based application access.

### Employee Provisioning – E-014A and E-014B

Created Sarah Mitchell as a standard employee identity and configured:

- Job Title: HR Specialist
- Department: Human Resources
- Company: Horizon Financial Services
- Employee ID: HFS-001
- Account Status: Enabled
- Group Membership: SG-HFS-HR

**Result:** Verified employee identity attributes and HR security group membership.

Also provisioned Jennifer Adams as HR Manager (HFS-002).

### Manager Relationship – E-015

Assigned Jennifer Adams as Sarah Mitchell's manager within Microsoft Entra.

**Result:** Verified the manager relationship in Sarah's identity properties.

## 4. Testing and Validation

| Test | Result |
|---|---|
| User creation interface accessible to User Administrator | PASS |
| Microsoft Authenticator registration verified | PASS |
| Five departmental security groups created | PASS |
| Sarah Mitchell's identity attributes verified | PASS |
| Sarah Mitchell's HR group membership verified | PASS |
| Jennifer Adams assigned as Sarah's manager | PASS |

## 5. What I Learned

**Identity Lifecycle Management:** Creating an employee account involves more than assigning a username and password. Accurate department, job title, and employee information supports downstream identity management processes.

**Role-Based Access Control:** Departmental security groups provide a structure for organizing access assignments. Group membership alone does not grant application permissions until access is configured.

**Least Privilege:** Business roles and administrative roles serve different purposes. An HR Manager does not automatically require administrative access to Microsoft Entra.

**Authentication and Authorization:** Registering Microsoft Authenticator establishes an available authentication method, while administrative role assignments determine authorized actions.

**Identity Governance:** Manager relationships provide organizational context that can support access approvals and access review workflows.

**Validation and Evidence:** IAM changes should be verified after implementation and documented with screenshots and test results.

## 6. Challenges and Limitations

- Initial user provisioning was performed manually.
- Departmental application permissions have not yet been configured.
- Conditional Access MFA enforcement has not yet been tested.
- Premium governance capabilities will require appropriate licensing.
- Automated provisioning and deprovisioning have not yet been implemented.

## 7. Next Steps

1. Develop CSV-driven employee provisioning using Microsoft Graph PowerShell.
2. Provision the remaining fictional employees.
3. Assign department security group memberships.
4. Validate provisioning results and handle errors.
5. Configure application access and test least-privilege restrictions.
6. Expand the lab into authentication monitoring, identity governance, and offboarding.

## 8. Evidence References

- E-011 – User Creation Permission
- E-012 – Authenticator Registration
- E-013 – Department Security Groups
- E-014A – Employee Identity Properties
- E-014B – Employee Group Membership
- E-015 – Employee Manager Relationship

All supporting screenshots are stored in the `08-Evidence` directory.
