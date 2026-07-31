# BTLO Challenge Write-up: Phishing Analysis 2

**Platform:** Blue Team Labs Online (BTLO)  
**Challenge Name:** Phishing Analysis 2  
**Category:** Security Operations  
**Difficulty:** Easy  
**Points:** 10  
**Status:** Completed  
**Author:** vatsayu  
**GitHub Repo:** https://github.com/vatsayu/BTLO-Writeups  

---

## 1. Scenario / Story

> Put your phishing analysis skills to the test by triaging and collecting information about a recent phishing campaign.

**Summary in my own words:**  
This challenge involves analyzing a phishing email that pretends to be from Amazon. The body is Base64 encoded. We need to extract sender/recipient details, decode the body, identify the malicious links, logo source, and any related social media accounts.

---

## 2. Objectives / Questions

1. What is the sending email address?
2. What is the recipient email address?
3. What is the subject line of the email?
4. What company is the attacker trying to imitate?
5. What is the date and time the email was sent? (As copied from a text editor)
6. What is the URL of the main call-to-action button?
7. Look at the URL using URL2PNG. What is the first sentence (heading) displayed on this site?
8. When looking at the main body content in a text editor, what encoding scheme is being used?
9. What is the URL used to retrieve the company’s logo in the email?
10. For some unknown reason one of the URLs contains a Facebook profile URL. What is the username of this account?

---

## 3. Tools Used

| Tool                  | Purpose                                      |
|-----------------------|----------------------------------------------|
| Text Editor / Sublime / VS Code | View raw .eml and extract headers & body |
| Mozilla Thunderbird   | Open the email safely                        |
| CyberChef             | Decode Base64 content                        |
| URL2PNG / Browserling | View the landing page safely                 |
| Browser DevTools      | Inspect HTML source of decoded body          |

---

## 4. Investigation Steps

### Step 1: Open the Email
- Downloaded the zip (password: `btlo`).
- Opened the `.eml` file in a text editor and Thunderbird.

### Step 2: Extract Basic Headers
- Located From, To, Subject, and Date fields.

### Step 3: Decode the Body
- The email body is Base64 encoded.
- Copied the encoded content into CyberChef → **From Base64**.
- Saved the decoded HTML for easier analysis.

### Step 4: Extract URLs and Logo
- From the decoded HTML, identified the main Call-to-Action (CTA) button URL.
- Found the Amazon logo source URL.
- Identified a Facebook profile URL embedded in the content.

### Step 5: Analyze the Landing Page
- Used URL2PNG (or similar safe screenshot tool) to view the destination page.

---

## 5. Findings & Answers

| #  | Question                                                                 | Answer                                                                 | Evidence / How I found it |
|----|--------------------------------------------------------------------------|------------------------------------------------------------------------|---------------------------|
| 1  | What is the sending email address?                                       | `amazon@zyevantoby.cn`                                                 | From header               |
| 2  | What is the recipient email address?                                     | `saintington73@outlook.com`                                            | To header                 |
| 3  | What is the subject line of the email?                                   | `Your Account has been locked`                                         | Subject header            |
| 4  | What company is the attacker trying to imitate?                          | `Amazon`                                                               | Sender address + body content + logo |
| 5  | What is the date and time the email was sent?                            | `Wed, 14 Jul 2021 01:40:32 +0900`                                       | Date header (exact copy)  |
| 6  | What is the URL of the main call-to-action button?                       | `https://emea01.safelinks.protection.outlook.com/?url=https%3A%2F%2Famaozn.zzyuchengzhika.cn%2F%3Fmailtoken%3Dsaintington73%40outlook.com&data=04%7C01%7C%7C70072381ba6e49d1d12d08d94632811e%7C84df9e7fe9f640afb435aaaaaaaaaaaa%7C1%7C0%7C637618004988892053%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C1000&sdata=oPvTW08ASiViZTLfMECsvwDvguT6ODYKPQZNK3203m0%3D&reserved=0` | Extracted from decoded HTML (originalSrc / href) |
| 7  | First sentence (heading) on the page (URL2PNG)                           | `This web page could not be loaded` *(or current status of the page)*  | URL2PNG / safe browser view |
| 8  | Encoding scheme used in the main body content                            | `base64`                                                               | Visible in the raw email body |
| 9  | URL used to retrieve the company’s logo                                  | `https://images.squarespace-cdn.com/content/52e2b6d3e4b06446e8bf13ed/1500584238342-OX2L298XVSKF8AO6I3SV/amazon-logo?format=750w&content-type=image%2Fpng` | `src` attribute of the logo image in decoded HTML |
| 10 | Facebook username from the URL                                           | `amir.boyka.7`                                                         | Facebook profile URL found in the email body |

---

## 6. Indicators of Compromise (IOCs)

### Email Related
- **Sender:** amazon@zyevantoby.cn
- **Recipient:** saintington73@outlook.com
- **Subject:** Your Account has been locked
- **Date:** Wed, 14 Jul 2021 01:40:32 +0900
- **Encoding:** Base64

### Network / URLs
| Type     | Value                                                                 | Notes                              |
|----------|-----------------------------------------------------------------------|------------------------------------|
| CTA URL  | https://emea01.safelinks.protection.outlook.com/?url=https%3A%2F%2Famaozn.zzyuchengzhika.cn%2F%3Fmailtoken%3Dsaintington73%40outlook.com... | Outlook SafeLinks wrapper          |
| Real URL | https://amaozn.zzyuchengzhika.cn/?mailtoken=saintington73@outlook.com | Actual phishing domain (typosquat) |
| Logo URL | https://images.squarespace-cdn.com/content/52e2b6d3e4b06446e8bf13ed/1500584238342-OX2L298XVSKF8AO6I3SV/amazon-logo?format=750w&content-type=image%2Fpng | Legitimate-looking logo source     |
| Facebook | https://www.facebook.com/amir.boyka.7                                 | Related account                    |

### Domains
- zyevantoby.cn
- amaozn.zzyuchengzhika.cn (typosquatting "amazon")

---

## 7. MITRE ATT&CK Mapping

| Tactic          | Technique ID | Technique Name                  | Evidence                              |
|-----------------|--------------|---------------------------------|---------------------------------------|
| Initial Access  | T1566        | Phishing                        | Phishing email pretending to be Amazon |
| Initial Access  | T1566.002    | Spearphishing Link              | Malicious CTA button                  |
| Defense Evasion | T1027        | Obfuscated Files or Information | Base64 encoded email body             |
| Defense Evasion | T1036        | Masquerading                    | Typosquatted domain + Amazon branding |

---

## 8. Lessons Learned

**Key Takeaways:**
- Attackers commonly Base64-encode the email body to evade simple text filters.
- Always decode the body fully before analyzing links and images.
- Outlook SafeLinks can wrap the real malicious URL — you must extract the real destination.
- Typosquatting (amaozn instead of amazon) is still very effective.
- Checking for related social media accounts can sometimes reveal additional attacker infrastructure.

**What I struggled with:**
- Correctly extracting the full SafeLinks URL and then the real destination URL.

**What I will do differently next time:**
- Immediately search for `Content-Transfer-Encoding: base64` and decode the body.
- Use CyberChef recipes for email analysis more efficiently.

---

## 9. References

- BTLO Challenge: [Phishing Analysis 2](https://blueteamlabs.online)
- CyberChef: https://gchq.github.io/CyberChef/
- URL2PNG: https://www.url2png.com
- Browserling (safe browser): https://www.browserling.com

---

