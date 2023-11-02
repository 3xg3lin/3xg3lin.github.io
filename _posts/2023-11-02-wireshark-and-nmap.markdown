---
layout: post
title: "Top 24 Penetration Tools in 2023(wireshark/nmap samples)"
date: 2023-11-02 16:45:59 +0500
---  
  
| **Tool** | **Short Description** |
| :---: | :---: |
| Wireshark: | Wireshark is a network packet analyzer. |  
| Metasploit: | Essentially, metasploit helps the pentesting team to search for vulnerabilities in the network. |  
| Burp Suite: | Burp Suite is integrated platform for performing security testing of web applications. (Alternative OWASP ZAP) |  
| Nmap: | **N**etwork **Map**per is the most famous scanning tool used by penetration testers. |  
| Sqlmap: | Sqlmap automates the process of detecting and exploiting SQL injection and comes with a very powerful detection engine. |  
| Intruder: | Intruder is a cloud-based software designed to help businesses automatically perform security scans to identify and remediate potential threats. |  
| Nessus: | Nessus is a platform for scanning vulnerability in devices, applications, operating systems and cloud security. |  
| Zed Attack Proxy: | OWASP ZAP (**Z**ed **A**ttack **P**roxy) is a free, open-source web application security scanner designed to be used during the development phase. |  
| Nikto: | Nikto is a free software command-line vulnerability scanner that scans web servers for dangerous files/CGIs, outdated server software and other problems. |  
| BeEF: | The **B**rowser **E**xploitation **F**ramework It is a penetration testing tool that focuses on the web browser. |  
| Invicti: | Invicti is an automated web application security scanner that enables you to scan websites, web applications, and web services, and identify security flaws. |  
| PowerShell-Suite: | Powershell is a collection of many scripts for quick access. |  
| W3af: | **W**eb **A**pplication **A**ttack and **A**udit **F**ramework is an open-source web application security scanner. |  
| Wapiti: | Wapiti allows you to audit the security of your websites or web applications. It performs a black-box scan. |  
| Radare: | Radare is a reverse engineering toolkit used to disassemble, analyze, emulate and debug applications and perform forensics on file systems on any modern operating system. |  
| MobSF: | **Mob**ile **S**ecurity **F**ramework is an automated, all-in-one mobile application (Android/iOS/Windows) pen-testing, malware analysis and security assessment framework capable of performing static and dynamic analysis. |  
| FuzzDB: | FuzzDB is an open source database of attack patterns, predictable resource names,  regex patterns for identifying interesting server responses, and documentation resources. |  
| Aircrack-ng: | Aircrack-ng is a software suite for analyzing and hacking WiFi networks. |  
| SET: | **S**ocial **E**ngineering **T**oolkit is a powerful collection of tools designed for social engineering. Penetration testers often use it to test an organization's security by simulating social engineering attacks on employees. |  
| Hexway: | Hexway is a pentest solution that simplifies reporting and saves pentesters time for better things. |  
| Shodan: | Shodan makes it possible to detect devices that are connected to the internet at any given time, the locations of those devices and their current users. |  
| DNSdumpster: | DNSdumpster is an online passive scanning tool to obtain all kinds of DNS related information. |  
| Hunter: | Hunter ,email outreach platform, works by scraping the internet for results that contain the email you're looking for. |  
| URL Fuzzer: | The URL Fuzzer uses a custom-built wordlist for discovering hidden files and directories. |  

## What is Network Forensic?  
Network Forensic examines the network and the traffic passing through it for suspicious activity.  
📝 ***NOTE: Network Forensic not only covers the TCP/IP network protocol but also the GSM network.***  
## So how can we listen to the network?  
For this we have to use forensic tools such as Wireshark or tcpdump, known as network sniffers.  
## How wireshark works  
Wireshark captures and monitors network traffic using promiscuous mode from networks such as Ethernet, wireless, etc.  
📝 ***Note: Promiscuous mode is a mode for a wired NIC (network interface controller) that causes the controller to pass all traffic it receives to the CPU. Thus, in contrast to Monitoring mode, you can monitor all traffic without disconnecting from the network.***  
## What we can do with wireshark?  
Wireshark is a powerful tool used for various purposes. You can use it for troubleshooting web applications (to analyze HTTP traffic between client and server), security analysis (to detect suspicious activity), and even telephony traffic (to analyze VoIP calls and other telephony protocols).  

