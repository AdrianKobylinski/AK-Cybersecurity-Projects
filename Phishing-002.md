# Phishing Email Analysis Report

**Author:** Adrian Kobyliński
**Date:** June 2026
**Category:** Email Security Analysis / Cybersecurity Lab

---

## Executive Summary

This analysis examined a suspected phishing email impersonating Microsoft account security services. The investigation included header inspection, infrastructure attribution, email authentication validation, and URL reputation analysis.

Multiple indicators of compromise were identified, including sender spoofing, VPS-based hosting infrastructure, and failed or missing email authentication mechanisms (SPF, DKIM, and DMARC permerror). Additionally, the embedded URL showed partial malicious reputation across security vendors.

The combination of social engineering techniques and technical inconsistencies strongly indicates malicious intent aimed at credential harvesting.

**Final Verdict:** Likely phishing attempt with high confidence based on email header and infrastructure analysis.


---

## 1. Introduction

This laboratory exercise was conducted for educational purposes to develop practical skills in email security analysis and phishing detection.

The analyzed email was treated as a case study. The investigation focused on identifying malicious indicators through content inspection, email header analysis, infrastructure attribution, authentication validation, and threat intelligence correlation.

---

## 2. Email Content Review

<img width="1918" height="975" alt="image" src="https://github.com/user-attachments/assets/0e8837ff-f870-49cc-8fd5-88c9c42147a6" />


*The email impersonated Microsoft's account security team and falsely claimed that unusual sign-in activity had been detected on the recipient's account.
The message informed the recipient of a login attempt originating from Russia/Moscow and provided technical details such as an IP address, operating system, browser, and timestamp to increase credibility.**

**The email relied on several common social engineering techniques, including:**
*  Account security concern (unauthorized login notification)
*  Fear of account compromise
*  Sense of urgency requiring immediate action

---

## 3. Header Extraction and Initial Inspection

Key email header fields were analyzed:

* **From:** no-reply@access-accsecurity.com  
* **Return-Path:** bounce@thcultarfdes.co.uk  
* **Message-ID:** [<032672b4-77ca-42f8-a036-9711e91bd1f3@DB8EUR06FT032.eop-eur06.prod.protection.outlook.com>]
-This does show the email passed through Microsoft Exchange Online Protection (EOP)
* **Source IP:** 89.144.44.2 
-public IP (not internal mail server)
* **Date:** Tue, 2023-09-08T05:47:04Z
* **Received:**  Fri, 8 Sep 2023 05:47:06
 +0000
  <img width="1910" height="801" alt="image" src="https://github.com/user-attachments/assets/767149b1-f38b-41e6-8aab-126eddcda6ca" />



## 4. Sender Identity Analysis

**A clear mismatch was identified between the visible sender identity and the actual sending infrastructure:**

* Claimed sender address appears to impersonate a security-related service (no-reply@access-accsecurity.com) 
* Return-Path is unrelated to the displayed sender and uses a different domain (bounce@thcultarfdes.co.uk) 
* The Return-Path domain appears randomly generated, consistent with disposable or automated phishing infrastructure
* Source IP (89.144.44.2) is part of the 89.144.44.0/24 network block 
* Message was relayed through Microsoft Exchange Online Protection (eop-eur06.prod.protection.outlook.com), indicating downstream filtering rather than origin authentication

**This combination strongly indicates abuse of email spoofing or phishing campaign delivery**

---

## 5. Infrastructure and IP Analysis

* **Source IP:** 89.144.44.2
* **Hosting Provider:** MatHost.eu
* **ASN:** AS214762
* **Geolocation:** Poland (Warsaw)

Mathost.eu is a web hosting provider offering VPS (Virtual Private Server) services.

## 6. Email Authentication Analysis

Authentication mechanisms were evaluated:

* SPF: none
* DKIM: none
* DMARC: permerror



  <img width="889" height="171" alt="image" src="https://github.com/user-attachments/assets/69d40ceb-7b67-4d0f-8bd9-63605c69bfd1" />

  <img width="1910" height="500" alt="image" src="https://github.com/user-attachments/assets/a0363eac-cc05-4a7a-b612-8c8a07c474e8" />



