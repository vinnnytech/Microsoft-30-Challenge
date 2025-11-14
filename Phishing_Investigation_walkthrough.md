# Micrsoft 30 Day Challenge Mini Project # 2 

### PHISHING ATTACK WALKTHROUGH

📄 Table of Contents

1.Simulated Suspicious Email
A crafted phishing-style email scenario used to demonstrate SOC investigation steps.

2.SOC Analyst Investigation Workflow
A full walkthrough of how a SOC analyst analyzes email headers, URLs, authentication results, and user activity to determine the threat level.

3.Safe Links Policy Configuration
Step-by-step instructions on how to configure Microsoft Defender Safe Links to protect users from malicious URLs in emails, Teams, and Office applications.

4.Incident Report Summary
A formal report documenting findings, indicators, timeline, root cause, and remediation steps taken during the simulated phishing investigation.


### Set up SAFE LINK Policy

In your Microsoft Defender:
Navigate to Email & Collaboration > Policies & Rules > Threat Policies > Safe Links

<img width="1519" height="837" alt="image" src="https://github.com/user-attachments/assets/4e1a8c50-542f-40c0-8940-5881dce780d0" />


Then go through the prompts below:

<img width="1119" height="228" alt="image" src="https://github.com/user-attachments/assets/75b71ae2-ea0e-46ee-bb49-69e2cadb9264" />

<img width="1073" height="345" alt="image" src="https://github.com/user-attachments/assets/a3476a07-b13f-40bd-925c-233e040a2ea2" />

<img width="1045" height="629" alt="image" src="https://github.com/user-attachments/assets/5610118a-aa92-4b3c-9fb8-4c7c52de9241" />

<img width="843" height="328" alt="image" src="https://github.com/user-attachments/assets/3e8a44df-0901-497c-82e2-7bba9b073449" />

<img width="753" height="561" alt="image" src="https://github.com/user-attachments/assets/4e0f7d51-a189-4c38-a5e4-cdca6383e8d4" />

<img width="955" height="274" alt="image" src="https://github.com/user-attachments/assets/617a754a-1fc5-46e8-b9da-9c3823e45441" />


### I test the policy by sending a an email to a user in my domain to see if the Safe LINK policy was impllemented :

<img width="1888" height="477" alt="image" src="https://github.com/user-attachments/assets/8ced313f-88e4-48ef-af66-d861f32e5759" />





## Investigation

To investigate the alert in Microsoft Defender naviated to :

   - Emails & Collaboration
   - Explorer

<img width="1881" height="809" alt="image" src="https://github.com/user-attachments/assets/02ab1f93-9bfa-440c-8315-97a0e2d2c54e" />

Navigated to Email message header and then copied and pasted the head details onto Visual Studio Code

I was look for the following:
  - Ensured that the Sender email and return path matched
  - Checked to see if SPF, DKIM and DMARC passed
  - Under the Aunthentication results, I ran the senders IP address in whois, domain tools and virustotal

<img width="1219" height="645" alt="image" src="https://github.com/user-attachments/assets/d1066046-5eab-47d6-86e7-9a02379cb983" />


Went back into Explorer and copied the susppicious URL and ran that against virustotal with no hits :

<img width="1579" height="768" alt="image" src="https://github.com/user-attachments/assets/29e43017-8509-4874-a653-99804145bd8e" />



IF there was something malicous with this link, I would navigate to the " TAKE ACTION" tab and check off on the following

<img width="1454" height="420" alt="image" src="https://github.com/user-attachments/assets/a07055cc-2bdc-4807-af4f-c5df7423ede7" />


# 📝 SOC Phishing Investigation Report

#### Findings 
   Time: 2024-11/14 17:40 UTC 
   
   Host: N/A
   
   IOC Domain: N/A
   
   IOC IP: N/A
   
   Possible Malware Family:N/A
   
   Filename: malicious powershell.exe
   
   SHA256 Hash: N/a


#### Investigation

A suspicious email was sent from cybersmoke312@gmail.com to bob@mydfir1.onmicrosoft.com, using the subject line “Urgent Payroll Update – Action Required Today” to create a false sense of urgency. Although the message passed SPF, DKIM, DMARC, and Composite Authentication, these checks only confirm the email came from a legitimate Gmail server—not that it is safe. The sender is using a free Gmail account rather than an official payroll or corporate domain, which is a common red flag in phishing campaigns. Combined with the payroll-themed lure and the email’s origin from a Google mail server (mail-oi1-x231.google.com), the message shows strong indicators of phishing intent even though no direct malware is visible in the header.

WHO – SENDER IP 2001:4860:4864:20::2b

WHAT – Received a malicious URL link https://secure-payroll-update.example.com/update_token.ps1  

WHEN: Based on the investigation, the link was sent on 2024-11/14 17:40 UTC

WHERE: The malicious URL was sent to cybersmoke312@gmail.com

WHY: The intent behind was for the attacker to trick the victim to download malware, steal credentials, or establish remote access — all without needing an EXE file that antivirus would catch

HOW: IF the link was clicked on, the user would be redirected to a fake secure payroll site and the sign into his account. The credential would be captured by the attacker

#### Recommendations 
1) Contain the user by ensuring that their password is changed and IF not set up MFA.
2) Run KQL to see if the signing were outside of normal hours and if they were signed in from another country.
3) Although domains are relatively easy to change for an attacker, consider placing these domains in a blocklist to prevent additional compromise.




