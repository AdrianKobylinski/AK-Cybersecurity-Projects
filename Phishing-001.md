

##	Introduction
This laboratory exercise is conducted for educational purposes only and is intended to develop practical skills in the field of cybersecurity analysis.
The subject of this task is an email message provided within the lab environment. The email will be treated as a case study for analysis, including examination of its structure, content, and potential security implications.
In the following sections, the email will be presented in detail, followed by a systematic analysis focusing on identifying possible threats, indicators of malicious activity, and relevant security considerations.

<img width="1919" height="978" alt="image" align="center" src="https://github.com/user-attachments/assets/03247b36-31b9-469b-8df1-c47e69353307" />

##	Google translated E-mail:
You have Livelo points on your Bradesco Bank card that expire TODAY. Avoid losing these points by redeeming them for Visa Infinite points right now.
	
Bradesco Bank customers earn Livelo points every time they use their debit or credit cards; accumulating them is quick and easy.
Exchange your points for airline miles.

Discounts of up to 35% on your card statement.

92,990

ONE THOUSAND POINTS EARNED EXPIRE TODAY
Use now

Use now before they expire! Take advantage of this opportunity to redeem your points for airline miles, discounts of up to 35% on your card, or thousands of rewards from our catalog.

 ## 1.Email Content Review

The analysis began with reviewing the email body. The message impersonated Bradesco Bank (Livelo) and claimed that reward points were about to expire immediately. 
The content was designed to create urgency and pressure the recipient into quick action. Social engineering techniques such as fear of loss and reward incentives were identified at this stage.

## 2: Header Extraction and Initial Inspection

The full email headers were extracted for further investigation. The following key fields were identified for analysis:
<img width="1910" height="801" alt="image" align="center" src="https://github.com/user-attachments/assets/ca3926fd-a41a-4765-a6a8-f1dcbaabc273" />

From: banco.bradesco@atendimento.com.br

Return-Path: root@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06

Received: from ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06 (137.184.34.4)

Message-Id: <20230919183549.39DEA3F725@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06>

Authenthication-Results:

<img width="586" height="163" align="center" alt="image" src="https://github.com/user-attachments/assets/9930f7bd-5595-491e-a77e-7958cd1be52a" />


spf= temperror

Received-SPF: TempError

dkim= none

dmarc= temperror

Received Date Tue, 19 Sep 2023 18:36:46 +0000
Date Tue, 19 Sep 2023 18:35:49 +0000 (UTC)

##  3: Sender Identity Analysis

The visible sender address was compared with the actual delivery path:

From: banco.bradesco@atendimento.com.br
Return-Path: root@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06

A clear mismatch was observed between the claimed sender and the actual sending system. The use of a system-level Linux account (root) and a VPS hostname indicated non-corporate email infrastructure and potential spoofing.

  ##4: Infrastructure and IP Analysis

The originating IP address was extracted from the Received header:
<img width="1591" height="350" align="center" alt="image" src="https://github.com/user-attachments/assets/3a8c7c9d-0831-47a0-81f2-fefbc78b65a4" />

IP: 137.184.34.4
Hosting Provider: DigitalOcean, LLC
ASN: AS14061
Location: United States (Broomfield)



The email was sent from a cloud-based VPS environment, which is commonly used for temporary or automated email sending. This is not typical for legitimate financial institutions.

 ## 5: Email Authentication Check

Authentication results were reviewed:

SPF: temperror (DNS timeout)
DKIM: none (no signature present)
DMARC: temperror
CompAuth: fail

These results indicate that the email could not be properly authenticated. The absence of DKIM and failure of SPF/DMARC significantly reduce the trust level of the message.

##  6: Message-ID Analysis

The Message-ID was examined:

<20230919183549.39DEA3F725@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06>

The Message-ID was generated using the same VPS hostname, further confirming that the email originated from a non-organizational mail server.

##  7: Timestamp Verification

The relationship between the Date and Received headers was analyzed:

Date: 18:35:49 UTC
Received: 18:36:46 UTC

The difference of approximately one minute was found to be normal and consistent with standard email processing delays within mail transfer systems.

##  8: Threat Intelligence and Reputation Check

The sender IP address was checked using external threat intelligence sources:
<img width="1911" height="635" align="center" alt="Pasted image 20260603004919" src="https://github.com/user-attachments/assets/35278b41-a0a2-490f-9681-3c123996cde9" />

