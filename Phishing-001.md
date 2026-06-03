#P	hishing E-mail source: https://github.com/rf-peixoto/phishing_pot/tree/main/email

##	Introduction
This laboratory exercise is conducted for educational purposes only and is intended to develop practical skills in the field of cybersecurity analysis.
The subject of this task is an email message provided within the lab environment. The email will be treated as a case study for analysis, including examination of its structure, content, and potential security implications.
In the following sections, the email will be presented in detail, followed by a systematic analysis focusing on identifying possible threats, indicators of malicious activity, and relevant security considerations.

<img width="1919" height="978" alt="image" src="https://github.com/user-attachments/assets/03247b36-31b9-469b-8df1-c47e69353307" />

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

From: banco.bradesco@atendimento.com.br
Return-Path: root@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06
Received: from ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06 (137.184.34.4)
Message-Id: <20230919183549.39DEA3F725@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06>

Authenthication-Results:
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

VirusTotal: 1/91 vendors flagged the IP as malicious
MXToolbox: 1 blacklist detection
Hosting Provider: DigitalOcean (AS14061)

The IP showed limited but existing malicious indicators, suggesting possible abuse of cloud infrastructure.

##  9: Final Correlation and Assessment

All collected data was correlated:

Sender domain mismatch
VPS-based infrastructure
Failed authentication mechanisms
Minor malicious reputation signals
Social engineering content

Based on these findings, the email was classified as a highly suspicious phishing attempt.