## 7. URL Analysis

<img width="1919" height="812" alt="image" src="https://github.com/user-attachments/assets/fe2306a8-52e2-477e-a280-a0e91b1b05ba" />

10/92 security vendors flagged this URL as malicious

<img width="643" height="162" alt="image" src="https://github.com/user-attachments/assets/d78a1568-9e51-4394-a487-4242c2caba54" />

**The referenced URL was inaccessible during the investigation. Consequently, dynamic analysis of the phishing landing page, redirects, and potential payloads could not be performed.**

<img width="1919" height="892" alt="image" src="https://github.com/user-attachments/assets/35cf028f-b81a-41e0-95c8-9010858c8676" />



## 8. Key Findings

- Sender impersonates Microsoft account security services  
- Uses fake “unusual sign-in activity” alert to create urgency  
- Strong social engineering via fear-based messaging  
- Domain spoofing detected (sender ≠ infrastructure)  
- Return-Path differs significantly from From-address  
- Email processed via Microsoft EOP, not originated by it  
- Source IP belongs to VPS infrastructure (MatHost.eu / AS214762)  
- SPF, DKIM, and DMARC all failed or are missing  
- DMARC shows permerror (misconfiguration or invalid record)  
- URL shows partial malicious reputation (10/92 engines)  
- Strong mismatch between narrative and technical origin  

---

## 9. Indicators of Compromise (IOCs)

### Email Indicators

| Indicator      | Value |
| -------------- | ----- |
| Sender Address | no-reply@access-accsecurity.com |
| Return-Path    | bounce@thcultarfdes.co.uk |
| Message-ID     | `<032672b4-77ca-42f8-a036-9711e91bd1f3@DB8EUR06FT032.eop-eur06.prod.protection.outlook.com>` |

---

### Network Indicators

| Indicator   | Value |
| ----------- | ----- |
| Source IP   | 89.144.44.2 |
| ASN         | AS214762 |
| Provider    | MatHost.eu |
| Location    | Warsaw, Poland |

---

### URL Indicators

| Indicator | Value |
| --------- | ----- |
| Status    | 10/92 vendors flagged as malicious |
| Type      | Credential phishing (suspected) |

---

### Authentication Status

| Mechanism | Status |
| ---------- | ------ |
| SPF        | None |
| DKIM       | None |
| DMARC      | Permerror |
| CompAuth   | Fail |

---

## 10. Risk Assessment

| Category                | Level |
| ----------------------- | ----- |
| Social Engineering      | High |
| Sender Legitimacy       | Low |
| Infrastructure Trust    | Low |
| Authentication Validity | Low |
| IP Reputation           | Medium |
| URL Risk                | High |
| Overall Risk            | High |

**Final Classification:** Likely Phishing Attempt

 (URL risk was assessed as High based on threat intelligence detections (10/92 vendors) despite the URL being inaccessible during analysis.)

---

## 11. MITRE ATT&CK Mapping

| Technique ID | Technique |
| ------------ | --------- |
| T1566.001    | Spearphishing via Email |
| T1583.003    | VPS Infrastructure Abuse |
| T1656        | Impersonation |

---


## 12. Conclusion

The analyzed email demonstrates strong indicators of a phishing attempt using impersonation of Microsoft security services combined with infrastructure hosted on VPS providers.

Multiple inconsistencies between sender identity, infrastructure origin, and authentication results confirm malicious intent.

**Final Verdict:** High-confidence phishing campaign.

---

## 13. References

- RF Peixoto – Phishing Email Dataset  
  https://github.com/rf-peixoto/phishing_pot/blob/main/email/sample-10.eml  

- VirusTotal  
  https://www.virustotal.com  

- MXToolbox  
  https://mxtoolbox.com  

- MITRE ATT&CK Framework  
  https://attack.mitre.org  

