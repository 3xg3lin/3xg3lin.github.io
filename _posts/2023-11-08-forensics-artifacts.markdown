---
layout: post
title: "What is Forensics Artifacts?"
date: 2023-11-08 16:45:59 +0600
---  

## What is DFIR?  
DFIR stands for Digital Forensics and Incident Response. This area covers the collection of forensic artifacts and helps Security Experts identify footprints left by an attacker.  

## What is Artifacts?  
Artifacts are pieces of evidence that point to an activity performed on a system. We actually collect artifacts to support a hypothesis or claim about the attacker's activity. For example, at a crime scene, fingerprints, a broken button on a jacket are artifacts. In a Windows system, a person's actions can be traced quite accurately using forensics because of the various artifacts that a Windows system creates for a particular activity.  

## Is my computer spying on me?  
A Windows system keeps this track to improve the user's experience. At least that's what they say.  

## What is Windows Registry?  
The Windows Registry is a collection of databases that store low-level settings for Windows to use. And The Windows registry consists of Keys and Values. 
-  **Registry Keys** are the folders you see in the regedit.exe utility.  
-  **Registry Values** are the data stored in these Registry Keys.  

## Windows hive? What is that?  
A **Registry Hive** is a group of Keys, subkeys and values stored in a single file on disk.  

## Structure of the Registry:
- `HKEY_CURRENT_USER`
- `HKEY_USERS`
- `HKEY_LOCAL_MACHINE`
- `HKEY_CLASSES_ROOT`
- `HKEY_CURRENT_CONFIG`  

If you want to know what all this section above means, you can check this [site](https://learn.microsoft.com/en-US/troubleshoot/windows-server/performance/windows-registry-advanced-users).
Up to this point, if you are in the live system, you can do all of this. But what if you are not?  

 📝 **Note:In that case we assume you have a disk image for sure**  

The majority of these hives are located in the `C:\Windows\system32\config` directory and are:
- DEFAULT (mounted on `HKEY_USERS\DEFAULT`) 
- SAM (mounted on `HKEY_LOCAL_MACHINE\SAM`)
- SECURITY (mounted on `HKEY_LOCAL_MACHINE\Security`)
- SOFTWARE (mounted on `HKEY_LOCAL_MACHINE\Software`)
- SYSTEM (mounted on `HKEY_LOCAL_MACHINE\System`)

Apart from these hives, two other hives containing user information can be found in the User profile directory.  

- NTUSER.DAT (mounted on `HKEY_CURRENT_USER` when a user logs in) hive is located in the directory `C:\Users\<username>\. 
- USRCLASS.DAT (mounted on `HKEY_CURRENT_USER\Software\CLASSES`) hive is located in the directory `C:\Users\<username>\AppData\Local\Microsoft\Windows`   

Of course, all of these hives are important, but we will examine a small part of them.  

### UserAssist  
**User Assist** registry keys keep a record of applications started by the user. Information such as the programs launched, when they were launched and how many times they were run. And the User Assist key is present in the NTUSER hive. We can find it in `NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count`.  

### ShimCache  
**ShimCache** is a mechanism used to keep track of application compatibility with the OS. It is also called Application Compatibility Cache (AppCompatCache). We can find it in `SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache`.  

### AmCache  
AmCache stores information of executed applications. These executed applications include the execution path, the time of first execution, the time of deletion and the initial installation. This hive is located in the file system at `C:\Windows\appcompat\Programs\Amcache.hve`.  

### Device identification  
The following locations keep a record of USB keys plugged into a system. These locations store the vendor ID, the product ID, the version of the USB device, and the time the devices were plugged into the system.  
- `SYSTEM\CurrentControlSet\Enum\USBSTOR`
- `SYSTEM\CurrentControlSet\Enum\USB`  

Now let's give some examples.  
💡 Tip: I used Eric Zimmerman's Registry Explorer tool for this investigation.  

First I examining `SAM` File.  
![SAM](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/903080e4-0466-4e58-a175-76350dda582c)  

Here we can see the user's relative identifier (RID), the user's login count, last login time, last failed login, last password change, password expiration, password policy and password hint, and the groups the user is part of.  

![RECENTFILES](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/9244e8d2-f014-430d-a0eb-79a984c70a42)





