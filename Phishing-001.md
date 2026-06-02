Phishing E-mail source: https://github.com/rf-peixoto/phishing_pot/tree/main/email

Introduction
This laboratory exercise is conducted for educational purposes only and is intended to develop practical skills in the field of cybersecurity analysis.
The subject of this task is an email message provided within the lab environment. The email will be treated as a case study for analysis, including examination of its structure, content, and potential security implications.
In the following sections, the email will be presented in detail, followed by a systematic analysis focusing on identifying possible threats, indicators of malicious activity, and relevant security considerations.

<img width="1919" height="978" alt="image" src="https://github.com/user-attachments/assets/03247b36-31b9-469b-8df1-c47e69353307" />

1. Initial Assessment and Social Engineering Indicators
At first glance, the email demonstrates clear characteristics of a phishing attempt leveraging social engineering techniques.
The content relies on several psychological manipulation techniques, including:
-urgency (“expire TODAY”),
-fear of loss (losing reward points),
-incentive-based persuasion (air miles, discounts up to 35%),
-and a clear call-to-action (“Use now”).

2. Sender Identity and Header Inconsistencies

The email shows significant inconsistencies between the visible sender and the actual sending infrastructure.

From: banco.bradesco@atendimento.com.br
Return-Path: root@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06
Message-ID: <20230919183549.39DEA3F725@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06>

The visible sender domain (atendimento.com.br) does not match the actual sending host, which is a Linux-based VPS hostname. Additionally, the use of the root system account in the Return-Path strongly indicates automated or misconfigured mail delivery rather than legitimate corporate infrastructure.

These inconsistencies represent a strong indicator of email spoofing.

