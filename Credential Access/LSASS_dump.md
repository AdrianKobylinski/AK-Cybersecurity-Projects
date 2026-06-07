## Detection Analysis

### Understanding LSASS

LSASS (Local Security Authority Subsystem Service) is a Windows process responsible for authentication and credential management.

Process path:

```text
C:\Windows\System32\lsass.exe
```

Attackers frequently target LSASS because it may contain:

* NTLM password hashes
* Kerberos tickets
* Authentication tokens
* Cached credentials

---

### Investigating Relevant Sysmon Events

After researching LSASS credential dumping techniques, I filtered Sysmon logs for the following Event IDs:

| Event ID | Description    |
| -------- | -------------- |
| 1        | Process Create |
| 10       | Process Access |
| 11       | File Create    |

The goal was to identify events associated with processes accessing LSASS and creating memory dump files.

During the investigation, four relevant events were identified.

---

### Log 1 - ProcDump Accessing LSASS

Event ID: 10 (Process Access)

Observed:

```text
SourceImage:
C:\Users\IEUser\Desktop\procdump.exe

TargetImage:
C:\Windows\System32\lsass.exe
```

This event shows ProcDump creating a memory dump file shortly after accessing LSASS, strengthening the evidence of credential dumping activity.

<img width="1199" height="786" alt="image" src="https://github.com/user-attachments/assets/b9d3572a-7ceb-49a4-8b21-42310274429d" />

---

### Log 2 - ProcDump Creating an LSASS Dump File

Event ID: 11 (File Create)

Observed:

```text
TargetFilename:
C:\Users\IEUser\Desktop\lsass.exe_190317_120941.dmp
```

This event shows ProcDump creating a memory dump file shortly after accessing LSASS.

<img width="1193" height="793" alt="image" src="https://github.com/user-attachments/assets/3a9d5d4f-cb94-416a-ad2f-cb0abf663655" />


---

### Log 3 - Task Manager Accessing LSASS

Event ID: 10 (Process Access)

Observed:

```text
SourceImage:
C:\Windows\System32\taskmgr.exe

TargetImage:
C:\Windows\System32\lsass.exe
```

This event shows Task Manager interacting with the LSASS process.

<img width="1202" height="798" alt="image" src="https://github.com/user-attachments/assets/58be2964-217a-443d-95ea-e8e17a09264d" />



---

### Log 4 - Task Manager Creating an LSASS Dump File

Event ID: 11 (File Create)

Observed:

```text
TargetFilename:
C:\Users\IEUser\AppData\Local\Temp\lsass (2).DMP
```

This event shows Task Manager creating a dump file in the user's temporary directory.

<img width="1201" height="788" alt="image" src="https://github.com/user-attachments/assets/08ccf040-9f47-43fa-a7ee-9eac3e84c08b" />




---

No Event ID 1 entries related to the observed LSASS dumping activity were identified within the analyzed sample. However, Event ID 1 remains valuable during investigations because it can provide additional context about process execution preceding credential dumping activity.


## MITRE ATT&CK Mapping

| Technique ID | Technique | Description |
|-------------|-----------|-------------|
| T1003.001 | OS Credential Dumping: LSASS Memory | Accessing LSASS memory to obtain credentials and authentication artifacts. |

Reference:

https://attack.mitre.org/techniques/T1003/001/

## Indicators of Compromise (IOCs)

### Processes

The following processes were observed interacting with LSASS or creating memory dump files:

```text
procdump.exe
taskmgr.exe
```

### Target Process

```text
lsass.exe
```

### Dump Files

```text
lsass.exe_190317_120941.dmp
lsass (2).DMP
```

### File Paths

```text
C:\Users\IEUser\Desktop\lsass.exe_190317_120941.dmp

C:\Users\IEUser\AppData\Local\Temp\lsass (2).DMP
```

### Affected Host

```text
PC04.example.corp
```

### Relevant Sysmon Events

```text
Event ID 10 - Process Access
Event ID 11 - File Create
```

### Behavioral Indicators

- A process obtaining access to `lsass.exe`.
- Creation of a `.dmp` file shortly after LSASS access.
- ProcDump interacting with LSASS.
- Task Manager generating a memory dump of LSASS.
- Dump files stored in user-accessible directories such as Desktop or Temp.


## Lessons Learned

- Learned the role of LSASS in Windows authentication and credential management.
- Installed and configured Sysmon for endpoint monitoring and log collection.
- Investigated Sysmon telemetry to identify signs of credential dumping activity.
- Observed how ProcDump can be abused to access LSASS memory and create dump files.
- Observed that legitimate tools such as Task Manager can also be used to dump LSASS memory.
- Practiced correlating process access and file creation events to support investigations.
- Mapped observed activity to MITRE ATT&CK technique T1003.001 (OS Credential Dumping: LSASS Memory).
