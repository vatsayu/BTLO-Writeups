# BTLO Challenge Write-up: The Report

**Platform:** Blue Team Labs Online (BTLO)  
**Challenge Name:** The Report  
**Category:** Security Operations  
**Difficulty:** Easy  
**Points:** 10  
**Status:** ✅ Completed  
**Author:** vatsayu  
**GitHub Repo:** https://github.com/vatsayu/BTLO-Writeups  

---

## 1. Scenario / Story

> You are working in a newly established SOC where still there is lot of work to do to make it a fully functional one. As part of gathering intel you were assigned a task to study a threat report released in 2022 and suggest some useful outcomes for your SOC.

**Summary:**  
This challenge requires analyzing the **Red Canary 2022 Threat Detection Report** (PDF). The goal is to extract key threat intelligence findings that can help improve a newly established SOC’s detection and response capabilities.

**Password for zip:** `BTLO`

---

## 2. Questions & Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Name the supply chain attack related to Java logging library in the end of 2021 (Format: AttackNickname) | `Log4j` |
| 2 | Mention the MITRE Technique ID which effected more than 50% of the customers (Format: TXXXX) | `T1059` |
| 3 | Submit the names of 2 vulnerabilities belonging to Exchange Servers (Format: VulnNickname, VulnNickname) | `ProxyLogon, ProxyShell` |
| 4 | Submit the CVE of the zero day vulnerability of a driver which led to RCE and gain SYSTEM privileges (Format: CVE-XXXX-XXXXX) | `CVE-2021-34527` |
| 5 | Mention the 2 adversary groups that leverage SEO to gain initial access (Format: Group1, Group2) | `Gootkit, Yellow Cockatoo` |
| 6 | In the detection rule, what should be mentioned as parent process if we are looking for execution of malicious js files [Hint: Not CMD] (Format: ParentProcessName.exe) | `wscript.exe` |
| 7 | Ransomware gangs started using affiliate model to gain initial access. Name the precursors used by affiliates of Conti ransomware group (Format: Affiliate1, Affiliate2, Affiliate3) | `Qbot, Bazar, IcedID` |
| 8 | The main target of coin miners was outdated software. Mention the 2 outdated software mentioned in the report (Format: Software1, Software2) | `JBoss, WebLogic` |
| 9 | Name the ransomware group which threatened to conduct DDoS if they didn’t pay ransom (Format: GroupName) | `Fancy Lazarus` |
| 10 | What is the security measure we need to enable for RDP connections in order to safeguard from ransomware attacks? (Format: XXX) | `MFA` |

---

## 3. Tools Used

- PDF Reader (Adobe Acrobat / Browser / SumatraPDF)
- Text search (Ctrl + F)

---

## 4. Investigation Steps

1. Downloaded the challenge zip and extracted it using password `BTLO`.
2. Opened the Red Canary 2022 Threat Detection Report PDF.
3. Used **Ctrl + F** extensively to search for keywords related to each question.
4. Cross-checked findings within the relevant sections of the report (Top Techniques, Initial Access, Ransomware, etc.).

**Useful search keywords used:**
- `Log4` / `Java logging`
- `TOP TECHNIQUES` / `50%`
- `Exchange` / `Proxy`
- `CVE` + `driver`
- `SEO`
- `javascript` / `wscript`
- `Conti` / `affiliate`
- `outdated` / `coin miner`
- `DDoS` / `ransomware`
- `RDP`

---

## 5. Key Takeaways for a New SOC

From the report, the following recommendations are useful for a newly established SOC:

- Prioritize detection for **T1059 (Command and Scripting Interpreter)** — it impacted more than 50% of customers.
- Monitor for known high-impact vulnerabilities such as **ProxyLogon**, **ProxyShell**, and **PrintNightmare (CVE-2021-34527)**.
- Detect SEO poisoning campaigns used by groups like **Gootkit** and **Yellow Cockatoo**.
- Create detection rules looking for `wscript.exe` as parent process when executing `.js` files.
- Watch for Conti ransomware precursors: **Qbot, Bazar, IcedID**.
- Enforce **MFA** on all RDP connections.
- Monitor outdated software (especially **JBoss** and **WebLogic**) commonly targeted by coin miners.

---

## 6. MITRE ATT&CK Mapping

| Technique ID | Technique Name                     | Relevance in Report                  |
|--------------|------------------------------------|--------------------------------------|
| T1059        | Command and Scripting Interpreter  | Most observed technique (>50%)       |
| T1190        | Exploit Public-Facing Application  | Exchange vulnerabilities             |
| T1189        | Drive-by Compromise                | SEO poisoning                        |
| T1021.001    | Remote Services: RDP               | Ransomware initial access            |
| T1498        | Network Denial of Service          | Fancy Lazarus DDoS threats           |

---

## 7. Lessons Learned

- Threat reports are extremely valuable for building detection use cases in a new SOC.
- Searching systematically with Ctrl+F is highly effective for report-based challenges.
- Understanding real-world trends (Log4j, ProxyLogon, Conti affiliates, etc.) helps prioritize SOC efforts.
- Simple security controls like MFA on RDP can significantly reduce ransomware risk.

---

## 8. References

- [Blue Team Labs Online](https://blueteamlabs.online)
- Red Canary 2022 Threat Detection Report (provided in the challenge)
