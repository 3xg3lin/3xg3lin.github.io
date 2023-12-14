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
📝 **Note: Displaying raw/full email headers in various email clients may be different. For this, visit this [link](https://mediatemple.zendesk.com/hc/en-us/articles/204644060-how-do-i-view-email-headers-for-a-message).  

![email-raw-headers-menu-2](https://github.com/3xg3lin/3xg3lin.github.io/assets/73038148/87374bcb-ece4-46fe-bfb0-70c917044649)  






