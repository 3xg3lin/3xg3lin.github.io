---
layout: post
title: "How to analyze mail?"
date: 2023-12-13 16:45:59 +0700
---
## How does spam affect us?  
An inexperienced and unsuspecting user in your corporate environment clicking on a link or downloading and running a malicious attachment can give an attacker a foothold on the network.  
Many products help fight spam and phishing, but sometimes these spam or phishing mails can get through.  
When this happens, as a Security Analyst, you need to analyze these emails.  
## The Email Address  
**Raymond Samuel Tomlinson** implemented the first email program on the ARPANET system. He was the first to use the @ sign to distinguish the user name from the machine name.  

what makes up an email address?  
- User Mailbox (or Username)  
- @  
- Domain  

## What if Email Delivery?  
There are 3 specific protocols for outgoing and incoming email messages. They are listed below:  
- SMTP (Simple Mail Transfer Protocol) - It is utilized to handle the sending of emails.  
- POP3 (Post Office Protocol) - Is responsible transferring email between a client and a mail server.  
- IMAP (Internet Message Access Protocol) - Is responsible transferring email between a client and a mail server.  

POP3 and IMAP looks the same. But there are differences between the two.  
**What is these differences?**  
> POP3
> - Emails are downloaded and stored on a single device.  
> - Sent messages are stored on the single device from which the email was sent.  
> - Emails can only be accessed from the single device the emails were downloaded to.  

> IMAP
> - Emails are stored on the server and can be downloaded to multiple devices.  
> - Sent messages are stored on the server.  
> - Messages can be synced and accessed across multiple devices.  

![email-network-flow](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/6206f25a-a95e-4b14-90d5-591d0dea4418)  

## what makes up an email message?  
There are two parts to an email:  
-  the email header (information about the email, such as the email servers that relayed the email)
-  the email body (text and/or HTML formatted text)

📝 **Note:The syntax for email messages is known as the Internet Message Format (IMF).**

Let's start with the following email header fields:  
- From - the sender's email address
- Subject - the email's subject line
- Date - the date when the email was sent
- To - the recipient's email address


![email-headers-1b](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/267042ae-3a56-4522-a56d-c2b01bb0840d)  

