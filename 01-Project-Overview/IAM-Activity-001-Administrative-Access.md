# IAM Activity 001 – Administrative Identity and RBAC Configuration

**Project:** Horizon Financial Services – Enterprise IAM Home Lab  
**Phase:** 1 – Identity Lifecycle Management  
**Platform:** Microsoft Entra ID  
**Status:** Completed – Initial Validation

## 1. Objective

Create and configure a dedicated administrative identity in Microsoft Entra ID and implement role-based access control using the principle of least privilege.

## 2. Business Purpose

Organizations must control administrative privileges to reduce the risk of unauthorized changes, accidental misconfigurations, and excessive access.

Separating administrative identities from personal accounts and assigning only necessary permissions helps establish accountability and reduce security exposure.

## 3. Environment and Tools

- Microsoft Entra ID (Free tier during initial setup)
- Microsoft Entra Admin Center
- Microsoft Authenticator
- Google Chrome Incognito mode
- Built-in User Administrator role

## 4. Implementation

1. Accessed Microsoft Entra using an existing Global Administrator account.
2. Created a dedicated administrative identity named HFS Lab Administrator.
3. Generated and subsequently reset the account's temporary password.
4. Successfully authenticated using the new identity.
5. Registered Microsoft Authenticator as an authentication method.
6. Assigned the built-in User Administrator role at the directory scope.
7. Opened a separate browser session to validate the assigned permissions.

## 5. Testing and Validation

**Test 1 – Account Authentication**

Expected result: The new account successfully authenticates to Microsoft Entra.

Actual result: Successfully accessed the Microsoft Entra dashboard.

Outcome: PASS

**Test 2 – Authentication Method Registration**

Expected result: Microsoft Authenticator is registered to the administrative identity.

Actual result: Microsoft Authenticator appeared in the account's Security Info.

Outcome: PASS

**Test 3 – Role Assignment**

Expected result: The account receives the User Administrator role.

Actual result: Microsoft Entra displayed the built-in User Administrator role with a direct directory assignment.

Outcome: PASS

**Test 4 – User Provisioning Access**

Expected result: The User Administrator account can access the user creation interface.

Actual result: The Create new user option was visible.

Outcome: PASS – Interface access confirmed; actual provisioning not yet tested.

## 6. Evidence

- E-010: User Administrator role assignment screenshot
- E-011: User creation interface availability screenshot
- E-012: Microsoft Authenticator registration confirmation (sanitized screenshot, optional)

## 7. Issues and Troubleshooting

The initial automatically generated password was not captured during account creation.

Resolution: Reset the account password through the Microsoft Entra Admin Center and completed the initial sign-in successfully.

## 8. Results

Created a dedicated administrative identity, registered an authentication method, assigned a restricted administrative role, and validated initial user management access.

## 9. Lessons Learned

- Microsoft Entra provides built-in administrative roles.
- RBAC allows organizations to delegate administrative duties.
- User Administrator provides more limited privileges than Global Administrator.
- Authentication method registration and administrative authorization are separate security controls.
- Role assignments should be tested after implementation.

## 10. Real-World IAM Application

An IAM analyst may provision administrative accounts, assign approved roles, verify authentication controls, and document access assignments to support least privilege and security auditing.

## Remaining Validation

- Verify successful user provisioning.
- Verify restrictions on higher-privileged administrative operations.
- Test MFA enforcement through an appropriate policy.
- Evaluate temporary privileged access using PIM after P2 licensing is configured.

---

**Documentation Status:** Initial validation complete.
