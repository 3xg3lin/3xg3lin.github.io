---
layout: post
title: "What is MITRE ATT&CK?"
date: 2023-10-19 16:45:59 +0400
---

![MITRE ATTACK](https://www.picussecurity.com/hubfs/Comp%201_2.gif)

*MITRE ATTA&CK* stands for *MITRE Adversarial Tactics,Techniques and Common Knowledge (ATT&CK)*. *MITRE ATTA&CK Framework* is knowledge base model for various malicious threats. This framework was created in 2013 by the *MITRE* corporation. Then this freamwork created based on real-world observations and it continues to evolve with the threat landscape.  

*MITRE ATTA&CK* Freamwork has tree main components:
- **Tactics**: This denotes the goals that the threat actor may want to achieve in order to attack the system.
- **Techniques**: This defines the ways that the threat actor uses to achieve tactical goals.
- The framework also contains documented details about previous usage of the techniques.

This Framework has different Matrices, and the most famous matrix is Enterprise Matrix. Enterprise Matrix describes the tactics and techniques used by threat actors against enterprise platforms.  

Some tactics mentioned under the Enterprise matrix are:
1. **Reconnaissance**: Covertly gathering information about a target or targets for planning an attack.
2. **Resource Development**: Deciding on information to carry out an attack.
3. **Initial Access**: Establishing an initial foothold over a system by gaining access.
4. **Execution**: Deployment of the tools to carry put the attack.
5. **Persistence**: maintaning presence over a network even if mitigation techniques have been employed.
6. **Privilege Escalation**: Getting much higher level controls such as administrator level or root level controls.
7. **Defense Evasion**: Attempting to bypass the security mechanisms implemented on the network for protection.
8. **Credential Access**: Gaining access to some important usernames and passwords.
9. **Discovery**: Trying to figure out the target environment.
10. **Lateral Movement**: It means going deep into the target network to get some sensitive information that could compromise the system.
11. **Collection**: Collecting of data about the target that can help to achieve a goal.
12. **Command & Control**: Once all kinds of access has been gained. Attacker uses this tactic to finally establish contol over the network or system.
13. **Exfiltration**: Stealing data from the compromised system.
14. **Impact**: Manipulation, interruption or destruction of system and the data inside.

Briefly, this framework is one such tool for individuals, organizations and governments to avoid their systems and networks becoming targets for malicious actors in cyberspace.

Let's try an Operating System Credential Dumping section in the *MITRE ATTA&CK Matrix*.

![Screenshot_2023-10-20_00-13-28](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/176d7f6d-d567-4514-9854-74437b2dc36c)

Now let's go to the Windows machine and set up the requirements to get ready to receive some credentials.

First of all, you should install one of the mimikatz, bloodhound programs.  
Or you can do it manually if you have gained a shell in the system.(click to reach [cheatsheet](https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993))  
I chose mimikatz (it's optional)  
Then I ran mimikatz.  
After then I check the mimikatz is running an administrator with the following code:  
```
privilege::debug
```
Now I need the dump hash with mimikatz:  
```
lsadump::lsa /inject
lsadump::lsa /patch
```
you can use one of the above code.(first one is suit for me)  

![mimikatz](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/47d28277-7210-48f7-94d2-d353d6a79457)

Now you see the hash.  
Notice that the Windows default user ID starts at 1000, and 500 for Administrator.  
After then I send the hash my pc using netcat.  

![netcat](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/697925ae-f5fe-46fc-ad06-1cd06ae78cf9)

And try to crack hash with hashcat but first i need to create a password list.  
To do that I write this simple code:  
```
#include<stdio.h>

int main(){
    for (int i=0;i<124;i++){
	    printf("%d\n",i);
    }
}
```
If you want to do that with crunch.  
```
crunch 3 3 0123456789 > password.txt
```
Now crack time...  

![2023-10-20_00-07](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/9f5c9cce-3728-4a70-9c9d-2de930cbe2ed)

![2023-10-20_00-08](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/8781800b-caf1-42d9-93e0-44af40555248)


You can see the password next to the hash in the picture above.  

Let's detail this with the Mitre attack framework...

![mitre1](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/add6a6e2-40f0-4451-9bea-c8aeb41ef86e)

- You should see some ID starts with T1* ,this means **T**echnique.  
- If you visit the tactic section named [Credential Access](https://attack.mitre.org/tactics/TA0006/), you should see ID like TA00*(This means **TA**ctic).  

There is another term is TTP, what is it?  
TTP mean **T**actic,**T**echnique and **P**rocedures. TTP helps security teams to detect and mitigate attacks by understanding the way threat actors operate.  

If you scroll down you will see Procedure Examples. This section describes some groups like APT that have attack methods and tactics.  

![mitre2](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/aeee0387-ce72-4579-8bf5-578dac52fc6f)

And if you little bit down you will see mitigation and detections section for Blue Team.  

![mitre3](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/4845b11c-b53c-4d10-9c90-3f38c84f6b32)

![mitre4](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/b906165e-14d2-4d43-8289-6709b6d01f94)

Group section has very important knowledge for TTP analysis like explore hacker group and the way they use to hack.  

![mitre5](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/419d81ac-9a03-4016-a167-f59a687b7c2c)

If you check Software section, you can see a tools used by hacker groups.

![mitre6](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/030cb5eb-dd3a-46c2-a4bb-860bde2232de)