Now let's look email details.  
📝 **Note: Displaying raw/full email headers in various email clients may be different. For this, visit this [link](https://mediatemple.zendesk.com/hc/en-us/articles/204644060-how-do-i-view-email-headers-for-a-message)**.  
⚠️ **Warning: Don't engage/interact with the email addresses or IP addresses.**  

![email-raw-headers-menu-2](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/87374bcb-ece4-46fe-bfb0-70c917044649)  
![email-raw-headers-2b](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/2d481b96-9f2b-4d1d-8e28-c447ac6df7b1)


From the above image, there are other email header fields of interest.   
- X-Originating-IP - The IP address of the email was sent from (this is known as an X-header)
- Smtp.mailfrom/header.from - The domain the email was sent from (these headers are within Authentication-Results)
- Reply-To - This is the email address a reply email will be sent to instead of the From email address

[Here](https://mediatemple.zendesk.com/hc/en-us/articles/204643950-understanding-an-email-header) is an example from Media Temple on how to analyze emails.  

Now it's time to analyze the body of an email.  
The email body is the part of the email which contains the text (plain or HTML formatted) the sender wants you to view.  

![290545454-28d6aaa0-dbdd-4a74-bbd3-0abc9e94b6cc](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/3d31d427-59c6-49be-b402-5d46c0013cf7)

Above image is _text-only_ email.  
![mail](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/25936cee-47cb-4934-a9de-572ccfceedc4)

Above image is HTML formatted email. And contains an image (which was blocked by the email client) and embedded hyperlinks.  
You can view an email's HTML code similar method.  
Image below is from Protonmail.  
![email-source-code-2](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/25845a94-8a67-4bca-94af-d2075840df55)  

Lastly, emails may contain attachments. You can view an email's attachment from an email's HTML format or by viewing the source code.  

![email-with-attachment](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/dd793ab5-52ff-4a98-ad33-5b94669b23bf)  
- The email body has an image.
- The email attachment is a PDF document.

Let's view this attachment within the source code.  
![email-with-attachment-2](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/0b52c899-4b1c-4f17-b2b2-af7d3417c9b6)  

we can see the headers associated with this attachment:  
- Content-Type is application/pdf.
- Content-Disposition specifies it's an attachment.
- Content-Transfer-Encoding tells us it's base64 encoded

Briefly explain the different types of malicious emails:
- Spam - unwanted junk emails sent in bulk to a large number of recipients.
- Phishing - Emails sent to the target(s) pretending to be from a trusted organization in order to persuade individuals to provide sensitive information.
- Spear phishing - takes phishing a step further by targeting a specific individual(s) or organization seeking sensitive information.
- Whaling - similar to spear phishing, but specifically targeting people in high C-Level positions (CEO, CFO, etc.) and the goal is the same.
- Smishing - takes phishing to mobile devices by targeting mobile users with specially crafted text messages.
- Vishing - is similar to smishing, but instead of using text messages for the social engineering attack, the attacks are based on voice calls.

So what is this special feature of these phishing emails?
- Sender email name/address will appear to be a trusted entity (email spoofing)
- The email subject line and/or body (text) is written with a sense of urgency or uses specific keywords such as Invoice, Suspended, etc.
- The email body (HTML) is designed to match a trusted organization (like Amazon)
- Email body (HTML) is poorly formatted or written.
- The email body uses generic content, such as Dear Sir/Madam.
- Hyperlinks (often using URL shortening services to hide their true source)
- A malicious attachment masquerading as a legitimate document.

📝 **Note: Hyperlinks and IP addresses should be 'defanged'.**  
💡 **Tip: Defanging is a way of making the URL/domain or email address unclickable to avoid accidental clicks.**  

Sometimes you may come across links that use URL shortening services.  
In this case, you can use a URL extender like the one on this [site](https://linkunshorten.com).  

Additional Resources:
- https://www.knowbe4.com/phishing
- https://www.itgovernance.co.uk/blog/5-ways-to-detect-a-phishing-email
- https://cheapsslsecurity.com/blog/10-phishing-email-examples-you-need-to-see/
- https://phishingquiz.withgoogle.com

Below is a checklist of what information an analyst should collect from the email header (aka artifacts):  
- Sender email address
- Sender IP address
- Reverse lookup of the sender IP address
- Email subject line
- Recipient email address (this information might be in the CC/BCC field)
- Reply-to email address (if any)
- Date/time

Below is a checklist of artifacts that an analyst should collect from the email body:  
- Any URL links (if an URL shortener service was used, then we'll need to obtain the real URL link)
- The name of the attachment
- The hash value of the attachment (hash type MD5 or SHA256)

After all this, let's continue with what kind of tool we should use when analyzing.  
###  Email header analysis   

**[Messageheader](https://toolbox.googleapps.com/apps/messageheader/analyzeheader)**
According to the [site](https://toolbox.googleapps.com/apps/main/), "Messageheader analyzes SMTP message headers that help identify the root cause of delivery delays. You can detect misconfigured servers and mail routing problems".  
📖 Usage: Copy and paste the entire email header and run the analysis tool.  

**[Message Header Analyzer](https://mha.azurewebsites.net/)**  
This is another tool for mail analysis.  

**[Mailheader](https://mailheader.org/)**  
You can also use this online tool.  

📝 Note: **Message Transfer Agent** ([MTA](https://csrc.nist.gov/glossary/term/mail_transfer_agent)) is software that transfers emails between sender and recipient.  

The below tools can help you analyze information about the sender's IP address or website:  

**[IPinfo](https://ipinfo.io/)**  
According to the site, "With IPinfo, you can pinpoint your users' locations, customize their experience, prevent fraud, ensure compliance and much more."  

**[URLScan](https://urlscan.io/)**  
According to the site, "urlscan.io is a free service for crawling and analyzing websites. When a URL is submitted to urlscan.io, an automated process will browse the URL like a normal user and record the activity generated by that page navigation. This includes the domains and IPs that were linked to, the resources requested from those domains (JavaScript, CSS, etc.), and additional information about the page itself. urlscan.io will take a screenshot of the page, record DOM content, JavaScript global variables, cookies created by the page, and countless other observations. If site users are targeting one of the 400+ brands tracked by urlscan.io, it will be highlighted as potentially malicious in the crawl results."  

**[Talos Reputation Center](https://talosintelligence.com/reputation)**  
According to the site, "Talos' Reputation Center provides access to comprehensive threat data and related information."  

📝 **Note: If you don't want to explicitly go to the URL, you can use tools like [URL2PNG](https://www.url2png.com/) and [Wannabrowser](https://www.wannabrowser.net/)**.  

### Email body analysis  

Malicious payload may be delivered to the recipient either as a link or an attachment.  
Links can be extracted manually, either directly from an HTML formatted email or by sifting through the raw email header.  

Below is an example of obtaining a link manually from an email by right-clicking the link and choosing Copy Link Location.  

![copy-link](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/d1164781-1768-4d9c-adcd-f2e3ec9c43e9)  

The same can be accomplished with [URL Extractor tool](https://www.convertcsv.com/url-extractor.htm).  
📖 Usage: Copy the raw header and paste it into the "Step 1: Select your input" text box.  

You can also use [CyberChef](https://gchq.github.io/CyberChef/) to extract URLs.  
![a31606afb772b8f87eebf0ff59f00fce](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/8b862e4a-fec2-4772-a95c-30495b50a5a9)  

💡 Tip: It's important to note the root domain for the extracted URLs. You will need to perform an analysis on the root domain as well.  

After extracting the URLs, the next step is to check the reputation of the URLs and root domain.  

What if the email has an attachment?  
First, you need to obtain the attachment securely. Then you can get its hash. And then you can check the reputation of the file against the hash to see if it is a known malicious document.  

**[Talos File Reputation](https://talosintelligence.com/talos_file_reputation)**  
According to the site, "The Cisco Talos Intelligence Group maintains a reputation trend over billions of files. This reputation system feeds into the AMP, FirePower, ClamAV and Open Source Snort product lines. The following tool allows you to perform mundane searches against the Talos File Reputation system. This system limits you to one search at a time and is limited to hash matching only. This search does not reflect the full capabilities of the Advanced Malware Protection (AMP) system".  

**[VirusTotal](https://www.virustotal.com/gui/)**  
According to the site, "Analyze suspicious files and URLs to detect malware types, automatically sharing them with the security community."  

### Malware Sandbox  

There are online tools and services where malicious files can be uploaded and analyzed to better understand what the malware was programmed to do. These services are known as malware sandboxes.  

**[Any.Run](https://app.any.run/)**  
According to the site, "Analyze a network, file, module and registry activity. Interact with the operating system directly from a browser. Instantly see feedback from your actions".  

**[Hybrid Analysis](https://www.hybrid-analysis.com/)**  
According to the site, "This is a free malware analysis service for the community that detects and analyzes unknown threats using a unique Hybrid Analysis technology."  

**[Joesecurity](https://www.joesecurity.org/)**  
According to the site, "Joe Sandbox empowers analysts with a wide range of product features. These include: Live Interaction, URL Analysis and AI-based Phishing Detection, Wound and Sigma rules support, MITRE ATT&CK matrix, AI-based malware detection, Mail Monitor, Threat Hunting and Intelligence, Automated User Behavior, Dynamic VBA/JS/JAR instrumentation, Execution Graphs, Localized Internet Anonymization and much more".  

💡 Tip: Another tool to help with automated phishing analysis is [PhishTool](https://www.phishtool.com/).  

### What tactics can we use as a defender to protect users from falling victim to a malicious email?  
Some examples of these tactics are listed below:  
- Email Security (SPF, DKIM, DMARC)
- SPAM Filters (flags or blocks incoming emails based on reputation)
- Email Labels (alert users that an incoming email is from an outside source)
- Email Address/Domain/URL Blocking (based on reputation or explicit denylist)
- Attachment Blocking (based on the extension of the attachment)
- Attachment Sandboxing (detonating email attachments in a sandbox environment to detect malicious activity)
- Security Awareness Training (internal phishing campaigns)

According to the MITRE ATT&CK Framework, [Phishing for Information](https://attack.mitre.org/techniques/T1598/) is defined as an attempt to disclose information by deceiving targets and includes four sub-techniques.  

![mitreattack](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/dc5121c6-fb4b-4ad1-92a2-b9d05a231b3a)  


What is the Sender Policy Framework (SPF)?  
According to [dmarcian](https://dmarcian.com/what-is-spf/), "The Sender Policy Framework (SPF) is used to authenticate the sender of an email. When an SPF record exists, Internet Service Providers can verify that a mail server is authorized to send email for a specific domain. An SPF record is a DNS TXT record that contains a list of IP addresses that are authorized to send email on behalf of your domain."

How does a basic SPF record look like?  
`v=spf1 ip4:127.0.0.1 include:_spf.google.com -all`  
An explanation for the above record:
- `v=spf1`-> This is the start of the SPF record.
- `ip4:127.0.0.1`-> This specifies which IP (in this case version IP4 & not IP6) can send mail
- `include:_spf.google.com`-> This specifies which domain can send mail
- `-all`-> non-authorized emails will be rejected

See SPF Record Syntax in dmarcian [here](https://dmarcian.com/spf-syntax-table/)  

![spf](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/0b68fb51-6717-40c8-9565-1231dd25d323)  
The image above shows the SPF record of a malicious email.  

What is DKIM?  
According to [dmarcian](https://dmarcian.com/what-is-dkim/), "DKIM stands for DomainKeys Identified Mail and is used for authentication of a sent email. Like SPF, DKIM is an open standard for email authentication used for DMARC alignment. A DKIM record exists in DNS, but it is slightly more complex than SPF. The advantage of DKIM is that it can survive redirection, which makes it superior to SPF and provides a basis for securing your email."  

How does a DKIM record look like?  
```
v=DKIM1; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAxTQIC7vZAHHZ7WVv/5x/qH1RAgMQI+y6Xtsn73rWOgeBQjHKbmIEIlgrebyWWFCXjmzIP0NYJrGehenmPWK5bF/TRDstbM8uVQCUWpoRAHzuhIxPSYW6k/w2+HdCECF2gnGmmw1cT6nHjfCyKGsM0On0HDvxP8I5YQIIlzNigP32n1hVnQP+UuInj0wLIdOBIWkHdnFewzGK2+qjF2wmEjx+vqHDnxdUTay5DfTGaqgA9AKjgXNjLEbKlEWvy0tj7UzQRHd24a5+2x/R4Pc7PF/y6OxAwYBZnEPO0sJwio4uqL9CYZcvaHGCLOIMwQmNTPMKGC9nt3PSjujfHUBX3wIDAQAB
```
An explanation of the above record:  
- `v=DKIM1`-> This is the version of the DKIM record. This is optional.
- `k=rsa`-> This is the key type. The default value is RSA. RSA is an encryption algorithm.
- `p=`-> This is the public key that will be matched to the private key, which was created during the DKIM setup process.

For additional information see the DKIM resource [here](https://dmarcian.com/dkim-selectors/) and [here](https://knowledge.validity.com/hc/en-us/articles/222481088-DKIM-DNS-record-overview)  

What is DMARC?  
According to dmarcian, "DMARC (Domain-based Message Authentication Reporting, & Conformance), an open source standard, uses a concept called alignment to tie the result of two other open source standards, SPF (a published list of servers authorized to send email on behalf of a domain) and DKIM (a tamper-evident domain seal associated with a piece of email), to the content of an email. If not already set up, putting a DMARC record for your domain will provide feedback that will allow you to troubleshoot your SPF and DKIM configurations if needed."  

How does a basic DMARC record look like?  
`v=DMARC1; p=quarantine; rua=mailto:postmaster@test.com`  
An explanation of the above record:  
- `v=DMARC1`-> Must be capitalized and is not optional
- `p=quarantine`-> If a check fails, then an email will be sent to the spam folder (DMARC Policy)
- `rua=mailto:postmaster@test.com`->  Aggregate reports will be sent to this email address

For additional information see DMARC resources [here](https://dmarcian.com/what-is-a-dmarc-record/) and [here](https://dmarc.org/overview/). See the resource below about DMARC [Alignment](https://dmarcian.com/alignment/).  

Let's use the Domain Health Checker at [dmarcian.com](https://dmarcian.com/domain-checker/) and check the DMARC status of adeo.com.tr.  

![test](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/55b4a4ba-f75c-4d4e-8eab-5d764a66df69)  

What is [S/MIME](https://docs.microsoft.com/en-us/exchange/security-and-compliance/smime-exo/smime-exo)?  
According to Microsoft, "S/MIME (Secure/Multipurpose internet Mail Extensions) is a widely accepted protocol for sending digitally signed and encrypted messages."  

The 2 main components for S/MIME are: 
- Digital Signatures
- Encryption

S/MIME guarantees data integrity and non-repudiation using Public Key Cryptography.  
![encrypt](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/329402cb-836d-43fe-8a67-5410218cd9eb)  

