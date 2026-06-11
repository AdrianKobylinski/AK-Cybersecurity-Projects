# CyberDefenders - Sysinternals Lab Write-up

## Introduction

This write-up covers the **Sysinternals Lab** challenge from CyberDefenders. The objective was to analyze an E01 forensic disk image and identify indicators of compromise using forensic artifacts and threat intelligence resources.

The investigation was performed using several forensic tools, including **Autopsy**, **AmCacheParser**, and **VirusTotal**. As I am still developing my digital forensics skills, this report reflects my learning process and methodology.

---

## Scenario

A user believed they were downloading the Sysinternals tool suite and attempted to execute it. However, the tools failed to launch and later became inaccessible. Shortly afterward, the user noticed that the system gradually became slower and less responsive.

---

## Tools Used

* Autopsy
* AmCacheParser (Eric Zimmerman)
* VirusTotal
* PowerShell Artifact Analysis

---

# 1. What was the malicious executable file name that the user downloaded?

To begin the investigation, I installed and launched Autopsy within a virtual machine. After importing the E01 image, I performed the analysis using the default ingest modules.

Once processing completed, I navigated to the Public user's Downloads directory and identified a single executable file.

 <img width="1717" height="809" alt="image" src="https://github.com/user-attachments/assets/c9e0f950-236d-45b1-8bf4-0c3cb13a70b7" />


**Answer:** `Sysinternals.exe`

---

# 2. When was the last time the malicious executable was modified? (UTC)

To determine when the executable was last modified, I examined the file metadata in Autopsy.

The timestamp was displayed in PST (Pacific Standard Time), so I converted it to UTC to obtain the correct answer.

<img width="430" height="159" alt="image" src="https://github.com/user-attachments/assets/8558f62a-c253-4d14-b7c7-102b5d80e942" />


**Answer:** `2022-11-01 21:15:13 UTC`

---

# 3. What is the SHA1 hash value of the malware?

Initially, I attempted to obtain the SHA1 hash directly from VirusTotal using the file's SHA256 value. However, the result did not match the expected answer.

To retrieve the correct SHA1 hash, I extracted the AmCache hive and analyzed it using **AmCacheParser** by Eric Zimmerman.

**AmCache** is a Windows registry artifact that stores metadata about executed programs, installed applications, and loaded drivers.

After reviewing the parser output, I located the entry corresponding to the malicious Sysinternals executable and recovered the required SHA1 hash.

<img width="1124" height="605" alt="image" src="https://github.com/user-attachments/assets/e9182bcf-2ae9-4f33-bc06-af9cc42cba1a" />

<img width="1084" height="504" alt="image" src="https://github.com/user-attachments/assets/2909b894-d062-4521-b3e2-b15259083d4c" />

<img width="1497" height="587" alt="image" src="https://github.com/user-attachments/assets/eeb8e70a-e0ee-437e-9c28-fb02f9b63312" />


**Answer:** `fa1002b02fc5551e075ec44bb4ff9cc13d563dcf`

---

# 4. What is the malware family?

After obtaining the malware hash, I searched it in VirusTotal.

The threat intelligence information and antivirus detections identified the sample as belonging to the **Rozena** malware family.

<img width="1897" height="833" alt="image" src="https://github.com/user-attachments/assets/8f611ea5-75d3-448c-b26d-82f6b78e540d" />


**Answer:** `Rozena`

---

# 5. What is the first mapped domain's Fully Qualified Domain Name (FQDN)?

To identify the FQDN, I reviewed the network indicators associated with the malware sample in VirusTotal.

The report contained domain information used by the malware during execution.

<img width="1888" height="694" alt="image" src="https://github.com/user-attachments/assets/8066bc79-7012-48b2-b6b0-37ba48520a75" />


**Answer:** `www.malware430.com`

---

# 6. What IP address is the domain mapped to?

At first, I was unsure where to find this information. After some research, I learned that PowerShell command history is commonly stored in the `ConsoleHost_history.txt` file.

In Autopsy, I navigated to:

```text
Users\IEUser\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine
```

and examined the contents of:

```text
ConsoleHost_history.txt
```

The file contained PowerShell commands that modified the Windows hosts file:

```powershell
Add-Content -Path $env:windir\System32\drivers\etc\hosts -Value "192.168.15.10 www.malware430.com" -Force

Add-Content -Path $env:windir\System32\drivers\etc\hosts -Value "192.168.15.10 www.sysinternals.com" -Force
```

Based on these entries, I determined the IP address associated with the domain.

<img width="1400" height="852" alt="image" src="https://github.com/user-attachments/assets/18100070-c7ff-45f7-9906-9bade8ff768a" />


**Answer:** `192.168.15.10`

---

# 7. What is the name of the executable dropped by the first-stage executable?

To answer this question, I reviewed the VirusTotal behavior report associated with the malware sample.

The behavioral analysis included information about files created during execution. Among the dropped artifacts, I identified the executable created by the first-stage malware.

<img width="1275" height="714" alt="image" src="https://github.com/user-attachments/assets/065651bb-5b5f-49cc-b13a-ada4fa3d46f4" />


**Answer:** `vmtoolsIO.exe`

---

# 8. What is the name of the service installed by the 2nd-stage executable?

Using the same VirusTotal behavioral analysis reviewed in the previous question, I examined the persistence mechanisms created by the second-stage payload.

The report identified the Windows service installed by the malware.

<img width="1275" height="714" alt="image" src="https://github.com/user-attachments/assets/4932c1eb-d2c0-44c9-9c3f-082716e8e094" />


**Answer:** `VMwareIOhelperService`

---

# Conclusion

This investigation demonstrated how multiple forensic artifacts can be combined to reconstruct malware activity from a disk image.

Key findings included:

* Identification of the malicious executable.
* Recovery of the malware SHA1 hash from AmCache.
* Attribution of the sample to the Rozena malware family.
* Discovery of malicious domains and associated IP addresses.
* Identification of dropped payloads and persistence mechanisms.

The challenge provided practical experience with Autopsy, Windows forensic artifacts, and threat intelligence platforms such as VirusTotal.
