# Micrsoft 30 Day Challenge Mini Project # 2 

Simulate a suspicious email, then walk through how a SOC analyst would investigate and respond.
Table of Contents:
1. Set up SAFE LINK Policy
2. Investigation
3. Report

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


I test the policy by sending a an email to a user in my domain to see if the Safe LINK policy was impllemented :
<img width="1888" height="477" alt="image" src="https://github.com/user-attachments/assets/8ced313f-88e4-48ef-af66-d861f32e5759" />





## Investigation

To invetigate the alert in Microsoft Defender naviated to :
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









---












Absolutely — I can write a full professional SOC incident report using the template you provided, based only on the email headers you shared.

Below is a clean, realistic, investigation-ready report.

📝 SOC Phishing Investigation Report

Case: Suspicious Email — “Urgent Payroll Update – Action Required Today”
Date Investigated: Nov 14, 2025
Analyst: [Vinny]

1. Findings (What did you find?)

A suspicious email was sent from cybersmoke312@gmail.com
 to bob@mydfir1.onmicrosoft.com
.

Subject attempts to create urgency: “Urgent Payroll Update – Action Required Today”

Email passed SPF, DKIM, DMARC, and Composite Authentication — but this DOES NOT guarantee safety.

Sender domain is free Gmail, not a corporate payroll domain.

Message references payroll updates, which is a common phishing lure.

Email originated from a Google mail server (mail-oi1-x231.google.com).

No explicit malware found in the header, but strong indicators of phishing intent.

2. Investigation Summary (What happened?)

A suspicious email claiming urgent payroll action was delivered to a user within the organization. Although the email passed technical authentication checks (SPF, DKIM, DMARC), further analysis shows that the sender uses a free Gmail account and impersonates a payroll-related communication. This is consistent with credential-harvesting phishing campaigns that rely on social engineering rather than spoofing domains.

The email was flagged for investigation due to its subject line, inconsistencies in the sender identity, and unusual reply pattern. No malicious attachment was included, but the theme of “urgent payroll update” is strongly aligned with phishing attempts.

3. Who, What, When, Where, Why, How
Who – Who was involved?

Sender: “Cyber Smoke” cybersmoke312@gmail.com

Recipient: bob@mydfir1.onmicrosoft.com

Infrastructure: Google SMTP servers → Microsoft 365 mail protection → Organization mail server

What – What happened?

A suspicious email impersonating payroll communications was sent to an employee. The email attempts to create urgency and may have included a phishing link (body not provided).

When – When did this occur and is it still happening?

Email sent: Fri, Nov 14, 2025, 12:40 PM EST

Email processed through Microsoft transport at: 17:40 UTC

Appears to be a single email; no evidence of ongoing attempts.

Where – Where in the environment did this happen?

Delivered to the user’s Microsoft 365 mailbox.

Processed by Microsoft Exchange Online Protection (EOP).

Routed through multiple Outlook data centers before final delivery.

Why – Why did this happen? (If known)

Likely part of a phishing campaign targeting users with payroll-themed lures.

Attackers often use Gmail accounts to bypass domain reputation filters.

Urgency in subject line suggests attempt to prompt the user to click a link.

How – How did this happen?

Email arrived legitimately through Gmail’s SMTP server (not spoofed).

SPF, DKIM, and DMARC all passed because Gmail’s signature is valid.

The attacker did not spoof the organization’s domain — instead they used social engineering.

Email slipped past Microsoft spam filters due to:

Clean domain reputation

Proper authentication

Lack of obvious malware

4. Recommendations (What steps should be taken?)
User Protection

Contact user to confirm:

Did they click any link?

Did they enter credentials?

Did they download anything?

If the user clicked the link:

Reset password immediately

Invalidate all session tokens in Entra ID

Review sign-ins for:

Impossible travel

Token replay

Suspicious IPs

Email & Domain Mitigation

Block sender: cybersmoke312@gmail.com

Block domain: gmail.com? (No — only block sender, NOT entire domain)

Add exact phishing indicators to:

URL blocklists

Safe Links custom block

M365 Defender advanced hunting

Threat Hunting

Run hunts for users who received or interacted with similar emails:

EmailEvents
| where SenderMailFromDomain == "gmail.com"
| where Subject contains "Payroll" or Subject contains "Urgent"


Search for URL clicks:

EmailUrlInfo
| where Url clicked contains domain from phishing email

Enhancement Recommendations

Enable M365 Advanced Phishing Protection (if not enabled)

Enable Safe Links real-time URL scanning

Improve user training on payroll scams

Add detection rules for payroll-themed social engineering attempts
