# Active Directory Attack & Detection Lab
**SOC Analyst / Blue Team Portfolio Project**

---

## Project Overview

This project simulates real-world identity-based attacks in a Windows Active Directory environment and demonstrates how a SOC analyst detects, investigates, and responds using native Windows logging and Group Policy controls.

The lab follows a complete **attack → detection → response → recovery** lifecycle, focusing on authentication abuse, account lockouts, and privileged account remediation.

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
- SOC-style event correlation  
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

![Windows Server First Boot](screenshots/Windows_Server_2022_Server_Manager_First_Boot.png)  
![Windows 11 Domain Login](screenshots/WIN11-CLIENT_Domain_Login_LAB.local.png)

---

## Phase 2 — Active Directory Design

**Objective**  
Create a structured, enterprise-style Active Directory layout.

**Actions Performed**
- Created Organizational Units (OUs) for departments and users
- Implemented logical separation for HR and IT
- Created domain users and security groups

![Base OU Structure](screenshots/Windows_Server_2022_Base_OU_Structure.png)  
![Departmental OUs](screenshots/Windows_Server_2022_Departmental_User_OUs.png)  
![IT User Creation](screenshots/Windows_Server_2022_IT_User_Creation.png)  
![Security Groups](screenshots/Windows_Server_2022_Security_Groups_Creation.png)

---

## Phase 3 — Delegation & Access Control

**Objective**  
Demonstrate least-privilege delegation for helpdesk operations.

**Actions Performed**
- Delegated password reset permissions to IT helpdesk group
- Verified delegated control using Active Directory tools
- Confirmed non-administrative password reset capability

![Delegation Wizard](screenshots/PHASE3B_delegate_control_wizard.png)  
![Delegated Group Selected](screenshots/PHASE3B_delegate_group_selected.png)  
![Delegated Password Reset Rights](screenshots/PHASE3B_Delegation_Reset_User_Passwords.png)  
![Helpdesk Password Reset Success](screenshots/PHASE3C_helpdesk_password_reset_success.png)

---

## Phase 4 — Attack Simulation & Detection

---

### Phase 4A — Authentication Logging

**Objective**  
Validate baseline authentication logging.

**Events Observed**
- Event ID 4624 — Successful logon
- Event ID 4625 — Failed logon

![Successful Logon](screenshots/PHASE4B_4624_Successful_Logon.png)  
![Failed Logon](screenshots/PHASE4B_4625_Failed_Logon.png)

---

### Phase 4B — Account Lockout Detection

**Attack Simulated**
- Repeated failed authentication attempts against `hr.user`

**Detection & Response**
- Event ID 4625 — Failed logon attempts
- Event ID 4740 — Account lockout
- Lockout policy enforcement verified
- Account unlocked on the domain controller

![Account Lockout Policy](screenshots/PHASE4B_Account_Lockout_Policy.png)  
![Account Unlock on DC](screenshots/PHASE4B_Account_Unlock_DC.png)

**Summary**  
A domain-wide account lockout policy was enforced via Default Domain Policy, triggered through repeated failed logon attempts, detected in Windows Security logs, and remediated by unlocking the affected account in Active Directory.

---

### Phase 4C — Password Reset Detection

**Attack Simulated**
- Administrative password reset on `hr.user`

**Detection**
- Event ID 4724 — Password reset detected on the domain controller

![Password Reset Event](screenshots/PHASE4C_Event4724_PasswordReset.png)

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

![Event Correlation Overview](screenshots/PHASE5A_Event_Correlation_Overview.png)

---

### Phase 5B — Recovery Verification

**Objective**  
Confirm normal user access after remediation.

![HR User Successful Login](screenshots/PHASE5B_HR_User_Successful_Login.png)

---

## Challenges & Troubleshooting

During this lab, several real-world issues were encountered and resolved:

- Windows Server and Windows 11 installation failures due to insufficient disk allocation in UTM
- UEFI boot issues requiring manual ISO validation and VM recreation
- Group Policy changes not applying immediately, requiring RSOP verification and forced policy updates
- Account lockout policies not triggering until confirmed on the endpoint via RSOP
- Initial login failures caused by incorrect domain username formatting (resolved by using `LAB\username`)

These challenges reflect common enterprise troubleshooting scenarios and were fully resolved through systematic investigation and validation.

---

## Conclusion

This project demonstrates hands-on SOC Analyst and Blue Team capabilities, including identity attack detection, Active Directory security controls, Windows event analysis, incident response, and recovery validation.

The lab reflects real enterprise workflows and serves as a comprehensive portfolio project for SOC Analyst roles.

---

## Repository Notes

- Screenshots are stored in the `/screenshots` directory
- Filenames are intentionally descriptive for audit clarity
- All activity was performed in a controlled lab environment
