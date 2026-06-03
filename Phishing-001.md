# Phishing Email Analysis Report

**Author:** Adrian Kobyliński
**Date:** June 2026
**Category:** Email Security Analysis / Cybersecurity Lab

---

## Executive Summary

This report presents an analysis of a suspected phishing email impersonating Bradesco Bank and its Livelo rewards program. The investigation included email content analysis, header inspection, sender validation, infrastructure attribution, authentication checks, threat intelligence enrichment, and URL availability assessment.

Multiple indicators of compromise were identified, including sender spoofing, use of cloud-based infrastructure, missing email authentication mechanisms, and social engineering techniques designed to create urgency and manipulate user behavior.

The embedded URL referenced in the email was found to be **unreachable at the time of investigation**, limiting further analysis of the phishing landing page.

**Final Verdict:** Likely Phishing (High Confidence)

---

## 1. Introduction

This laboratory exercise was conducted for educational purposes to develop practical skills in email security analysis and phishing detection.

The analyzed email was treated as a case study. The investigation focused on identifying malicious indicators through content inspection, email header analysis, infrastructure attribution, authentication validation, and threat intelligence correlation.

---

## 2. Email Content Review

The email impersonated Bradesco Bank (Livelo) and falsely claimed that reward points were about to expire.

The message relied on common social engineering techniques, including:

* Urgency (points expiring “today”)
* Fear of financial loss
* Reward-based incentives
* Strong call-to-action prompts (“Use now”)

These techniques are typical of phishing campaigns designed to pressure users into immediate interaction without verification.

---

## 3. Header Extraction and Initial Inspection

Key email header fields were analyzed:

* **From:** [banco.bradesco@atendimento.com.br](mailto:banco.bradesco@atendimento.com.br)
* **Return-Path:** root@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06
* **Message-ID:** [20230919183549.39DEA3F725@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06](mailto:20230919183549.39DEA3F725@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06)
* **Source IP:** 137.184.34.4
* **Date:** Tue, 19 Sep 2023 18:35:49 UTC
* **Received:** Tue, 19 Sep 2023 18:36:46 UTC

  <img width="1910" height="801" alt="image" src="https://github.com/user-attachments/assets/767149b1-f38b-41e6-8aab-126eddcda6ca" />

---

## 4. Sender Identity Analysis

A discrepancy was identified between the visible sender and actual sending infrastructure:

* Claimed sender domain suggests legitimate financial institution
* Return-Path indicates Linux system account (`root`)
* Hostname corresponds to cloud VPS instance
* No alignment between authenticated and displayed sender identity

This strongly indicates sender spoofing and unauthorized use of infrastructure.

---

## 5. Infrastructure and IP Analysis

* **Source IP:** 137.184.34.4
* **Hosting Provider:** DigitalOcean LLC
* **ASN:** AS14061
* **Geolocation:** United States (Broomfield)

The email originated from a cloud-hosted VPS environment, which is commonly associated with automated or malicious email campaigns.

---

## 6. Email Authentication Analysis

Authentication mechanisms were evaluated:

<img width="586" height="163" alt="image" src="https://github.com/user-attachments/assets/07dd0e9d-8eea-4172-a206-cc718949620c" />


* SPF: temperror (DNS timeout)
* DKIM: none
* DMARC: temperror
* CompAuth: fail

Initial inspection indicates weak sender verification and potential spoofing.

---

## 7. Message-ID Analysis

The Message-ID confirms generation from the same VPS infrastructure:

```
<20230919183549.39DEA3F725@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06>
```

This indicates the message was not sent through legitimate corporate email infrastructure.

---

## 8. URL Analysis

<img width="1919" height="1010" alt="image" src="https://github.com/user-attachments/assets/23a4dde1-3733-4a66-9ede-913885bdb309" />


The email contained a URL intended for reward redemption.

However, during investigation, the URL was **not reachable**.

### Observations:

* No HTTP response or redirect chain available
* Landing page could not be analyzed
* No payload or content inspection possible

### Possible explanations:

* Phishing infrastructure taken offline
* Domain suspended or removed
* Time-limited / single-use phishing URL expired
* Network-level blocking prevented access

### Impact:

Due to unavailability, no further technical analysis of the URL was possible.

---

## 9. Language and Targeting Analysis

The email content is written in **Brazilian Portuguese** and references Brazilian financial services, including Bradesco Bank and the Livelo rewards program.

