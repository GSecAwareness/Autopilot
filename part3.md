# Intune Policy Configurations - NIST 800-53 Compliance

Now that we have our template in place, it's time to set up the configurations that align with our security goals. To do this, we’ll use Microsoft Intune, a cloud-based endpoint management solution that helps organizations enforce security settings and maintain compliance across devices. Intune allows administrators to apply policies for encryption, firewalls, malware protection, and access controls using configuration profiles, compliance policies, and conditional access. It also integrates with Entra ID to ensure only trusted, managed devices can access company resources. If a device doesn’t meet compliance standards, it can be blocked, quarantined, or automatically remediated. Once our configurations are ready, we’ll assign them to the group we created in the first section


### **AC-2 Account Management**
- ****Require Entra ID joined device OR MFA for all users. This protects access to company resources by requiring users to use a managed device or perform MFA.****   

Devices > Conditional Access > New Policy > Name, Users (All), Resources (All), Grant (Require MFA, Require Entra hybrid joined device), For Multiple Controls (Require one of the selected controls)

### **AC-2(5) Access Enforcement**  

- ****Disable Built-in Admin Accounts. I could not find an easy to do this. You can enable LAPS, but I strictly wanted the built-in admin account disabled. The easiest way to do this in Intune was to create a PowerShell script and enforce it on all devices. Be mindful that the script will be run only once, unless it is scheduled.****
   
    ****Create a PowerShell Script in Administrator: Windows PowerShell ISE****  
          *Get-Localuser –Name “Administrator” | Disable-Localuser*
    ****Save DisableLocalAdmin.ps1 in Documents****  

Devices > Scripts and Remediation > Platform Scripts > Add > Add DisableLocalAdmin.ps1 

### **AC-3 Least Privilege**  

- ****Location-based access (US only)****

Devices > Conditional Access > Manage > Named Locations > Add Countries Location > Name (United States only), Select “United States” > Create 
Devices > Conditional Access > New Policy > Name, Users (All), Resources (All), Network (Include selected Networks and locations > Select > United States only) 

