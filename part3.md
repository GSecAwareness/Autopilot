# Intune Policy Configurations - NIST 800-53 Compliance

Now that we have our template in place, it's time to set up the configurations that align with our security goals. To do this, we’ll use Microsoft Intune, a cloud-based endpoint management solution that helps organizations enforce security settings and maintain compliance across devices. Intune allows administrators to apply policies for encryption, firewalls, malware protection, and access controls using configuration profiles, compliance policies, and conditional access. It also integrates with Entra ID to ensure only trusted, managed devices can access company resources. If a device doesn’t meet compliance standards, it can be blocked, quarantined, or automatically remediated. Once our configurations are created, assign them to the Autopilot group we created in the first section.


### **AC-2 Account Management**
- ****Require Entra ID joined device OR MFA for all users.**** This protects access to company resources by requiring users to use a managed device or perform MFA.  

Devices > Conditional Access > New Policy > Name, Users (All), Resources (All), Grant (Require MFA, Require Entra hybrid joined device), For Multiple Controls (Require one of the selected controls)

### **AC-2(5) Access Enforcement**  

- ****Disable Built-in Admin Accounts.**** I could not find an easy to do this. You can enable LAPS, but I strictly wanted the built-in admin account disabled. The easiest way to do this in Intune was to create a PowerShell script and enforce it on all devices. Be mindful that the script will be run only once, unless it is scheduled.
-    
    ****Create a PowerShell Script in Administrator: Windows PowerShell ISE****  
          *Get-Localuser –Name “Administrator” | Disable-Localuser*
    ****Save DisableLocalAdmin.ps1 in Documents****  

Devices > Scripts and Remediation > Platform Scripts > Add > Add DisableLocalAdmin.ps1 

### **AC-3 Least Privilege**  

- ****Location-based access (US only)****

Devices > Conditional Access > Manage > Named Locations > Add Countries Location > Name (United States only), Select “United States” > Create 
Devices > Conditional Access > New Policy > Name, Users (All), Resources (All), Network (Include selected Networks and locations > Select > United States only) 

### **CM-6: Configuration Management** 

- To strengthen system security, it's important to implement the ****Windows Security Baseline.**** This set of security settings, recommended by Microsoft, helps improve Windows security by reducing vulnerabilities and boosting overall protection. It provides a straightforward way to follow best practices, without the need for manual setup, and includes a variety of Intune policy options for enforcing key security measures.

Intune > Endpoint Security > Security Baselines > Security Baseline for Windows 10 and later > Create Profile > Name, Description > Configurations Settings (review and adjust settings) > Add Assignments (Users, Groups) and Exclusions > Create  

### **CM-7: Least Functionality**  

- Ensures managed devices can only ****install Intune-deployed applications,**** reducing the attack surface.

Intune Admin > Endpoint Security > App Control for Business (Preview)   

**Managed Installer** > Add > run through the instructions to designate Intune as the managed application installer  

**App Control for Business** > Create Policy > Name (App Control) > Configuration Settings format (use built-in controls) > Enable App Control (Enforce) > Select additional rules for trusting apps (trust apps from managed installers) > Next > Assign > Create  

### **CM-8 System Componenet Inventory**

- Ensure an up-to-date inventory through ****automatic removal of decommissioned or inactive devices****

Intune Admin > Devices > Organize Devices > Device clean-up rules > Delete devices based on last check-in date (yes) > Delete devices that haven’t checked in for this many days (30)  

### **IA-2 Identification and Authentication**  

- ****Require MFA with Authenticator Only.**** This ensures that the only way to authenticate with MFA is through an Authenticator, as opposed to less secure methods like SMS or Email=based authentication.

Entra Admin Center > Security > Manage > Authentication Methods  
Under the Authentication Method Policies, enable only the Authenticator (for all users)

### **IA-4 Identifier Management**

