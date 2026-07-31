# BTLO Challenge Write-up: Phishing Analysis

**Platform:** Blue Team Labs Online (BTLO)  
**Challenge Name:** Phishing Analysis  
**Category:** Security Operations  
**Difficulty:** Easy  
**Points:** 10  
**Status:** Completed  
**Author:** vatsayu  
**GitHub Repo:** https://github.com/vatsayu/BTLO-Writeups  

---

## 1. Scenario / Story

> A user has received a phishing email and forwarded it to the SOC.  
> Can you investigate the email and attachment to collect useful artifacts?

**Summary in my own words:**  
A user received a suspicious email that looks like an undeliverable message and forwarded it to the SOC team. We need to analyze the email headers, attachment, and any embedded links to extract useful artifacts and indicators of compromise.

---

## 2. Objectives / Questions

1. Who is the primary recipient of this email?
2. What is the subject of this email?
3. What is the date and time the email was sent?
4. What is the Originating IP?
5. Perform reverse DNS on this IP address, what is the resolved host?
6. What is the name of the attached file?
7. What is the URL found inside the attachment?
8. What service is this webpage hosted on?
9. Using URL2PNG, what is the heading text on this page?

---

## 3. Tools Used

| Tool                  | Purpose                                      |
|-----------------------|----------------------------------------------|
| Mozilla Thunderbird   | Open and view .eml file safely               |
| Text Editor (Notepad++ / VS Code / Sublime) | Analyze raw email headers and body |
| CyberChef             | Decoding if needed                           |
| whois.domaintools.com | Reverse DNS / WHOIS lookup                   |
| URL2PNG               | Screenshot of the phishing page              |

---

## 4. Investigation Steps

### Step 1: Open the Email
- Downloaded the challenge zip and extracted the `.eml` file.
- Opened the email using Mozilla Thunderbird (safer than double-clicking).

### Step 2: Analyze Email Headers
- Viewed full headers in Thunderbird or opened the `.eml` in a text editor.
- Extracted key fields: From, To, Subject, Date, Originating IP, etc.

### Step 3: Analyze Attachment
- Identified the attached file.
- Opened the attachment carefully and extracted the URL inside it.

### Step 4: Investigate the URL
- Performed reverse DNS on the originating IP.
- Used URL2PNG to see the content of the linked page.

---

## 5. Findings & Answers

| # | Question                                      | Answer                                                                 | Evidence / How I found it                          |
|---|-----------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------|
| 1 | Who is the primary recipient of this email?   | `kinnar1975@yahoo.co.uk`                                               | Visible in the "To" field of the email             |
| 2 | What is the subject of this email?            | `Undeliverable: Website contact form submission`                       | Subject line of the email                          |
| 3 | What is the date and time the email was sent? | `18 March 2021 04:14`                                                  | Date header in the email                           |
| 4 | What is the Originating IP?                   | `103.9.171.10`                                                         | Found in the email headers (X-Originating-IP or Received) |
| 5 | Reverse DNS of the Originating IP             | `c5s2-1e-syd.hosting-services.net.au`                                  | whois.domaintools.com lookup                       |
| 6 | Name of the attached file                     | `Website contact form submission.eml`                                  | Attachment name in the email                       |
| 7 | URL found inside the attachment               | `https://35000usdperwwekpodf.blogspot.sg?p=9swghttps://35000usdperwwekpodf.blogspot.co.il?o=0hnd` | Extracted from the attached .eml file              |
| 8 | What service is this webpage hosted on?       | `blogspot`                                                             | Domain ends with blogspot                          |
| 9 | Heading text on the page (URL2PNG)            | `Blog has been removed`                                                | Screenshot from URL2PNG                            |

---

## 6. Indicators of Compromise (IOCs)

### Email Related
- **Recipient:** kinnar1975@yahoo.co.uk
- **Subject:** Undeliverable: Website contact form submission
- **Date:** 18 March 2021 04:14
- **Originating IP:** 103.9.171.10
- **Reverse DNS:** c5s2-1e-syd.hosting-services.net.au

### Network / URLs
| Type   | Value                                                                 | Notes                  |
|--------|-----------------------------------------------------------------------|------------------------|
| URL    | https://35000usdperwwekpodf.blogspot.sg?p=9swghttps://35000usdperwwekpodf.blogspot.co.il?o=0hnd | Found in attachment    |
| Domain | 35000usdperwwekpodf.blogspot.sg                                       | Blogspot phishing page |
| Domain | 35000usdperwwekpodf.blogspot.co.il                                    | Related Blogspot page  |

### File Related
| Filename                              | Type     | Notes                     |
|---------------------------------------|----------|---------------------------|
| Website contact form submission.eml   | Email    | Attachment containing URL |

---

## 7. MITRE ATT&CK Mapping

| Tactic          | Technique ID | Technique Name             | Evidence                          |
|-----------------|--------------|----------------------------|-----------------------------------|
| Initial Access  | T1566        | Phishing                   | Phishing email with malicious link |
| Initial Access  | T1566.002    | Spearphishing Link         | Malicious Blogspot URL            |
| Defense Evasion | T1027        | Obfuscated Files or Information | Nested/obfuscated URL structure |

---

## 8. Lessons Learned

**Key Takeaways:**
- Always analyze email headers thoroughly — the Originating IP and Received headers are critical.
- Attachments can contain further malicious content (even another .eml).
- Free hosting services like Blogspot are commonly abused for phishing.
- Tools like Thunderbird + text editor + URL2PNG make phishing analysis efficient and safe.

**What I struggled with:**
- Extracting the exact URL from the nested attachment.

**What I will do differently next time:**
- Always open .eml files in Thunderbird or a dedicated email client instead of double-clicking.
- Document every artifact immediately in a structured table.

---

## 9. References

- BTLO Challenge: [Phishing Analysis](https://blueteamlabs.online)
- URL2PNG: https://www.url2png.com
- DomainTools WHOIS: https://whois.domaintools.com
- CyberChef: https://gchq.github.io/CyberChef/

---