<ins>Let's do some examples with Wireshark and explain some terms...</ins>  
First, I setup _vsftpd_ server on my virtual machine.  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/3693e132-af13-4c5f-b192-e7b4e3535086)  
Everythink okey.  
💡 Tip: FTP (File Transfer Protocol) is used to communicate and transfer files between computers on a TCP/IP (Transmission Control Protocol/Internet Protocol) network, aka the internet.  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/cd9d249a-2e2e-4ec8-91e8-02eae9c3a595)  
And we can see the output of the nmap scan.  
💡 Tip: The brute force attack method uses trial and error to crack the password.  
We can use Hydra for brute force attack.  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/91bba629-c417-40ea-a754-0be7b54dfff6)  
Let's monitor this with wireshark.  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/3657efc0-6c0d-4223-ab67-e54ba6354323)  
If you want the see successfully login type following code:  
`ftp.response.code==230`  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/2c3f95dc-0ae8-4734-af56-b545e8208352)  
Or incorrect login:  
`ftp.response.code==530`  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/0ee5e4b7-6675-4c52-ae67-c685234519c4)  
You can reach the [ftp status code here](https://en.wikipedia.org/wiki/List_of_FTP_server_return_codes).  
If we want to see all commands sent from the client.  
`ftp.request.command`  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/67e679a6-69a7-4ae1-831b-3bf24867ad96)  
Maybe we want to see all of this stream. For this select the menu item **Analyze** → **Follow** → **TCP Stream**.  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/69c377c8-aef8-402e-b79f-343e0b464448)  
For see the flow graph. Select the menu item **Statistics** → **Flow Graph**.  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/0854ff35-5df8-47d5-bb95-29bf8f00b338)  
💡 Tip: IP Spoofing is the creation of IP packets with a false source IP address, for the purpose of impersonating another computing system.  
📝 ***Note: IP spoofing usually prefer with UDP protocol. Because UDP doesn't establish a connection or maintain a session between the sender and receiver like TCP does.***  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/80850b3e-e8a5-4a7b-a126-1522c6728bfa)  
We can see the nmap output, but one of these outputs is not correct. This is because FTP works with TCP and needs a connection via session. Keep in mind that!  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/b7956013-d6da-41ce-a91d-83d3bc302618)  
But we can monitor this network traffic.  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/03a66288-93d7-42da-a577-dd8105df9661)  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/1afac9c1-ac30-49e4-9f2e-83ce6bd14e81)  

Let's check how secure the vpn is...  
📝 ***Note: We use VPNs to protect against hackers during public Wi-Fi connections. The biggest problem is the possibility of data leaking out. One of this leak is WebRTC leaks,DNS leaks and IP address leaks.  
💡 Tip: WebRTC enables real-time communication for web browsers and mobile applications, such as sending audio, video or general data. So it can reveal the IP address of the end user.  
💡 Tip: DNS returns an IP address and IP addresses can be monitored if there is a DNS leak.  
After connecting to the VPN, wireshark looks like this:  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/6a27c7d8-2742-459a-95c2-0eed5af815a6)  
And then I navigate around.  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/c61b1437-c752-48bf-ba35-8d071ccdc040)  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/b77802ee-2bff-4561-a47c-1c84740e4c2c)  
So let's try to monitor these pages.  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/c43f5d29-7cd5-4ff1-add8-b554b1bb6f75)  
There's nothing!  
Let's try for DNS too...  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/743ffaf5-e33e-4c25-b88c-602672fae411)  
Still nothing.  
Finally, try to observe our public IP.  
📝 ***Note: You can see the public IP address on your terminal using the code below or type "What is my public IP address" in your browser.***  
```
$ curl ifconfig.co
$ curl ifconfig.me
$ curl icanhazip.com
```  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/fb9a9365-db80-4d29-8bac-918a55dccf18)  
And still nothing.  

Now we will use dirb. For this I first start the apache server in my virtual machine.  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/bdb46192-ef8f-4a9e-b971-753b7367071d)  
Now select the menu item **Statistics** → **HTTP** → **Packet Counter**  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/b9c2dace-4757-4c55-938d-a65c0dc85b88)  
We can see all HTTP request method is "GET".  
If we want to list all directory and folder request from the client. Select the menu item **Statistics** → **HTTP** → **Requests**  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/e0d00819-e594-4d60-a377-dc484d213f79)  
Of course we can also see the HTTP Stream. Select the menu item **Analyze** → **Follow** → **HTTP Stream**  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/1fa1cdfd-fcb5-4d7b-86a2-0a0fcf294f39)  
Now type `http.response.code==200` for listing all available pages.  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/160e3cde-ef76-4f29-8d3b-6c945dd57be9)  

Now let's try a brute force attack on the login page. But first we need to create a login page. I ask chatgpt to help me with this.  
```
<!DOCTYPE html>
<html>
<head>
    <title>Login Page</title>
</head>
<body>
    <h1>Login Page</h1>

    <form method="post" action="welcome.php">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" required><br><br>

        <label for="password">Password:</label>
        <input type="password" id="password" name="password" required><br><br>

        <input type="submit" value="Login">
        
	<p id="error-message" style="color: red;"><?php echo isset($_GET["error"]) ? $_GET["error"] : ""; ?></p>
	
    </form>
</body>
</html>
```  
💡 Tip: The file name of the code above is login.html  
```  
<!DOCTYPE html>
<html>
<head>
    <title>Welcome Page</title>
</head>
<body>
    <h1>Welcome Page</h1>

    <?php
    if ($_SERVER["REQUEST_METHOD"] == "POST") {
        $username = $_POST["username"];
        $password = $_POST["password"];

        // Check if the provided username and password are correct
        if ($username === "test" && $password === "test") {
            echo "Logged in successfully";
        } else {
		header("Location: login.html?error=Invalid%20password");
            exit;
        }
    }
    ?>
</body>
</html>
```  
💡 Tip: The file name of the above code is welcome.php  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/4787fa7e-f0f6-4ec8-a3b7-6e32f2e2b9a7)  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/e5cb4b87-6805-40c0-af2e-25c9e3c1af44)  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/7ea33895-5085-4d89-8bdb-bc18a93f1bd4)  
Everythink great!  
Now let's start a brute force attack using the Burp Suite. First, I captured the request using the Proxy tab, then I sent it to the Intruder tab.  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/711dc5bf-5d76-4e1c-b666-878ba6de812e)  
The attack started.  
And then I sort by status code to find the http response code 200.  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/714096ca-479a-40a4-9ef0-989f3d63f8ea)  
And finally, let's monitor all this network traffic with wireshark.  
![image](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/834c44bf-e328-4b8c-a688-d16fee2dbfa2)  

























