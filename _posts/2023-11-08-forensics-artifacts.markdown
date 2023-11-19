---
layout: post
title: "What is Forensics Artifacts?"
date: 2023-11-08 16:45:59 +0600
---  

### What is DFIR?  
DFIR stands for Digital Forensics and Incident Response. This area covers the collection of forensic artifacts and helps Security Experts identify footprints left by an attacker.  

### What is Artifacts?  
📝 **Note:Forensic artifacts are important pieces of information about human activity.**  
In computer forensics, these artifacts are pieces of evidence that point to an activity performed on a system.  

### So, the artifacts actually is evidence relevant to the investigation?  
No, it may or may not be related to the investigation.  

### How???  
Briefly, artifacts are objects that have a forensic value. It can be any object that contains some data or evidence of something that happened. We actually collect artifacts to support a hypothesis or claim about the attacker's activity. For example, at a crime scene, fingerprints, a broken button on a jacket are artifacts. In a Windows system, a person's actions can be traced quite accurately using forensics because of the various artifacts that a Windows system creates for a particular activity.  

### Is my computer spying on me?  
A Windows system keeps this track to improve the user's experience (At least that's what they say). Forensic investigators also use this tracked data as artifacts to identify activity on a system.

### What is Windows Registry?  
The Windows Registry is a collection of databases that store low-level settings for Windows to use. And The Windows registry consists of Keys and Values. 
-  **Registry Keys** are the folders you see in the regedit.exe utility.  
-  **Registry Values** are the data stored in these Registry Keys.  

### Windows hive? What is that?  
A **Registry Hive** is a group of Keys, subkeys and values stored in a single file on disk.  

### Structure of the Registry:
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

### What is Transaction Logs and Backups?  
We can say that ***transaction logs*** are the change log of the registry hive. Windows often uses transaction logs when writing data to registry hives. This means that the transaction logs can often have the latest changes in the registry that haven't made their way to the registry hives themselves. The transaction log for each hive is stored as a .LOG file in the same directory as the hive itself (like `SAM.LOG`). If there are multiple transaction logs, their extensions can be **.LOG1**, **.LOG2**. ***Registry backups*** are backups of registry hives located in the `C:\Windows\system32\config` directory. These hives are copied to the `C:\Windows\system32\config\RegBack` directory every ten days. Registry backups can be an excellent place to look for some registry keys that may have been deleted/modified.  

