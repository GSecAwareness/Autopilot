# Autopilot Template - NIST 800-53 Compliance

NIST (SP) 800-53 Rev. 5 is a security and privacy framework developed by the National Institute of Standards and Technology (NIST) to protect federal information systems and organizations from security threats. It provides a structured set of controls to ensure compliance with US federal laws like Federal Information Security Modernization Act (FISMA). FISMA mandates that federal agencies and contractors implement strict security measures to protect sensitive government data from unauthorized access or mishandling. While primarily designed for government use, many businesses adopt NIST 800-53 to enhance their cybersecurity posture. I created this template to ensure that our company establishes a strong security baseline for each tenant with proper security configurations in Microsoft 365 adminsitration. 

## Security Template in accordance with NIST 800-53

| **Control ID**  | **Category**                 | **Description**                                   |
|----------------|-----------------------------|---------------------------------------------------|
| **AC-2**       | Account Management (Compliance) | Require Entra ID joined account                  |
| **AC-2(5)**    | Access Enforcement (Configuration) | Disable inactive accounts, including built-in admin accounts |
| **AC-3**       | Least Privilege (Conditional Access) | Limit access based on location (US only)         |
| **AC-6**       | Least Privilege (RBAC/Privilege Access) | Limit access based on roles by setting permissions |
| **AC-17**      | Remote Access (Conditional Access) | Remote access only from Entra ID-joined devices (trusted, managed devices) |
| **AU-6**       | Audit Logs (Endpoint Security) | Enable Windows Event Log collection              |
| **CM-6**       | Configuration Management (Security Baseline) | Require Windows Security Baseline for hardening  |
| **IA-2**       | Identification & Authentication (Compliance) | Require MFA with Authenticator only             |
| **IA-4**       | Identifier Management (Endpoint Security) | Allow Self-service Password Reset               |
| **IA-5**       | Authenticator Management (Endpoint Security) | Use Entra ID default password policy (8 characters, upper, lower, symbols, block common, lockout after failed attempts) |
| **IR-4**       | Incident Handling (Endpoint Security) | Enable Defender for Endpoint for threat detection |
| **IR-4**       | Incident Handling (Endpoint Security) | Enable auto-connect Defender for Endpoint with Intune |
| **SC-7**       | Boundary Protection (Configuration) | Require Firewall to be enabled                   |
| **SC-12**      | Protection of Data at Rest (Configuration) | Enable BitLocker automatically                  |
| **SC-28**      | Protection of Data at Rest (Configuration) | Require files to save to OneDrive folders by default |
| **SC-28(1)**   | Protection of Data at Rest (Configuration) | Prevent password storage in Edge                |

