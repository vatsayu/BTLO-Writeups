# BTLO – ATT&CK

**Platform:** Blue Team Labs Online (BTLO)  
**Category:** Incident Response  
**Difficulty:** Easy  
**Points:** 10  
**Status:** ✅ Completed  
**Date:** September 2026  

---

## Scenario

You are hired as a Blue Team member for a company. You are assigned to perform threat intelligence for the company. See how you can operationalize the MITRE ATT&CK framework to solve these scenario-based problems.

---

## Theory – What is MITRE ATT&CK?

**MITRE ATT&CK** is a globally accessible knowledge base of adversary tactics and techniques based on real-world observations.

| Concept | Meaning | Example ID |
|---------|---------|------------|
| **Tactic** | The *why* – attacker’s goal | TA0001 (Initial Access) |
| **Technique** | The *how* – specific method | T1538 (Cloud Service Dashboard) |
| **Group** | Known threat actor / APT | G0099 (APT-C-36) |
| **Software** | Malware or tool used by attackers | S0372 (LockerGoga) |

### How Blue Teams use it
- Map alerts and incidents to techniques
- Prioritize detections and mitigations
- Understand attacker behavior and plan defenses
- Share threat intelligence in a common language

**Primary resource:** [https://attack.mitre.org](https://attack.mitre.org)

---

## Tools / Resources Used

- [MITRE ATT&CK website](https://attack.mitre.org)
- ATT&CK search (Groups, Techniques, Software, Tactics)
- Challenge reading material (if provided by BTLO)

---

## Investigation & Answers

### Question 1
**Your company heavily relies on cloud services like Azure AD, and Office 365 publicly. What technique should you focus on mitigating, to prevent an attacker performing Discovery activities if they have obtained valid credentials?**  
(Hint: Not using an API to interact with the cloud environment!)  
(2 points)

**Method:**
1. Attacker already has valid credentials
2. Goal = Discovery in cloud (Azure AD / Office 365)
3. Hint = **not** using an API → means using the web GUI / dashboard
4. Search ATT&CK for cloud discovery techniques that use the dashboard/GUI

**Technique found:** Cloud Service Dashboard

**Answer:** `T1538`

**Direct link:** https://attack.mitre.org/techniques/T1538/

---

### Question 2
**You were analyzing a log and found uncommon data flow on port 4050. What APT group might this be?**  
(2 points)

**Method:**
1. Port 4050 is uncommon for normal traffic
2. Search MITRE ATT&CK Groups for “4050” or non-standard port usage
3. Found: APT-C-36 (also known as Blind Eagle) has used port 4050 for C2

**Answer:** `G0099`

**Direct link:** https://attack.mitre.org/groups/G0099/

---

### Question 3
**The framework has a list of 9 techniques that falls under the tactic to try to get into your network. What is the tactic ID?**  
(2 points)

**Method:**
1. “Try to get into your network” = first foothold
2. In ATT&CK this is the **Initial Access** tactic
3. Tactic ID for Initial Access is TA0001

**Answer:** `TA0001`

**Direct link:** https://attack.mitre.org/tactics/TA0001/

---

### Question 4
**A software prohibits users from accessing their account by deleting, locking the user account, changing password etc. What such software has been documented by the framework?**  
(2 points)

**Method:**
1. Behavior = locking / deleting accounts / changing passwords → Impact
2. Related technique: Account Access Removal (T1531)
3. Search Software linked to that behavior → LockerGoga ransomware is documented doing this

**Answer:** `S0372` (LockerGoga)

**Direct link:** https://attack.mitre.org/software/S0372/

---

### Question 5
**Using ‘Pass the Hash’ technique to enter and control remote systems on a network is common. How would you detect it in your company?**  
(2 points)

**Method:**
1. Technique = Pass the Hash (T1550.002)
2. Open the Detection section on the MITRE page
3. Official guidance wording is used as the expected answer

**Answer:**  
`Monitor newly created logons and credentials used in events and review for discrepancies`

**Direct link:** https://attack.mitre.org/techniques/T1550/002/

---

## Summary of Findings

| # | Question | Answer |
|---|----------|--------|
| 1 | Cloud Discovery technique (GUI, not API) | **T1538** |
| 2 | APT group using port 4050 | **G0099** |
| 3 | Tactic ID to get into the network | **TA0001** |
| 4 | Software that locks/deletes accounts | **S0372** |
| 5 | How to detect Pass the Hash | **Monitor newly created logons and credentials used in events and review for discrepancies** |

---

## Direct Links Reference

| ID | Name | Link |
|----|------|------|
| T1538 | Cloud Service Dashboard | https://attack.mitre.org/techniques/T1538/ |
| G0099 | APT-C-36 (Blind Eagle) | https://attack.mitre.org/groups/G0099/ |
| TA0001 | Initial Access | https://attack.mitre.org/tactics/TA0001/ |
| S0372 | LockerGoga | https://attack.mitre.org/software/S0372/ |
| T1550.002 | Pass the Hash | https://attack.mitre.org/techniques/T1550/002/ |

---

## Key Takeaways

- Always start from the **tactic** (goal) then find the **technique** (method)
- Groups and Software pages are excellent for mapping real-world indicators (ports, tools, behaviors)
- Detection language on MITRE pages is often the exact wording expected in challenges
- Build the habit: for every new technique you meet → read Description + Detection + one Procedure Example
- Pass the Hash is a classic Lateral Movement technique; detection focuses on abnormal logon and credential events

---

## How to Keep Learning ATT&CK

1. Open each ID above and read the Description + Detection sections
2. Write 2 lines in your own words about what the technique does and how you would detect it
3. Link new techniques you see in future labs back to the ATT&CK matrix

---

**Challenge completed and documented.**  
Ready to upload to: https://github.com/vatsayu/BTLO-Writeups
