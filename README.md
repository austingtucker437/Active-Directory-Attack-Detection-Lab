# Active Directory Attack & Detection Lab
**SOC Analyst / Blue Team Portfolio Project**

---

## Project Overview

This project simulates real-world identity-based attacks in a Windows Active Directory environment and demonstrates how a SOC analyst detects, investigates, and responds using native Windows logging and Group Policy controls.

The lab follows a complete attack → detection → response → recovery lifecycle, focusing on authentication abuse, account lockouts, and privileged account remediation.

All work was performed in a local virtualized lab environment using enterprise-style configurations.

---

## Lab Architecture

### Environment Components

- **Domain Controller**
  - Windows Server 2022
  - Domain: `lab.local`
  - Hostname: `DC-01`

- **Client Machine**
  - Windows 11 Pro
  - Domain-joined workstation
  - Hostname: `WIN11-CLIENT`

- **Virtualization Platform**
  - UTM (Intel-based macOS)

---

## Tools & Technologies Used

- Windows Server 2022  
- Windows 11 Pro  
- Active Directory Domain Services (AD DS)  
- Group Policy Management  
- Event Viewer (Windows Security Logs)  
- RSOP (Resultant Set of Policy)  
- UTM Virtualization  
- GitHub (Documentation & Portfolio)  

---

## Skills Demonstrated

- Active Directory deployment and administration  
- Organizational Unit (OU) design  
- User and group management  
- Least-privilege delegation  
- Group Policy enforcement  
- Windows authentication logging  
- Account lockout attack simulation  
- SOC-style log correlation  
- Incident response and remediation  
- Technical documentation  

---

## Phase Breakdown

---

## Phase 1 — Environment Setup

**Objective**  
Deploy a functional Active Directory lab environment.

**Actions Performed**
- Installed Windows Server 2022 and Windows 11 Pro
- Resolved UEFI boot and installation issues in UTM
- Allocated appropriate CPU, RAM, and disk resources
- Successfully joined the Windows 11 client to the domain

![Server First Boot](./screenshots/Windows_Server_2022_Server_Manager_First_Boot.png)  
![Client Domain Login](./screenshots/WIN11-CLIENT_Domain_Login_LAB.local.png)

---

## Phase 2 — Active Directory Design

**Objective**  
Create a structured, enterprise-style Active Directory layout.

**Actions Performed**
- Created Organizational Units (OUs) for departments
- Created domain users and security groups
- Applied role-based access separation

![Base OU Structure](./screenshots/Windows_Server_2022_Base_OU_Structure.png)  
![Departmental OUs](./screenshots/Windows_Server_2022_Departmental_User_OUs.png)  
![Security Groups](./screenshots/Windows_Server_2022_Security_Groups_Creation.png)

---

## Phase 3 — Delegation & Access Control

**Objective**  
Demonstrate least-privilege delegation for helpdesk operations.

**Actions Performed**
- Delegated password reset permissions
- Verified delegated access worked as intended
- Prevented unauthorized administrative actions

![Delegation Wizard](./screenshots/PHASE3B_delegate_control_wizard.png)  
![Reset Permission Delegation](./screenshots/PHASE3B_Delegation_Reset_User_Passwords.png)  
![Password Reset Success](./screenshots/PHASE3C_helpdesk_password_reset_success.png)

---

## Phase 4 — Attack Simulation & Detection

---

### Phase 4A — Authentication Logging

**Objective**  
Validate baseline authentication logging.

**Events Observed**
- Event ID 4624 — Successful logon
- Event ID 4625 — Failed logon

![Successful Logon](./screenshots/PHASE4B_4624_Successful_Logon.png)  
![Failed Logon](./screenshots/PHASE4B_4625_Failed_Logon.png)

---

### Phase 4B — Account Lockout Detection

**Attack Simulated**
- Repeated failed logon attempts against `hr.user`

**Detection & Response**
- Event ID 4625 — Failed logons
- Event ID 4740 — Account lockout
- RSOP verified policy enforcement
- Account unlocked in Active Directory

![Account Lockout Policy](./screenshots/PHASE4B_Account_Lockout_Policy.png)  
![Account Unlock](./screenshots/PHASE4B_Account_Unlock_DC.png)

**Summary**  
A domain-wide account lockout policy was enforced via Default Domain Policy, triggered by repeated failed authentication attempts, detected in Windows Security logs, and remediated by unlocking the affected account in Active Directory.

---

### Phase 4C — Password Reset Detection

**Attack Simulated**
- Administrative password reset on `hr.user`

**Detection**
- Event ID 4724 — Password reset detected on the domain controller


**Summary**  
An administrative password reset was successfully detected via Windows Security Event ID 4724, demonstrating visibility into privileged account management activity.

---

## Phase 5 — SOC Correlation & Recovery

---

### Phase 5A — Event Correlation

Multiple authentication and account management events were correlated to reconstruct the full attack timeline.

| Event ID | Description |
|--------|-------------|
| 4625 | Failed logon attempts |
| 4740 | Account lockout |
| 4724 | Password reset |
| 4624 | Successful logon |

![Event Correlation Overview](./screenshots/PHASE5A_Event_Correlation_Overview.png)

---

### Phase 5B — Recovery Verification

**Objective**  
Confirm normal user access after remediation.

![HR User Login Success](./screenshots/PHASE5B_HR_User_Successful_Login.png)

---

## Challenges & Troubleshooting Encountered

During this project, several real-world challenges were encountered and resolved, including:

- UEFI boot issues and Windows installation loops in UTM
- Incorrect VM resource allocation requiring rebuilds
- Domain join failures caused by DNS misconfiguration
- Initial confusion between local vs domain authentication formats
- Group Policy propagation delays requiring manual updates
- Event visibility issues until advanced auditing was correctly enabled

Resolving these issues strengthened troubleshooting, system administration, and incident-response skills.

---

## Conclusion

This project demonstrates end-to-end SOC Analyst and Blue Team capabilities, including identity attack detection, log analysis, Active Directory security controls, incident response, remediation, and recovery verification.

The lab mirrors real enterprise workflows and serves as a strong portfolio project for SOC Analyst roles.

---

## Repository Notes

- Screenshots are stored in the `/screenshots` directory
- Filenames are intentionally descriptive for audit clarity
- All activity was performed in a controlled lab environment
