# SSH Brute-Force Detection & Incident Investigation Lab

![Project Status](https://img.shields.io/badge/Status-In%20Progress-orange)
![Platform](https://img.shields.io/badge/Platform-Linux-blue)
![Language](https://img.shields.io/badge/Language-Python-yellow)
![Environment](https://img.shields.io/badge/Environment-VMware%20Fusion-purple)

## 1. Project Overview

A practical cybersecurity home lab designed to investigate
SSH authentication activity, identify suspicious login
patterns, and develop detection capabilities using Python.

The project follows a hands-on Blue Team investigation
workflow, from generating controlled authentication events
to analysing logs and documenting findings.

**Project status:** In Progress

## 2. Objectives

- Understand SSH authentication and server-side logging.
- Generate controlled failed and successful authentication events.
- Investigate Linux authentication logs.
- Identify repeated authentication failures.
- Develop a Python-based SSH log analysis tool.
- Document investigation findings and recommendations.

## 3. Lab Architecture

The environment runs on VMware Fusion using a private
NAT network.

| Machine | Operating System | Architecture | IP Address | Role |
|---|---|---|---|---|
| Kali | Kali Linux Rolling | ARM64 | 10.0.1.129 | Security testing |
| Ubuntu | Ubuntu 26.04.1 LTS | ARM64 | 10.0.1.131 | SSH target and log collection |
| Windows | Windows 10 Home | ARM64 | 10.0.1.132 | Future endpoint monitoring |

**Network configuration**

- Network: 10.0.1.0/24
- Subnet mask: 255.255.255.0
- Default gateway: 10.0.1.2
- Virtualisation: VMware Fusion NAT

## 4. Initial Environment Verification

### 4.1 Network Connectivity

Connectivity between Kali and Ubuntu was tested using ICMP.

Command:

```bash
ping -c 4 10.0.1.131
```

Result:

- 4 packets transmitted
- 4 packets received
- 0% packet loss

TCP connectivity to the SSH service was also verified.

```bash
nc -vz 10.0.1.131 22
```

Result: Connection successful.

### 4.2 SSH Service Verification

Ubuntu was listening on TCP port 22 over IPv4 and IPv6.

The SSH server uses systemd socket activation.

### 4.3 SSH Authentication

An SSH connection was initiated from Kali to Ubuntu.

```bash
ssh omkaru@10.0.1.131
```

Initial authentication attempts were rejected.
A subsequent login succeeded using password authentication.

The successful login was confirmed in Ubuntu's authentication log.

### 4.4 Authentication Log Inspection

Log location:

```text
/var/log/auth.log
```

Inspection command:

```bash
sudo tail -n 30 /var/log/auth.log
```

Observed server-side events included:

```text
Server listening on 0.0.0.0 port 22.
Server listening on :: port 22.
Accepted password for omkaru from 10.0.1.129
pam_unix(sshd:session): session opened for user omkaru
```

These entries confirm SSH service availability and
successful authentication from the Kali VM.

The failed authentication attempts visible in the
client-side screenshots still require correlation with
server-side authentication records.

## 5. Evidence and Screenshots

### 5.1 Secure Log Transfer Using SCP

An authentication log file was transferred from Ubuntu
to the Mac host using SCP.

![SCP log transfer](evidence/screenshots/01-mac-scp-transfer.png)

### 5.2 SSH Authentication Attempts from Kali

Initial authentication rejections followed by a
successful SSH login.

![SSH authentication attempts](evidence/screenshots/02-kali-ssh-attempts.png)

### 5.3 Ubuntu Authentication Logs

Inspection of server-side SSH and PAM authentication events.

![Ubuntu authentication log](evidence/screenshots/03-ubuntu-auth-log.png)

### 5.4 Ubuntu System Information

Ubuntu operating system, kernel, architecture and
virtualisation information.

![Ubuntu system information](evidence/screenshots/04-ubuntu-system-info.png)

### 5.5 Successful SSH Session

Successful remote login from Kali to Ubuntu.

![Successful SSH session](evidence/screenshots/05-successful-ssh-session.png)

## 6. Tools and Technologies

| Tool | Purpose |
|---|---|
| VMware Fusion | Virtual lab environment |
| Kali Linux | Controlled security testing |
| Ubuntu Server | SSH target and log source |
| OpenSSH | Remote authentication |
| SCP | Secure file transfer |
| Linux CLI | System and log investigation |
| Python | Planned detection automation |
| Git and GitHub | Version control and documentation |

## 7. Investigation Workflow

- [x] Configure virtual lab environment.
- [x] Verify Kali-to-Ubuntu network connectivity.
- [x] Verify SSH port accessibility.
- [x] Establish successful SSH authentication.
- [x] Inspect Ubuntu authentication logs.
- [x] Transfer an authentication log file to the Mac host.
- [ ] Correlate failed login attempts with server-side logs.
- [ ] Generate controlled authentication failures.
- [ ] Develop Python log parser.
- [ ] Implement suspicious login detection.
- [ ] Investigate detection results.
- [ ] Prepare final incident report.

## 8. Planned Detection Capabilities

The Python detection tool will aim to identify:

- Repeated failed SSH authentication attempts.
- Source IP addresses associated with failures.
- Targeted usernames.
- Successful logins following repeated failures.
- Authentication activity within configurable time windows.

Detection thresholds will be configurable and evaluated
against controlled lab data.

## 9. Security and Scope

All testing is restricted to virtual machines controlled
by the author within a private lab network.

No external systems are targeted.

Authentication logs and screenshots will be reviewed
and sanitised before public publication.

## 10. Learning Outcomes

This project is intended to develop practical experience in:

- Linux authentication mechanisms.
- SSH service configuration.
- Network connectivity verification.
- Log analysis and event correlation.
- Python-based security automation.
- Incident investigation and technical reporting.

## 11. Future Improvements

- Add structured JSON detection output.
- Introduce severity classification.
- Integrate detection results with a SIEM.
- Build visualisation for authentication events.
- Develop incident response recommendations.

---

**Author:** Omkar Vijay Bagade

**Focus:** Blue Team | SOC Analysis | Detection Engineering
