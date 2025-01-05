# Setting Up Autopilot  

## Introduction

### Windows Autopilot is a cloud-based deployment service designed to simplify and automate device setup and configuration. By uploading a device's hardware hash into the Autopilot enrollment section, you can enable zero-touch deployment for new devices (directly from the manufacturer) or reset and reconfigure existing managed devices efficiently.
### Autopilot can be set up to ensure managed devices, such as laptops, automatically apply security policies, device configurations, and compliance requirements.
### I think of Autopilot as a recipe for setting up a Windows device. Instead of manually adding each ingredient (software, policies, settings), Autopilot applies the recipe automatically as soon as the device starts up. 

---
#### Create a Group

1. Go to Azure Administration -> Manage -> Groups -> New Groups

2. Create a Security Group and name it "WIndows 11 Autopilot." The membership type shoudld be set as "Dynamic Device"

![getcontent](https://github.com/GSecAwareness/Autopilot/blob/main/Dynamic%20Device.PNG)

Every device in this group will be eligible for Autopilot.

3. Under device membership rules, add the following rules:

DeviceOSType = Windows

Device OSVersion Starts with 10.0.2 (this is the code for Windows 11)

![getcontent](https://github.com/GSecAwareness/Autopilot/blob/main/Configure%20Rules.PNG)






  

