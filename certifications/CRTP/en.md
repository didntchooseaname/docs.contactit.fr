---
label: CRTP (en)
icon: shield-lock
categories: [Cybersecurity]
date: 2024-03-19
tags: [Cybersecurity, crtp, altered, security, red, team, certification]
order: 97
description: Experience feedback on CRTP, Altered Security's Red Team certification in an Active Directory environment.
---

# 🛡️ Certified Red Team Professional

![Source: redteamdefense.org](images/redteam.webp)

Experience feedback on the "Certified Red Team Professional" certification by Altered Security.

## ▶️ Introduction

**Altered Security** (formerly Pentester Academy) was founded by **Nikhil Mittal** and offers certifications focused on **offensive security** (Red Team) in an **Active Directory** environment. Nikhil is globally recognized for his expertise in AD Red Teaming, and he has given numerous talks such as at DefCon [RACE - Minimal Rights and ACE for Active Directory Dominance](https://www.youtube.com/watch?v=F_Fy7M1AO_Q), [Powershell for Penetration Testers](https://www.youtube.com/watch?v=PezFo2Y1BUA), or [PowerPreter Post Exploitation Like a Boss](https://www.youtube.com/watch?v=NXydblaJaZQ).

:icon-code-of-conduct: The certifications offered by Altered Security are **recognized** (also in France), especially by **specialized companies** (such as ESNs offering penetration testing services to their clients).

---

## 📕 Getting Started

When you purchase the CRTP ($249 or $300 with the bootcamp), you will need to provide an email address. Using a **Gmail address will save you from having to contact support later**, as authentication to the learning platform is only via a **Google account**.

![Interface of Altered Security's learning platform](images/interface.webp)

:icon-milestone: You will then have access to the various certification resources in the **"Access Lab Material"** section, which will redirect you to a OneDrive link where you will find diagrams, explanatory videos (:warning: different from the bootcamp), the **"Lab Manual"** which is the main course (same as the "Lab Manual" section), the **"Sliver_C2"** folder which covers applying the course using a C2, and an archive containing all the tools needed for certification learning.

:icon-alert: It is highly recommended to read the **"Frequently Asked Questions"** section which, as its name suggests, will answer many of your questions.

:icon-project-roadmap: Regarding the course, there are **24 "Learning Objectives"**, each indicating a goal to achieve (one or more pieces of information to be filled out) through enumeration/exploitation of the lab environment. This covers everything from reconnaissance/enumeration/exploitation of Kerberos to certificate attacks and SQL server exploitation, while considering noise and OPSEC management.

📍 The learning environment and the certification exam environment are very similar, especially in terms of security measures:
All servers run **Windows Server 2022** with **up-to-date security patches**, **Windows Firewall and Defender are active**, some servers are in **"core"** version, and there are **applocker policies**. This means the security level is significant compared to an SMB production environment. **The presence of misconfigurations allows machine compromise.**

The entry into the CRTP begins with **bypassing these protections**.

When you successfully complete all the learning objectives, you can download a certificate:

![Lab Completion Certificate](images/labcertificate.webp)

---

## 🛠️ Bootcamp

:icon-check-circle: I highly recommend taking the **bootcamp** (live sessions and videos) in addition to the certification, whether you are **a beginner in Red Teaming and Active Directory** or not. The bootcamp consists of 4 live video sessions with **Nikhil** every Sunday for **4 hours** (recordings are available and downloadable).

:icon-zap: It adds **real value** to the learning process, providing solid foundations and going beyond the platform course. It dives deeper into **Windows log management and noise**, staying undetected by security solutions. This includes **obfuscating common scripts** like **"Mimikatz"**, **disabling/neutralizing PowerShell security measures** (ScriptBlock logging, Module logging, Transcription, AMSI), and explaining each argument passed to commands and obfuscating them as well.

:icon-mute: **Noise management is an element that will follow you throughout this bootcamp** and Nikhil will continually remind you of its importance, providing multiple practical lab examples. He will be attentive and read your questions in the chat, even asking you questions, creating a **real exchange between teacher and students**. The dedicated Discord server will centralize various bootcamp information and allow you to **interact with fellow learners**.

:icon-eye: The certification package also includes access to a **Microsoft Defender for Endpoints (MDE) instance** in the perimeter, giving you a view from the defense side as well.

---

## 💻 Exam

:icon-desktop-download: During the exam, the candidate's mission is to execute commands on the five lab machines. Obtaining local administrator rights on these machines is not required, and no flags need to be collected. The candidate has **24 hours**, including an additional hour for tool installation, to compromise the entire lab and take screenshots documenting the compromise methods, enriching the final report.

:icon-shield-check: **Microsoft Defender is active on all machines**. It can be disabled if you have local administrator privileges. It is important to note that **one machine will require its bypass**, using the methods learned during the course. During the exam, techniques such as brute-forcing, guessing, certificate abuse, or exploiting known vulnerabilities (CVEs) are not useful.

---

## 📖 Report

:icon-project-roadmap: The report must be written entirely in **English**. It should suggest **corrective measures for identified inappropriate configurations** and include references to **blog articles** to enhance credibility. It is also essential to explain the tools used and the reasons for their selection. My personal report follows a "write-up" structure:
- **Title**
- **Table of Contents**
- **Compromise Diagram**
- **Detailed Steps Description**
- **Conclusion**
- **Tools Used Presentation**
- **References to consulted blog articles**

---

## 📋 Conclusion

:icon-arrow-right: It is not necessarily required to prepare by doing boxes on online platforms (HackTheBox/ProLabs/TryHackMe...), if you already have a background in **system administration in a production Active Directory environment** and have delved into its **functionality**. Starting with the CRTP bootcamp is entirely feasible. If you are a complete beginner and have never encountered these concepts, it is better to **gain experience**, either through **practical work in a company** if possible, or **learning via the mentioned platforms**.

:icon-hourglass: With a solid foundation and significant investment, it is possible to pass the CRTP in an average of two weeks. However, if you take a 30-day lab access, **you will get an additional two weeks for free**. **All resources are accessible for life**, including future updates. Note, however, that **the certification exam is less complex than the training lab** (due to its 24-hour activity). You will then be required to submit an **English compromise report**, detailing all **observations, misconfigurations, tools used, POC sources, and remediation recommendations for each element in the environment** (recommendations score more points but are not mandatory). This report is to be written and submitted within **48 hours maximum** after the end of the exam lab activity. You can also specify that English is not your native language if this hinders your expression. The Altered Security team will take this into account when reviewing your report.

---