This indicates a localized social engineering approach aimed at Portuguese-speaking users, likely with focus on Brazilian recipients. The use of native language significantly increases message credibility and reduces user suspicion.

Additionally, the sender domain uses a Brazilian country-code structure (.com.br), reinforcing the appearance of legitimacy.

However, infrastructure analysis shows the email was sent from a cloud-hosted VPS located in the United States (DigitalOcean, AS14061), indicating a mismatch between content localization and actual hosting origin.

**Conclusion:** The campaign is best classified as a Brazilian-themed phishing attempt delivered via non-Brazilian infrastructure.

---

## 10. Threat Intelligence Analysis

External reputation sources were checked:

<img width="1911" height="635" alt="image" src="https://github.com/user-attachments/assets/a1f9dce5-62c8-450a-a8b6-ee7fced5c567" />

* VirusTotal: 1/91 vendors flagged the IP as malicious
* MXToolbox: 1/61 blacklist detection

<img width="1591" height="350" alt="image" src="https://github.com/user-attachments/assets/" />

* Hosting provider: DigitalOcean (AS14061)

While not heavily blacklisted, the presence of malicious indicators combined with authentication failures increases suspicion level.

---

## 11. Key Findings

* Sender impersonates legitimate Brazilian financial institution (Bradesco Bank / Livelo)
* Email content is written in Brazilian Portuguese (localized social engineering)
* Reward-based urgency used to manipulate user behavior
* Sender domain structure (.com.br) reinforces legitimacy spoofing
* Return-Path reveals non-corporate Linux VPS infrastructure
* Email originated from cloud-hosted environment (DigitalOcean, AS14061, USA)
* SPF, DKIM, and DMARC authentication all failed or are absent
* Clear mismatch between localized content layer and infrastructure origin
* URL referenced in email was unreachable at time of investigation
* Threat intelligence shows limited but existing malicious reputation

---

## 12. Indicators of Compromise (IOCs)

### Email Indicators

| Indicator      | Value                                                                                                                                     |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Sender Address | [banco.bradesco@atendimento.com.br](mailto:banco.bradesco@atendimento.com.br)                                                             |
| Return-Path    | root@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06                                                                                                |
| Message-ID     | [20230919183549.39DEA3F725@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06](mailto:20230919183549.39DEA3F725@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06) |

---

### Network Indicators

| Indicator   | Value                      |
| ----------- | -------------------------- |
| Source IP   | 137.184.34.4               |
| ASN         | AS14061                    |
| Provider    | DigitalOcean LLC           |
| Geolocation | United States (Broomfield) |

---

### Authentication Status

| Mechanism | Status           |
| --------- | ---------------- |
| SPF       | Fail / TempError |
| DKIM      | Not Present      |
| DMARC     | Fail / TempError |
| CompAuth  | Fail             |

---

## 13. Risk Assessment

| Category                | Level                 |
| ----------------------- | --------------------- |
| Social Engineering      | High                  |
| Sender Legitimacy       | Low                   |
| Infrastructure Trust    | Low                   |
| Authentication Validity | Low                   |
| IP Reputation           | Medium                |
| URL Verification        | Unknown (Unreachable) |
| Overall Risk            | High                  |

**Final Classification:** Likely Phishing Attempt

---

## 14. MITRE ATT&CK Mapping

| Technique ID | Technique                                      |
| ------------ | ---------------------------------------------- |
| T1566.001    | Phishing: Spearphishing via Email              |
| T1583.003    | Acquire Infrastructure: Virtual Private Server |
| T1656        | Impersonation                                  |

---

## 15. Limitations of Analysis

The investigation was limited by the unavailability of the referenced URL. As a result, no landing page inspection, redirect analysis, or payload examination could be performed.

Despite this limitation, email-level indicators and infrastructure analysis provide sufficient evidence to support the phishing classification.

---

## 16. Conclusion

The analyzed email demonstrates multiple characteristics consistent with phishing activity targeting users through impersonation of a trusted financial institution.

Technical analysis revealed inconsistencies between the claimed sender and actual infrastructure, combined with missing authentication mechanisms and strong social engineering techniques.

Although the URL was unreachable, the collected evidence is sufficient to classify the email as a high-confidence phishing attempt.

---

## 17. References

* RF Peixoto – Phishing Email Dataset
  https://github.com/rf-peixoto/phishing_pot/blob/main/email/sample-1.eml

* VirusTotal
  https://www.virustotal.com

* MXToolbox
  https://mxtoolbox.com

* MITRE ATT&CK Framework
  https://attack.mitre.org