- Allow ****Self-Service Password Reset (SSPR).**** This allows users to reset their own passwords without contacting the IT department for help. This is an organizational policy and is very easy to implement for all users. Follow the instructions to choose the correct authentication method. Finally, it enables users to register their own authentication method. Run through the rest of the options on this blade to enable some necessary protection.

Entra Admin > Protection > Password Reset > Properties > Toggle “All” for Self service password reset enabled  

Entra Admin > Protection > Password Reset > Authentication Methods > Checkmark “Mobile App Notification” and “Mobile App Code”  

Entra Admin > Protection > Password Reset > Registration > Toggle “Require users to register when signing in”  

### **IA-5 Authenticator Management**

- ****Password Complexity**** uses Microsoft default standards (8 characters, must include 3 of the following: upper, lower, numbers, and special characters). However, we can enforce stricter password protection by banning common passwords and enforcing lockout settings.  

Entra Admin > Protection > Authentication Methods > “Lockout Threshold: 3” This will allow 3 failed login attempts before its first lockout  

Entra Admin > Protection > Authentication Methods > Toggle “Enforce Custom List” and enter common passwords for banning, one per line. Toggle “Enforced”  

### **IR-4 Incident Handling**

- Enable ****Defender for Endpoints**** for threat detection and remediation.

Security Admin Center > Settings > Endpoints > General > Advanced Features > Toggle Microsoft Intune Connection  

To verify:  

- Go to Intune Admin > Endpoint Security.  
- Under the “Overview” tab (the default view), check the connector status to see if your devices are listed.   
- If the devices are not listed, sync them. This process may take up to 24 hours.   
- If the devices do not appear, you have two options:   
   - Manually onboarding using a script or creating a policy to do this.  
   - Create a policy to onboard the devices.
 
**Manual Onboard with Script**  

Security Admin Center > Settings > Endpoints > General > Device Management > Onboarding > Download Onboarding Package. Copy and paste the script into PowerShell to manually onboard the package.  

**Policy Configuration**  

Intune Admin Center > Endpoint detection and response  

- Under policies, open "Default EDR policy for all devices"
- Review the policy to ensure that it is enabled and set for all devices

### **SC-7 Boundary Protection**  

- Require ****Firewall to be enabled**** This is part of the Windows Security Baseline. Visit Endpoint Security and open Windows Security Baseline. Choose the Configuration settings and scroll down to the firewall to view this policy. Make sure Windows Security Baseline is assigned to “All Users.”  

### **SC-12 Protection of Data at Rest**  

- ****Enable BitLocker automatically for Operating System Drives.**** This enables encryption at the drive level and is necessary for a good security posture.

Intune Admin Center > Endpoint Security > Disk Encryption > Create Policy 

- Run through the Profile Configuration to complete the setup. There are multiple options that will tailor specifically to your organization. 

### **SC-28 Protection of data at rest**

- ****Saves Files to One Drive by default and Known Folder Move.**** This policy will sign users into One Drive automatically and will move their main folders (Desktop, Documents, Pictures) to One Drive.

Intune Admin Center > Devices > Configuration > Create > Win 10 and Later > Settings  

- Name the policy "Enforce One Drive KFM"
- Under Configurations, choose One Drive
- Choose "Silently move Windows known folder to One Drive," "Silently sign in users to the One Drive sync app with their Windows credentials," and "Use One Drive Files on Demand"

### **SC-28(1) Protection of data at rest

- Disable **Password Manageer in Edge.** This policy is enabled through the Windows Security Baseline.

---

There are other methods of achieving security besides policies.   

### **AC-6 Least Privilege**

- ****Limit access based on roles.**** Role-based access can be used to assign users specific privleges, such as a type of administrator. In this scenario, the user will be assigned a HelpDesk Administrator role.   

Users > Name of User > Assigned Roles > Add Assignments > Select Role  

### **AU-6 Audit Logs**  

- Ensure **Audit Logging and Sign-In Logging is enabled** collection. This is to confirm and clarify the location of the audit and sign-in logs within Intune, which are enabled by default.

Entra > Monitoring and Health > Sign-in Logs or Audit Logs  






     








