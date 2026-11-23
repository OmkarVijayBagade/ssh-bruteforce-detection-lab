<div align="center">SSH Brute-Force Detection Lab </div>
A controlled SSH authentication investigation using Kali Linux and Ubuntu Server </div></br>
Overview</br>
This project documents a controlled SSH authentication investigation in an isolated virtual lab. Kali Linux is used as the testing client and Ubuntu Server as the SSH target. The investigation focuses on reviewing authentication activity, correlating client-side attempts with server-side logs, and identifying the sequence of rejected attempts followed by a successful SSH login.

The goal is not simply to generate failed logins. It is to preserve the evidence, establish what happened from the available logs, and distinguish observed facts from assumptions.
Lab Environment
Component	Role
Kali Linux	Testing client / source of SSH authentication attempts
Ubuntu Server	SSH target / source of authentication logs
SSH	Remote access protocol under investigation
auth.log	Server-side authentication evidence


Keep this exercise inside systems and networks you own or are explicitly authorised to test. Do not run password-guessing activity against public systems.

Investigation Workflow
1. Establish SSH connectivity between the Kali and Ubuntu virtual machines.
2. Generate controlled authentication attempts against the Ubuntu SSH service.
3. Review the resulting authentication events on the Ubuntu server.
4. Correlate rejected attempts with the later successful SSH session.
5. Preserve the relevant log excerpt and screenshots in the repository.
6. Record the findings and limitations without treating failed logins alone as proof of compromise.
Evidence
The screenshots below are stored in this repository under [`evidence/screenshots/`](evidence/screenshots/). They are embedded directly from the repository using relative paths, so they remain visible on GitHub without external image hosting.
<table>
  <tr>
    <td align="center" width="50%">
      <strong>Lab evidence — 08:33:58</strong><br>
      <img src="evidence/screenshots/Screenshot%202026-11-23%20at%2008.33.58.png" alt="Lab evidence captured at 08:33:58" width="100%">
    </td>
    <td align="center" width="50%">
      <strong>Lab evidence — 08:34:09</strong><br>
      <img src="evidence/screenshots/Screenshot%202026-11-23%20at%2008.34.09.png" alt="Lab evidence captured at 08:34:09" width="100%">
    </td>
  </tr>
  <tr>
    <td align="center" width="50%">
      <strong>Lab evidence — 08:34:45</strong><br>
      <img src="evidence/screenshots/Screenshot%202026-11-23%20at%2008.34.45.png" alt="Lab evidence captured at 08:34:45" width="100%">
    </td>
    <td align="center" width="50%">
      <strong>SSH authentication attempts — 08:37:08</strong><br>
      <img src="evidence/screenshots/Screenshot%202026-11-23%20at%2008.37.08.png" alt="SSH authentication attempts from Kali" width="100%">
    </td>
  </tr>
  <tr>
    <td align="center" width="50%">
      <strong>Lab evidence — 09:03:39</strong><br>
      <img src="evidence/screenshots/Screenshot%202026-11-23%20at%2009.03.39.png" alt="Lab evidence captured at 09:03:39" width="100%">
    </td>
    <td width="50%"></td>
  </tr>
</table>Log Evidence
The collected sample log is available at [`evidence/sample-logs/auth_logs.txt`](evidence/sample-logs/auth_logs.txt).

On Ubuntu, SSH authentication events are commonly recorded in /var/log/auth.log on Debian-based systems. Relevant entries can include failed authentication, accepted authentication, session opening and session closing events.

Example commands for reviewing the server-side evidence:
# Search for SSH-related events
sudo grep -i sshd /var/log/auth.log

# Review failed authentication events
sudo grep -i "failed password" /var/log/auth.log

# Review successful authentication events
sudo grep -i "accepted" /var/log/auth.log

# Follow new authentication events live
sudo tail -f /var/log/auth.log
The sample log in this repository is the retained evidence for this exercise; the commands above are provided to make the review reproducible on the Ubuntu target.
Findings
- The recorded sequence includes initial SSH authentication rejections.
- A successful SSH login followed those rejected attempts.
- The Ubuntu authentication log provides server-side evidence of authentication outcomes.
- A successful login after failures deserves investigation, but the sequence alone does not establish that an account was compromised. The source, account, timing, authorisation context and activity after login must also be checked.
Investigation Notes and Limitations
- Authentication logs show authentication outcomes; they do not automatically reveal every command typed or every file read, modified or copied during an SSH session.
- A successful authentication event proves that the server accepted the credentials for that account. It does not, by itself, prove malicious intent.
- To investigate post-login activity, correlate authentication timestamps with other available telemetry, such as sudo records, shell history where reliable, auditd events, process accounting, file metadata and network monitoring.
- Log completeness depends on the system's logging configuration, retention and time synchronisation.
- Keep original evidence intact and work from copies when performing analysis.
Repository Structure
ssh-bruteforce-detection-lab/
├── evidence/
│   ├── sample-logs/
│   │   └── auth_logs.txt
│   └── screenshots/
│       ├── Screenshot 2026-11-23 at 08.33.58.png
│       ├── Screenshot 2026-11-23 at 08.34.09.png
│       ├── Screenshot 2026-11-23 at 08.34.45.png
│       ├── Screenshot 2026-11-23 at 08.37.08.png
│       └── Screenshot 2026-11-23 at 09.03.39.png
├── .gitignore
└── README.md
Skills Practised
- Linux authentication-log review
- SSH troubleshooting and authentication analysis
- Evidence collection and organisation
- Timeline building from host logs
- Correlating failed and successful authentication events
- Separating observed facts from assumptions and identifying telemetry gaps
Next Steps
- Add a concise event timeline with exact timestamps, source IP, target account and authentication outcome from the retained log.
- Calculate the number and rate of failed attempts, if the log supports it.
- Verify whether the successful login was expected and authorised.
- Review available post-login telemetry before drawing conclusions.
- Consider a detection rule or alert threshold, then test it against both suspicious and legitimate authentication activity.