VirusTotal: 1/91 vendors flagged the IP as malicious
<img width="1591" height="350" align="center" alt="image" src="https://github.com/user-attachments/assets/6bc26336-a353-4181-8909-bae76ad5f005" />

MXToolbox: 1 blacklist detection
Hosting Provider: DigitalOcean (AS14061)

The IP showed limited but existing malicious indicators, suggesting possible abuse of cloud infrastructure.

## 9. Indicators of Compromise (IOCs)

| IOC Type                 | Value                                                                         | Observation                                                   |
| ------------------------ | ----------------------------------------------------------------------------- | ------------------------------------------------------------- |
| Sender Address           | [banco.bradesco@atendimento.com.br](mailto:banco.bradesco@atendimento.com.br) | Claimed sender impersonating Bradesco Bank                    |
| Return-Path              | root@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06                                    | Uses a Linux root account and VPS hostname                    |
| Source IP                | 137.184.34.4                                                                  | Originating IP extracted from the Received header             |
| Hostname                 | ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06                                         | Cloud VPS hostname, not typical corporate mail infrastructure |
| Hosting Provider         | DigitalOcean, LLC                                                             | Cloud hosting provider                                        |
| ASN                      | AS14061                                                                       | Autonomous System owned by DigitalOcean                       |
| Geolocation              | Broomfield, USA                                                               | Location associated with the source IP                        |
| SPF                      | TempError                                                                     | SPF validation could not be completed                         |
| DKIM                     | None                                                                          | Message was not DKIM signed                                   |
| DMARC                    | TempError                                                                     | DMARC validation could not be completed                       |
| CompAuth                 | Fail                                                                          | Microsoft composite authentication failed                     |
| Message-ID               | <20230919183549.39DEA3F725@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06>             | Generated by the VPS hostname                                 |
| VirusTotal Reputation    | 1/91 detections                                                               | One security vendor flagged the IP                            |
| MXToolbox Blacklists     | 1/61 detections                                                               | IP appeared on one blacklist                                  |
| Social Engineering Theme | Reward Points Expiration                                                      | Creates urgency and fear of loss                              |
| Target Brand             | Bradesco Bank (Livelo)                                                        | Brand impersonation attempt                                   |

---

## 10. Risk Assessment

| Category                       | Assessment      |
| ------------------------------ | --------------- |
| Social Engineering             | High            |
| Sender Legitimacy              | Low             |
| Infrastructure Trustworthiness | Low             |
| Authentication Validity        | Low             |
| IP Reputation                  | Medium          |
| Likelihood of Phishing         | High            |
| Overall Risk                   | High            |
| Final Verdict                  | Likely Phishing |

---

## 11. Lessons Learned

This analysis demonstrated the importance of combining technical email header analysis with content-based inspection. While the email appeared to originate from a legitimate financial institution, several technical indicators revealed inconsistencies that reduced trust in the message.

Key observations included a mismatch between the visible sender and the actual sending infrastructure, the use of a cloud-hosted VPS, and the absence of proper email authentication mechanisms such as DKIM. The investigation also highlighted how phishing campaigns commonly leverage social engineering techniques, including urgency, fear of loss, and financial incentives, to influence user behavior.

Additionally, this exercise reinforced the value of threat intelligence enrichment. Although the source IP had only limited reputation indicators, combining those findings with authentication failures and infrastructure analysis provided a stronger basis for assessing the email as suspicious.

---

## 12. Recommendations

* Classify the email as a phishing or high-confidence suspicious message.
* Quarantine the email to prevent user interaction.
* Block the sender domain and associated indicators if confirmed malicious.
* Add the identified IOCs to internal monitoring and blocklists.
* Perform an organization-wide search to determine whether other users received the same email.
* Review email gateway logs for similar messages originating from the same infrastructure.
* Investigate whether any users interacted with links or attachments contained in the email.
* Monitor authentication logs for signs of credential compromise.
* Enhance email security rules to detect similar sender mismatches and authentication failures.
* Document the findings and indicators for future phishing investigations.
* Use this case as a training example to improve user awareness regarding reward-based phishing campaigns.

### Sources:
Phishing E-mail source: https://github.com/rf-peixoto/phishing_pot/tree/main/email

MxToolbox

Virustotal

