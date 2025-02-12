# Intune Policy Configurations - NIST 800-53 Compliance

Now that we have our template in place, it's time to set up the configurations that align with our security goals. To do this, we’ll use Microsoft Intune, a cloud-based endpoint management solution that helps organizations enforce security settings and maintain compliance across devices. Intune allows administrators to apply policies for encryption, firewalls, malware protection, and access controls using configuration profiles, compliance policies, and conditional access. It also integrates with Entra ID to ensure only trusted, managed devices can access company resources. If a device doesn’t meet compliance standards, it can be blocked, quarantined, or automatically remediated. Once our configurations are ready, we’ll assign them to the group we created in the first section


### **AC-2 Account Management**
- **Disable password saving**:  
  - Device Configuration → Create New → Settings → Edge → Password Manager and Protection  
  - **Toggle off**: Enable saving passwords to the password manager  
- **Delete stored passwords automatically**:  
  - (Within the same policy): Microsoft Edge → Clear Browser Data when Edge closes  

## **Firewall Configuration**
- **Enable firewall for all profiles**:  
  - Public, Private, Domain  

## **Password Reset**
- **Enable self-service password reset**:  
  - Password Reset → Properties → Self-service password reset  
