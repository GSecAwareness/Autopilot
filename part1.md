# Setting Up Autopilot  

## Introduction

### Windows Autopilot is a cloud-based deployment service designed to simplify and automate device setup and configuration. By uploading a device's hardware hash into the Autopilot enrollment section, you can enable zero-touch deployment for new devices (directly from the manufacturer) or reset and reconfigure existing managed devices efficiently.
### Autopilot can be set up to ensure managed devices, such as laptops, automatically apply security policies, device configurations, and compliance requirements.
### Autopilot is like a meal prep service for Windows devices. Instead of cooking from scratch (manually installing software and configuring settings), Autopilot takes a pre-set recipe and applies the neccessary ingredients (apps, policies, and settings) as soon as the device starts up--delivering a ready-to-use system without the need for re-imaging. 
### When I first started learning Autopilot, I struggled to find a template that clearly defined how policies should be created and what they meant. At the same time, I was studying for my Security+ certification and came across various NIST frameworks, particularly NIST SP 800-53 Rev. 5. This framework outlines security and privacy controls for federal systems and organizations. Inspired by its structured approach, I created an Autopilot template aligned with NIST 800-53. This template was implemented across multiple tenants in our business, establishing a strong security baseline for all our clients.


---
#### Create a Group

***Step 1*** 

*Go to Azure Administration -> Manage -> Groups -> New Groups*

***Step 2***  

*Create a Security Group and name it "Windows 11 Autopilot." The membership type should be set as "Dynamic Device"*

---

![getcontent](https://github.com/GSecAwareness/Autopilot/blob/main/Dynamic%20Device.PNG)
---

Every device in this group will be eligible for Autopilot.

***Step 3***  

*Under device membership rules, add the following rules:*

**DeviceOSType** = Windows

**Device OSVersion** Starts with 10.0.2 (this is the code for Windows 11)

![getcontent](https://github.com/GSecAwareness/Autopilot/blob/main/Configure%20Rules.PNG)
---
Create and Save


***Step 4 Optional***

*Create a company branding for the sign in process. There are various options like personalized messages and company logos. Once you are satisfied, save the configuration*

***Step 5***  

*You can retrieve device hardware hashes using a PowerShell script. I opted for a script that saves the hashes to a .csv file and stores it in a folder (C:/HWID) at the root directory. Alternatively, you can use a script that uploads the hashes directly to your Intune account.*

*I found the direct upload method to be time-consuming and tedious, so saving the hash locally streamlined the process. Additionally, signing into a global administrator account on a user's device is not a best practice for system administration and should be avoided.*

![getcontent](https://github.com/GSecAwareness/Autopilot/blob/main/powershell%20script.png)
---

***Step 6***

*Choose Import and follow the instructions to import from the CSV file. This could take up to 5 minutes for it to show up in the device list.*

***Step 7***  

*Under Windows Autopilot, click on Deploymenet Profiles*  

*Create New -> Windows PC -> Name -> Leave the rest as defaults and click Next.*  

*On the Out-Of-Box screen, we have many options. I leave the first option as User-Driven for the Deployment mode. This ensures that the user enters their credentials. I don't change many other options.*  

*Apply device name template: "Yes"*  

*I chose to name each device Name%RAND:3% where RAND:3 indicates 3 random numbers.*  

![getcontent](https://github.com/GSecAwareness/Autopilot/blob/main/RAND.PNG)
---
*On the next screen, add the group that we created earlier.*

![getcontent](https://github.com/GSecAwareness/Autopilot/blob/main/Group.PNG)
---

to be continued...











  

