# Setting Up Autopilot  

## Introduction

### Windows Autopilot is a cloud-based deployment service designed to simplify and automate device setup and configuration. By uploading a device's hardware hash into the Autopilot enrollment section, you can enable zero-touch deployment for new devices (directly from the manufacturer) or reset and reconfigure existing managed devices efficiently.
### Autopilot can be set up to ensure managed devices, such as laptops, automatically apply security policies, device configurations, and compliance requirements.
### Autopilot is like a meal prep service for Windows devices. Instead of cooking from scratch (manually installing software and configuring settings), Autopilot takes a pre-set recipe and applies the neccessary ingredients (apps, policies, and settings) as soon as the device starts up--delivering a ready-to-use system without the need for re-imaging. 
### When I first started learning Autopilot, I struggled to find a template that clearly defined how policies should be created and what they meant. At the same time, I was studying for my Security+ certification and came across various NIST frameworks, particularly NIST SP 800-53 Rev. 5. This framework outlines security and privacy controls for federal systems and organizations. Inspired by its structured approach, I created an Autopilot template aligned with NIST 800-53. This template was implemented across multiple tenants in our business, establishing a strong security baseline for all our clients.


---
#### Create a Group

***Step 1*** 

Go to Azure Administration -> Manage -> Groups -> New Groups  

***Step 2***  

Create a Security Group and name it "Windows 11 Autopilot." The membership type should be set as "Dynamic Device"  

---

![getcontent](https://github.com/GSecAwareness/Autopilot/blob/main/Dynamic%20Device.PNG)
---

Every device in this group will be eligible for Autopilot.

***Step 3***  

Under device membership rules, add the following rules:  

**DeviceOSType** = Windows

**Device OSVersion** Starts with 10.0.2 (this is the code for Windows 11)

![getcontent](https://github.com/GSecAwareness/Autopilot/blob/main/Configure%20Rules.PNG)
---
Create and Save


***Step 4 Optional***

Create a company branding for the sign in process. There are various options like personalized messages and company logos. Once you are satisfied, save the configuration  

***Step 5***  

You can retrieve device hardware hashes using a PowerShell script. I opted for a script that saves the hashes to a .csv file and stores it in a folder (C:/HWID) at the root directory. Alternatively, you can use a script that uploads the hashes directly to your Intune account.  

I found the direct upload method to be time-consuming and tedious, so saving the hash locally streamlined the process. Additionally, signing into a global administrator account on a user's device is not a best practice for system administration and should be avoided.  

![getcontent](https://github.com/GSecAwareness/Autopilot/blob/main/powershell%20script.png)
---

***Step 6***

Choose Import and follow the instructions to import from the CSV file. This could take up to 5 minutes for it to show up in the device list.  

***Step 7 - Deployment Profiles***  

Under Windows Autopilot, click on Deploymenet Profiles  

Create New -> Windows PC -> Name -> Leave the rest as defaults and click Next.  

On the Out-Of-Box screen, we have many options. I leave the first option as User-Driven for the Deployment mode. This ensures that the user enters their credentials. I don't change many other options.  

Apply device name template: "Yes"  

I chose to name each device Name%RAND:3% where RAND:3 indicates 3 random numbers.  

![getcontent](https://github.com/GSecAwareness/Autopilot/blob/main/RAND.PNG)
---
On the next screen, add the group that we created earlier. Click Next and Review and Create!  

![getcontent](https://github.com/GSecAwareness/Autopilot/blob/main/Group.PNG)
---

***Step 8 - Creating and Configuring Apps***

The next step is to assign apps to the Autopilot deployment. This ensures that Office productivity apps are installed alongside Windows during the initial setup.  

Intune Admin Center --> Apps --> Windows --> Create  
Under App Type, choose Microsoft 365 Apps for Windows 10 and Later  
This will bring up a new page for configuring the 365 Apps. There isn't much to change on the first page. Toggle "Show this as a featured app in the Company Portal" if you please. Click Next.  

***Step 9***  

Under Configure app sute, select the specific apps you want and the Update Channel settings.  
"Current Channel (Preview)" is an update channel that provides early access to the latest features before they're released to the broader audience in the Current Channel. This allows organizations to test and evaluate new functionalities in a controlled environment before wider deployment.  
"Current Channel" is the default update channel for most Microsoft 365 users. It provides the latest features, security updates, and bug fixes as soon as they are ready, typically releasing updates at least once a month.  

![get-content](https://github.com/GSecAwareness/Autopilot/blob/main/1.PNG)  
---

***Step 10***

The next page is dedicated to assignments and exclusions. Choose your users or groups accordingly. Review and Create.  

![get-content](https://github.com/GSecAwareness/Autopilot/blob/main/2.PNG)   
---

***Step 11 - Create a Firefox Win32 App***

Lets add a few other apps to our deployment. We need some management apps as well as an additional browser.  
In the same Apps section, choose Create, Windows App(Win32), and Select  

![get-content](https://github.com/GSecAwareness/Autopilot/blob/main/3.PNG)  
---

I want to use the standalone enterprise version of Firefox instead of the UWP version from the Microsoft Store because it provides better control for our deployment.  

The enterprise version allows Mozilla to manage updates directly, without delays from the Microsoft Store. Additionally, administrators can enforce policies via Intune and customize settings. These are capabilities that the UWP version lacks due to its sandboxed nature and limited policy support.

Search for "firefox standalone enterprise download." Go to the provided page and download the correct version in MSI format. 
The MSI needs to be converted into an .intunewin file using the Win32 Content Prep Tool from Micsoft's Github page: [Here](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool)  
Extract the Prep Tool and run the executable, which will open a command prompt. 

![get-content](https://github.com/GSecAwareness/Autopilot/blob/main/4.PNG)
---

***Step 12***

The command prompt will ask for the Source and Desintation folder, as well as the file to be converted. To make it easier, I renamed the file "FF.msi." After some time, the conversion will complete and you can continue with the app deployment. Be mindful that the conversion can take a long time due to encryption.

Once completed, choose the newly converted file and continue the process. Under App information, add the Publisher name and any other information you want to show.

Under Detection rules, you will need to choose a Rules format. I chose "Manually configure detection rules" and MSI" as the type.  This ensures that:  
1) the app installed correctly
2) prevents reinstallation
3) works with Firefox MSI auto updates
---

![get-content](https://github.com/GSecAwareness/Autopilot/blob/main/5.PNG)
---

The next section is about Dependencies. In this specific case, we do not have dependencies for the installation of Firefox. 

The section after that is Supersedence. This replaces an old app with a newer version. This doesn't apply because Firefox will update itself. 

Under Assignments, choose your User or Group and click Next to continue. It may take a moment to create the app because Intune has to upload the intunewin file. 

Once the apps are deployed, we need to concentrate on creating the Intune policies, which will align with NIST (SP) 800-53, to our Autopilot group. We will continue this in the next section.

 























  

