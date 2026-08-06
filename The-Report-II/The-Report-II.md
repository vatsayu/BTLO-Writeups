# BTLO Challenge Write-up: The Report II

**Platform:** Blue Team Labs Online (BTLO)  
**Challenge Name:** The Report II  
**Category:** Security Operations  
**Difficulty:** Medium  
**Points:** 20  
**Status:** ✅ Completed  
**Author:** vatsayu  
**GitHub Repo:** https://github.com/vatsayu/BTLO-Writeups  

---

## 1. Scenario / Story

> This challenge is an extension for an existing ‘The Report’ challenge where you are working in a newly established SOC where there is still a lot of work to do to make it a fully functional one. As part of the SOC improvement process, you were assigned a task to study a report released by MITRE and suggest some useful outcomes for your SOC.

**Note:** Answer the questions with the answers as the way you see in the document to avoid formatting issues.

**Report:** MITRE – *11 Strategies of a World-Class Cybersecurity Operations Center*  
**Password for zip:** `BTLO`

---

## 2. Questions & Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Submit the name of the units/teams (in short form) that are responsible for maintaining network and other IT equipment, incident detection and response, and security compliance and risk measurement | `NOC, SOC, ISCM` |
| 2 | After investigation, what are the 4 suggested ‘Response Options’? | `Block activity, deactivate account, continuous watching, refer to outside party` |
| 3 | What is the name of a military strategy used in SOCs to achieve a high level of situational awareness? | `OODA` |
| 4 | What is the name of the suggested organisational model if the constituency size is between 1000 to 10,000 employees? | `Distributed SOC` |
| 5 | In a Large Centralised SOC, who is responsible for generating SOC metrics, maintaining situational awareness, and conducting internal/external trainings? | `SOC Operations lead` |
| 6 | In Coordinating & National SOCs model what are the 2 functions mentioned as Optional Capability under Expanded SOC Operations Category? | `Deception, Insider Threat` |
| 7 | What are the two virtual console technologies (in short form) mentioned to support Virtual SOC/ Remote Work scenarios during pandemics? | `iLO, iDRAC` |
| 8 | What is the name of the model used to distribute work load of SOC 24/7 across different timezones to eliminate working at night hours? | `Follow-the-Sun` |
| 9 | Submit the priorities (Low, Medium, High) assigned to Phishing, Insider Threat and Pre-incident Port Scanning activities respectively | `Medium, High, Low` |
| 10 | Mention the name of the Open source Operating system mentioned, that can help in mobile incident investigations | `Santoku` |
| 11 | Before choosing a CTI tool, the document suggests tool support for 2 open threat intelligence standards (short forms) | `STIX, TAXII` |
| 12 | Name the Data Source which consumes the highest volume (typically TB’s/day)? | `PCAP` |
| 13 | In order to support forensics, what is the recommended data retention period (in months) to store logged EDR data? | `6` |
| 14 | According to the threat intelligence concept the ‘Pyramid of Pain’, what indicators are Trivial, Easy, Challenging, Tough for adversaries to change? | `Hash values, IP addresses, Tools, TTPs` |
| 15 | Name of the Red Teaming approach to mimic the TTPs of an adversary? | `Adversary emulation` |

---

## 3. Tools Used

- PDF Reader / Web Browser
- Ctrl + F (Search)

---

## 4. Investigation Steps

1. Downloaded the challenge zip and extracted it using password `BTLO`.
2. Opened the MITRE PDF (*11 Strategies of a World-Class Cybersecurity Operations Center*).
3. Used **Ctrl + F** with keywords from each question to locate the relevant sections and figures.
4. Carefully copied answers exactly as they appear in the document to avoid formatting issues.

**Useful search keywords:**
- Network / NOC / ISCM
- Basic SOC Workflow
- situational awareness / OODA
- Constituency Size
- Large SOC / SOC Operations lead
- Expanded SOC Operations
- Virtual SOC
- Follow-the-Sun
- Phishing / Incident Prioritization
- Santoku
- STIX / TAXII
- Data Source / PCAP
- EDR
- Pyramid of Pain
- Adversary emulation

---

## 5. Key Takeaways for SOC Improvement

- Understand the relationship between **NOC, SOC, and ISCM**.
- Use the **OODA loop** for high situational awareness.
- Choose the right SOC organisational model based on constituency size (e.g., Distributed SOC for 1,000–10,000 employees).
- Apply proper incident prioritization (Phishing = Medium, Insider Threat = High, Pre-incident Port Scanning = Low).
- Prefer tools that support **STIX** and **TAXII** for threat intelligence.
- Retain EDR data for at least **6 months** to support forensics.
- Focus higher on the Pyramid of Pain (TTPs are hardest for adversaries to change).

---

## 6. Lessons Learned

- Reading official frameworks (MITRE SOC strategies) is highly valuable for building or improving a SOC.
- Many answers are directly visible in figures and tables — searching efficiently is key.
- Understanding organisational models, prioritization, and data retention helps in real SOC design decisions.

---

## 7. References

- [Blue Team Labs Online](https://blueteamlabs.online)
- MITRE: *11 Strategies of a World-Class Cybersecurity Operations Center*
