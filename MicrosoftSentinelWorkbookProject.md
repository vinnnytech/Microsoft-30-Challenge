# 📊 Explicit Credential Logons (Event ID 4648) — Microsoft Sentinel Workbook Project

This project showcases how I used **Microsoft Sentinel Workbooks** and **Kusto Query Language (KQL)** to detect and visualize **Explicit Credential Logons (Event ID 4648)**.  

Event ID **4648** is triggered when a process attempts to log on using **explicit credentials**, which is often seen in:

- Lateral movement  
- Credential theft  
- Privilege escalation  
- Use of alternate or compromised accounts  

Because of this, 4648 is a high-value event for **SOC analysts** and **threat hunters**.

---

## 🚀 Goal of the Project

My main goal was to:

- Build a **custom dashboard** in the **Workbook** module of `security.microsoft.com`
- Use a **KQL query** to hunt for suspicious explicit credential logons
- Visualize the results using a **pie chart** and other workbook visualizations
- Show how this can help identify abnormal account behavior in a SOC environment

---

## 🧭 Navigating to Sentinel Workbooks

To create the dashboard, I used the following path in the portal:

1. Go to **security.microsoft.com**
2. Navigate to:  
   **Microsoft Sentinel → Threat Management → Workbooks**
3. Create a new workbook and add a **KQL query control**
4. Use the KQL query below as the data source for visualizations

<img width="1031" height="471" alt="image" src="https://github.com/user-attachments/assets/b5aa7399-1f56-40fb-bac9-fb47221fa05f" />


---
## KQL Threat Hunting Example: Investigating Explicit Credential Logons (Event ID 4648)

As part of my Microsoft Sentinel detective work, I use KQL to hunt for suspicious authentication activity—especially events tied to credential misuse and lateral movement. The query below focuses on Event ID 4648, which indicates that a process attempted to log on using explicit credentials (often associated with attacker techniques such as pass-the-hash or privilege escalation).
KQL

## 🧠 KQL Query Used

```kusto
SecurityEvent_CL
| where TimeGenerated >= ago(30d)
| where EventID_s == "4648"
| where isnotempty(Activity_s) and isnotempty(Account_s)
| project TimeGenerated, Account_s, Activity_s, Computer, EventID_s
| sort by TimeGenerated desc
| summarize Count = count() by Account_s 

```

<img width="1471" height="216" alt="image" src="https://github.com/user-attachments/assets/3c4cd24a-9a38-461b-9df1-06eb2255b2b8" />

---

## Below is a visualization of that KQL in a PIE CHART layout:

<img width="610" height="343" alt="image" src="https://github.com/user-attachments/assets/51e367ca-0ae9-4a23-bd99-4e70f56c15ea" />


