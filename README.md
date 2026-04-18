# Project – SAML SSO with Microsoft Entra ID (BlueWave HR Portal)

![Microsoft Entra ID](https://img.shields.io/badge/Microsoft%20Entra%20ID-Enterprise%20SSO-0078D4?style=flat&logo=microsoftazure&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-success)
![IAM](https://img.shields.io/badge/Domain-SAML%20%7C%20SSO%20%7C%20Federation-blueviolet)

---

## Overview

This project configures **SAML-based Single Sign-On (SSO)** using a non-gallery Enterprise Application in Microsoft Entra ID for the fictional company **BlueWave Health**. It demonstrates real-world SSO federation including enterprise app registration, SAML metadata configuration, claims mapping, user assignment, and access validation — all documented with evidence.

---

## Environment

| Tool | Purpose |
|------|---------|
| Microsoft Entra ID | Identity Provider (IdP) |
| Azure Portal | GUI administration |
| SAML Toolkit (samltoolkit.azurewebsites.net) | Service Provider (SP) for SSO testing |
| Edge / Chrome | Browser-based SSO validation |
| Windows VM (Parallels) | Local lab environment |
| GitHub | Documentation and version control |

---

## Application – BlueWave HR Portal

A non-gallery Enterprise Application was registered in Entra ID to simulate the BlueWave Health HR portal, acting as the SAML Service Provider.

| Property | Value |
|----------|-------|
| Application Name | BlueWave HR Portal |
| Application ID | 031cf434-9250-41a5-8efa-9... |
| Object ID | c279d495-a541-472b-8be0-... |
| Created | April 10, 2026 |
| Homepage URL | https://account.activ... |

![Enterprise App Overview](Project%20Screenshots/enterprise-app-overview.png)
*BlueWave HR Portal enterprise application overview in Microsoft Entra ID*

![Enterprise App List](Project%20Screenshots/enterprise-app-list.png)
*Enterprise applications list confirming BlueWave HR Portal registration*

---

## SAML Configuration

Basic SAML settings were configured to establish the trust relationship between Entra ID (IdP) and the BlueWave HR Portal (SP).

| Field | Value |
|-------|-------|
| Identifier (Entity ID) | https://bluewave-hr-portal.com/saml/metadata |
| Reply URL (ACS URL) | https://bluewave-hr-portal.com/saml/acs |
| Sign-on URL | https://bluewave-hr-portal.com/login |
| Relay State | Optional |
| Logout URL | Optional |

![SAML Basic Config](Project%20Screenshots/saml-basic-config.png)
*Basic SAML configuration showing Entity ID, ACS URL, and Sign-on URL*

---

## Attributes & Claims Mapping

Custom claims were mapped to pass user identity attributes from Entra ID to the application in the SAML assertion.

| Claim Name | Type | Source Value |
|------------|------|-------------|
| Unique User Identifier (Name ID) | SAML | user.userprincipalname |
| department | SAML | user.department |
| givenName | SAML | user.givenname |
| emailaddress | SAML | user.mail |
| givenname (schema) | SAML | user.givenname |
| name | SAML | user.userprincipalname |
| surname | SAML | user.surname |
| jobTitle | SAML | user.jobtitle |
| surname | SAML | user.surname |

![Attributes and Claims](Project%20Screenshots/attributes-claims.png)
*Attributes & Claims configuration showing all mapped SAML claims*

---

## User Assignment

Only authorized users were assigned to the BlueWave HR Portal application, enforcing access control at the application level.

| User | Object Type | Access |
|------|------------|--------|
| Sarah Lee | User | Assigned |
| Taylor Green | User | Assigned |

![Users and Groups](Project%20Screenshots/users-and-groups.png)
*Users and groups assignment showing Sarah Lee and Taylor Green with access to BlueWave HR Portal*

---

## SSO Validation

### SAML Toolkit – Service Provider

The SAML Toolkit site at `samltoolkit.azurewebsites.net` was used to validate the SSO integration end-to-end.

![SAML Toolkit Home](Project%20Screenshots/saml-toolkit-home.png)
*SAML Toolkit for Azure AD — used as the Service Provider for SSO testing*

### Troubleshooting Scenario – John Doe (Unassigned User)

A simulated failure was created by attempting SSO as John Doe, who was not assigned to the application. This produced an access error, confirming that assignment enforcement is working correctly.

![John Doe No Access](Project%20Screenshots/john-doe-no-access.png)
*John Doe's My Apps dashboard — BlueWave HR Portal is absent due to missing application assignment*

### Resolution – Assigning John Doe

John Doe was assigned to the BlueWave HR Portal application. After assignment, the app appeared in his My Apps dashboard and SSO was validated successfully.

![John Doe Access Restored](Project%20Screenshots/john-doe-access-restored.png)
*John Doe's My Apps dashboard after assignment — BlueWave HR Portal now visible and accessible*

### Successful SSO – Sarah Lee

Sarah Lee, a pre-assigned user, successfully authenticated via SAML SSO and the BlueWave HR Portal appeared in her My Apps dashboard.

![Sarah Lee My Apps](Project%20Screenshots/sarah-lee-my-apps.png)
*Sarah Lee's My Apps dashboard showing BlueWave HR Portal — confirming successful SSO access*

---

## Screenshot Naming Reference

| File Name | Description |
|-----------|-------------|
| `Project Screenshots/enterprise-app-overview.png` | BlueWave HR Portal enterprise app overview |
| `Project Screenshots/enterprise-app-list.png` | All enterprise applications list |
| `Project Screenshots/saml-basic-config.png` | Basic SAML configuration (Entity ID, ACS URL, Sign-on URL) |
| `Project Screenshots/attributes-claims.png` | Attributes & Claims mapping |
| `Project Screenshots/users-and-groups.png` | Application user assignment (Sarah Lee, Taylor Green) |
| `Project Screenshots/saml-toolkit-home.png` | SAML Toolkit for Azure AD landing page |
| `Project Screenshots/john-doe-no-access.png` | John Doe My Apps — no BlueWave HR Portal (before assignment) |
| `Project Screenshots/john-doe-access-restored.png` | John Doe My Apps — BlueWave HR Portal visible (after assignment) |
| `Project Screenshots/sarah-lee-my-apps.png` | Sarah Lee My Apps — confirmed SSO access |

---

## Skills Demonstrated

| Skill | How It Was Applied |
|-------|--------------------|
| SAML Federation | Configured end-to-end SAML trust between Entra ID and a non-gallery enterprise app |
| Enterprise App Integration | Registered and configured the BlueWave HR Portal as an Entra ID enterprise application |
| Claims Mapping | Mapped user attributes (UPN, email, department, job title) into SAML assertion claims |
| User Assignment | Assigned specific users to the application to enforce access control |
| SSO Troubleshooting | Identified and resolved failed SSO caused by missing user assignment |
| Access Validation | Validated My Apps dashboard appearance and SSO flow for assigned users |
| Entra Enterprise Applications | Navigated and administered enterprise application settings in Azure Portal |

---

## Lessons Learned

**User assignment is the first thing to check.** The troubleshooting scenario reinforced that missing application assignment is one of the most common causes of SSO failures. The application and SAML configuration were correct — the only issue was that John Doe had not been added to the application's user list. In production, assignment errors are frequently misdiagnosed as federation problems.

**Claims mapping directly affects application behavior.** The attributes passed in the SAML assertion determine what the Service Provider knows about the user. Incorrect or missing claims (such as email or job title) can cause authorization failures even after successful authentication. Testing with a real SP like the SAML Toolkit makes this visible and verifiable.

**Non-gallery apps require manual configuration.** Unlike pre-integrated gallery applications, non-gallery apps require every SAML field to be set manually. This project reinforced a solid understanding of the SAML trust model — Entity ID, ACS URL, and signing certificates — rather than relying on auto-populated defaults.

---

## Repository Structure

```text
Project-SAML-SSO-BlueWave/
├── Project Screenshots/
│   ├── enterprise-app-overview.png
│   ├── enterprise-app-list.png
│   ├── saml-basic-config.png
│   ├── attributes-claims.png
│   ├── users-and-groups.png
│   ├── saml-toolkit-home.png
│   ├── john-doe-no-access.png
│   ├── john-doe-access-restored.png
│   └── sarah-lee-my-apps.png
└── README.md
```

---

## References

- [Microsoft Entra ID Documentation](https://learn.microsoft.com/en-us/entra/identity/)
- [Configure SAML-based SSO](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/configure-saml-single-sign-on)
- [SAML Toolkit for Azure AD](https://samltoolkit.azurewebsites.net)
- [NIST SP 800-63 Digital Identity Guidelines](https://pages.nist.gov/800-63-3/)