📝 **Note:You can use a live system or an image of the system to perform a forensic examination.**  
💡 **Tip:If we want to copy the registry hives from %WINDIR%\System32\Config, we cannot because it is a restricted file. For this you can use KAPE,FTK imager (FTK imager will not copy the Amcache.hve file) tools. In this case, we will use [Eric Zimmerman's tools](https://ericzimmerman.github.io/#!index.md) for live analysis.**  

### OS Version  
We can use the registry key `SOFTWARE\Microsoft\Windows NT\CurrentVersion` to find the operating system version.  

### Current control set  
Control sets are configuration data used to control the startup of the system. Usually we see two Control Sets, ControlSet001 points to the Control Set the machine booted with, and ControlSet002 is the last known good configuration. Their locations will be:  
+ SYSTEM\ControlSet001
+ SYSTEM\ControlSet002

And Windows creates a temporary Control Set called CurrentControlSet (`HKLM\SYSTEM\CurrentControlSet`) when the machine is alive.  
We can find out which Control Set is used as CurrentControlSet by checking `SYSTEM\Select\Current`.  
Similarly, the `SYSTEM\Select\LastKnownGood` configuration can also be found.  

![CurrentControlSet](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/8fb250ba-7968-4b30-bac2-154921b460f1)  

### Computer Name  
We can find the Computer Name from `SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName`.  

### Time Zone Information  
We can check `SYSTEM\CurrentControlSet\Control\TimeZoneInformation` to find the Time Zone Information.  

### Network Interfaces and Past Networks  
The `SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces` registry key will give a list of network interfaces on the machine.  

💡 **Tip: The past networks connected to the machine can be found in the following location:  
```
SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged
SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Managed
```

### Autostart Programs (Autoruns)  
The following registry keys include information about programs or commands that run when a user logs on.   
```
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce
SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Explorer\Run
SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```
📝 **Note: `SYSTEM\CurrentControlSet\Services` contains information about services. For example, look at the screenshot below.**  

![Services](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/4c21ac10-117d-4bc5-800d-59dbdbaece1e)  

In this registry key, if the start key is set to `0x02`, this means that this service will start at boot.

### SAM hive  
The SAM hive contains user account information, login information, and group information. This information is mainly located in `SAM\Domains\Account\Users`.  
![SAM](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/903080e4-0466-4e58-a175-76350dda582c)  

Here we can see the user's relative identifier (RID), the user's login count, last login time, last failed login, last password change, password expiration, password policy and password hint, and the groups the user is part of.  

### Recent Files  
Windows keeps a list of recently opened files for each user. This information is stored inside `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`.  
![RECENTFILES](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/9244e8d2-f014-430d-a0eb-79a984c70a42)  


### ShellBags  
When any user opens a folder, it opens in a specific layout. Windows stores this information and can determine the Most Recently Used files and folders. We can find this information on the following locations:  
```
USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\Bags
USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\BagMRU
NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU
NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags
```  
📝 **Note: Registry Explorer doesn't give us much information about ShellBags. You can also use another tool by Eric Zimmerman called ShellBag Explorer.**  

### Open/Save and LastVisited Dialog MRUs  
When we open or save a file, a dialog box appears asking us where to save or open that file from. When we open or save a file in a specific location, Windows remembers that location. We can find these registry keys in the following location:
```
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePIDlMRU
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPidlMRU
```
### Windows Explorer Address/Search Bars  
We can determine a user's recent activity by looking at paths typed in the Windows Explorer address bar or search terms entered in the Windows search bar. For this we can use the following location:
```
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery
```

### UserAssist  
**User Assist** registry keys keep a record of applications started by the user. Information such as the programs launched, when they were launched and how many times they were run.  

📝 **Note: However, programs that were run using the command line can't be found in the User Assist keys.**  

And the User Assist key is present in the NTUSER hive. We can find it in `NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count`.  

![userassist](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/35d42a8f-3e73-46f9-b3cc-a429a90ff978)   

We mentioned UserAssist above. We can see the time when the application was started.

### ShimCache  
**ShimCache** is a mechanism used to keep track of application compatibility with the OS. It is also called Application Compatibility Cache (AppCompatCache). We can find it in `SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache`.  
ShimCache stores the filename, file size and last modified time of executables.  

📝 **Note: Registry Explorer does not parse ShimCache data in a human-readable format, so we can use another tool called AppCompatCache Parser from Eric Zimmerman.**  

### AmCache  
AmCache stores information of executing applications, similar to Shimcache. This stored data includes execution path, installation, execution and deletion times, and SHA1 hashes of the executed programs. This hive is located in the file system at `C:\Windows\appcompat\Programs\Amcache.hve`.  

### BAM/DAM  
BAM(**B**ackground **A**ctivity **M**onitor) tracks the activities of applications in the background. And DAM(**D**esktop **A**ctivity **M**oderator) is a part of Microsoft Windows that optimizes the power consumption of the device. The location of BAM and DAM is as follows:  
```
SYSTEM\CurrentControlSet\Services\bam\UserSettings\{SID}
SYSTEM\CurrentControlSet\Services\dam\UserSettings\{SID}
```  
📝 **Note: This location contains information about last run programs, their full paths, and last execution time.**  

### Device identification  
The following locations keep a record of USB keys plugged into a system. These locations store the vendor ID, the product ID, the version of the USB device, and the time the devices were plugged into the system.  
- `SYSTEM\CurrentControlSet\Enum\USBSTOR`
- `SYSTEM\CurrentControlSet\Enum\USB`  

![USB](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/0829e81f-7796-49ae-90b2-57aed03b597a)  

### USB device Volume Name  
The device name of the connected drive can be found at the following location:  
`SOFTWARE\Microsoft\Windows Portable Devices\Devices`  

📝 **Note: We can compare the GUID in this registry key with the Disk ID specified in the device definition to associate names with unique devices.**  

### What is NTFS file system?  
File Allocation Table (FAT) is one of the default file systems for Microsoft Operating Systems, at least until the late 1990s. However, FAT doesn't offer much for security, reliability and recovery capabilities. It also has certain limitations when it comes to file and volume sizes. Hence, Microsoft developed a newer file system called the New Technology File System (NTFS) to add these features.  

### What is these features in NTFS?  
+ Journaling: The NTFS file system keeps a log of changes to the metadata in the volume. This feature helps the system recover from a crash. This log is stored in **$LOGFILE** in the volume's root directory.
+ Access Controls: The NTFS file system has access controls that define the owner of a file/directory and permissions for each user. FAT doesn't have these access controls.
+ Volume Shadow Copy: The NTFS file system keeps track of changes made to a file using a feature called Volume Shadow Copies. Using this feature, a user can restore previous file versions for recovery or system restore.  
📝 **Note: Ransomware actors have been observed deleting shadow copies on file systems to prevent victims from recovering data.**
+ Alternate Data Streams:

