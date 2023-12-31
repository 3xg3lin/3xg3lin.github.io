---
layout: post
title: "Wazuh installation on linux via QEMU/KVM"
date: 2023-10-11 16:45:59 +0300
---

Firstly, I installed QEMU/KVM on my machine. Installation process is going to be different from distro to distro. So, you should follow the instructions for your linux with quick search on the enthernet. After that I write the following code:
```
# virt-install \
    --name Ubuntu \
    --memory 4096 \
    --vcpus 3 \
    --disk size=50 \
    --cdrom /home/ghost/Downloads/ubuntu-22.04.3-desktop-amd64.iso \
    --os-variant ubuntu22.04 \
    --network default
```
We should do a similar instruction for Windows:
```
# virt-install \
    --name Windows-10 
    --memory 4096 \
    --vcpus 3 \
    --disk size=50 \
    --cdrom /home/ghost/Downloads/Win10_22H2_Turkish_x64v1.iso \
    --os-variant win10 \
    --network default
```
If you want the user manuel for this you can check the [rhel website](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/virtualization_deployment_and_administration_guide/sect-guest_virtual_machine_installation_overview-creating_guests_with_virt_install).  
Then, we continue the wazuh setup...  
Actually, [Wazuh Installation Guide](https://documentation.wazuh.com/current/installation-guide/index.html) is easy to use so I followed that installation manuel.  
```
# curl -sO https://packages.wazuh.com/4.5/wazuh-install.sh
# curl -sO https://packages.wazuh.com/4.5/config.yml
```
I installed this two files on my machine. After that I edited the `.config.yml`:
```
nodes:
  # Wazuh indexer nodes
  indexer:
    - name: node-1
      ip: 127.0.0.1
    #- name: node-2
    #  ip: <indexer-node-ip>
    #- name: node-3
    #  ip: <indexer-node-ip>

  # Wazuh server nodes
  # If there is more than one Wazuh server
  # node, each one must have a node_type
  server:
    - name: wazuh-1
      ip: 127.0.0.1
    #  node_type: master
    #- name: wazuh-2
    #  ip: <wazuh-manager-ip>
    #  node_type: worker
    #- name: wazuh-3
    #  ip: <wazuh-manager-ip>
    #  node_type: worker

  # Wazuh dashboard nodes
  dashboard:
    - name: dashboard
      ip: 127.0.0.1
```
And after that I run the following command:
```
# bash wazuh-install.sh --generate-config-files
```
And I do this with `--wazuh-indexer` option:
```
# bash wazuh-install.sh --wazuh-indexer node-1
```
Then I set `--start-cluster`:
```
# bash wazuh-install.sh --start-cluster
```
for learn to admin password I typed the following:
```
# tar -axf wazuh-install-files.tar wazuh-install-files/wazuh-passwords.txt -O | grep -P "\'admin\'" -A 1
```
After that I replaced with my `<ADMIN_PASSWORD>` with the password gotten from the previous command output:
```
# curl -k -u admin:<ADMIN_PASSWORD> https://127.0.0.1:9200
```
And again I used this password for the following command:
```
# curl -k -u admin:<ADMIN_PASSWORD> https://127.0.0.1:9200/_cat/nodes?v
```
The Wazuh indexer is successfully installed. Now, I started the Wazuh Server installation.
```
# bash wazuh-install.sh --wazuh-server wazuh-1
```
After Wazuh Server installation, I continued with Wazuh Dashboard:
```
# bash wazuh-install.sh --wazuh-dashboard dashboard
```
After that to find out my admin password I typed this code:
```
# tar -O -xvf wazuh-install-files.tar wazuh-install-files/wazuh-passwords.txt
```
Now I navigated the `https://127.0.0.1/`  

![base](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/d8e4cfaf-c425-4db0-b5d1-68051183d551)  

When you login the wazuh you should see this page first. Then I added agent in my Windows machine. You can do this with click 'Deploy new agent' button.

![Screenshot-4](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/12c51c73-b0ea-4bc2-b5f0-f6b0c8c06266)


Now, I attack RDP brute force with hydra and crowbar. *(I'm doing all of this in my virtual network)* 

**Note: Don't forget the install crowbar with this [github link](https://github.com/galkan/crowbar) ,but if you want to use hydra you can find that on the repository.**  

So first step is finding the IP and RDP port for target machine.*(Default 3389)*  

![nmap](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/929d3e72-3093-4a13-9021-450a51d00e0a)

We saw the port and IP.  
- Maybe you want to use netdiscover or another tool. It's up to you.  

After that I started the attack.  

![2023-10-10_18-49](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/4d15f453-4764-4af4-b632-5b10f171acc0)

![2023-10-10_18-59](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/767b9011-5db8-4580-9d60-1f0b08391bd0)

Now let's go back the wazuh and inspect the attack.

![2023-10-11_13-00](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/0f356eb0-e08a-4371-89e9-b25c38fbc77c)

You can see the description above.  
It says `User account locked out` and many `logon failure`.  
So What is **rule.id** and **rule.level**?  
**rule.id** identifies the alert. It's like a identity. We can define a [brute force attack for a Windows machine](https://documentation.wazuh.com/current/proof-of-concept-guide/detect-brute-force-attack.html) with 2 different ids.  
- rule.id:(60122) indicates login failures  
- rule.id:(60204) indicates multiple login failures
- rule.id:(60106) if login is successful

Ok, so what is **rule.level**?  
[**rule.level**](https://documentation.wazuh.com/current/user-manual/ruleset/rules-classification.html) specifies the severity level of each triggered warning.  
- rule.level:(3) indicates successful login attempts
- rule.level:(5) means an error occurred by the user himself
- rule.level:(9) indicates login attempt from unknown user
- rule.level:(10) indicates multiple failed login attempts
 
We can look Windows logs too. 

![captures1](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/6ae886d8-69e9-4d10-bf4c-3e83e0cba219)

To detect RDP brute force attacks, we should check for event IDs 4624 and 4625.  
So what is 4624 and 4625?  
- 4624 is represents a successful login to an account.  
- 4625 means a failed login attempt.  

You can see the detail of this log in the picture below.

![captures2](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/f5c17b6f-3bf8-4f46-91e9-3367d39b0ee0)
  
Logon Type: It shows how to user attempted the log in. It's 5 so that means this user log in as a service.  

![captures3](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/7a8cc711-9662-4528-8e0d-04d50c6fc751)

For the above image, you can see which user is trying to log in to **WorkstationName**. And you can see in **TargetUserName** who trying to log in as. **LogonType** is 3, which is the Logon Type used for logging in over the network.  
