<h1 align="center">

<a href="https://www.socfortress.co"><img src="https://raw.githubusercontent.com/socfortress/CoPilot/main/frontend/src/assets/images/socfortress_logo.svg" width="300" height="200"></a>

SOCFortress CoPilot Action

[![Medium](https://img.shields.io/badge/Medium-12100E?style=for-the-badge&logo=medium&logoColor=white)](https://socfortress.medium.com/)
[![YouTube Channel Subscribers](https://img.shields.io/youtube/channel/subscribers/UC4EUQtTxeC8wGrKRafI6pZg)](https://www.youtube.com/@taylorwalton_socfortress/videos)
[![Discord Shield](https://discordapp.com/api/guilds/871419379999469568/widget.png?style=shield)](https://discord.gg/UN3pNBzaEQ)
[![GitHub Sponsors](https://img.shields.io/badge/sponsor-30363D?style=for-the-badge&logo=GitHub-Sponsors&logoColor=#EA4AAA)](https://github.com/sponsors/taylorwalton)

[![Get in Touch](https://img.shields.io/badge/📧%20Get%20in%20Touch-Friendly%20Support%20Awaits!-blue?style=for-the-badge)](https://www.socfortress.co/contact_form.html)

</h1>

<h4 align="center">

CoPilot Action extends the power of [SOCFortress CoPilot](https://github.com/socfortress/CoPilot) by providing automated response capabilities through Velociraptor integration. Execute remote actions such as blocking IPs on Windows firewalls, listing users, collecting system information, and much more across your endpoint infrastructure.

</h4>

## Table of contents

-   [Overview](#overview)
-   [Prerequisites](#prerequisites)
-   [Installation](#installation)
-   [Configuration](#configuration)
-   [Usage](#usage)
    -   [Windows Actions](#windows-actions)
    -   [Linux Actions](#linux-actions)
-   [Supported Actions](#supported-actions)
-   [Creating Custom Actions](#creating-custom-actions)
-   [Troubleshooting](#troubleshooting)
-   [Help](#help)
-   [License](#license)
-   [Sponsoring](#sponsoring)

## Overview

CoPilot Action is a powerful feature within the SOCFortress CoPilot platform that enables security teams to execute automated response actions across their endpoint infrastructure. By leveraging Velociraptor's client-server architecture, CoPilot Action provides the ability to:

- 🛡️ **Block/Unblock IPs** on Windows firewalls
- 👥 **Enumerate users and groups** across systems
- 📊 **Collect system information** for incident response
- 🔍 **Execute custom scripts** for threat hunting
- ⚡ **Automate response actions** based on security events

## Prerequisites

Before using CoPilot Action, ensure you have the following components installed and configured:

### Required Components

1. **SOCFortress CoPilot** - The main CoPilot platform
   - Installation guide: [CoPilot Repository](https://github.com/socfortress/CoPilot)
   - Version: Latest stable release recommended

2. **Velociraptor Server** - For endpoint management and artifact execution
   - Download: [Velociraptor Releases](https://github.com/Velocidex/velociraptor/releases)
   - Minimum version: 0.74.1 or higher

3. **Velociraptor Clients** - Deployed on target endpoints
   - Windows clients for Windows-based actions
   - Linux clients for Linux-based actions

### Network Requirements

- Velociraptor clients must be able to communicate with the Velociraptor server
- Endpoints must have internet access to download scripts from GitHub repositories
- CoPilot must have API access to the Velociraptor server

## Installation

### Step 1: Upload Velociraptor Artifacts

Upload the required Velociraptor artifacts to your Velociraptor server:

1. **Download the artifacts** from this repository:
   - `velociraptor/Linux.Execute.RemoteBashScript.yaml`
   - `velociraptor/Windows.Execute.RemotePowerShellScript.yaml`

2. **Upload to Velociraptor**:
   - Access your Velociraptor web interface
   - Navigate to **View Artifacts** → **Upload Artifacts**
   - Upload both YAML files
   - Verify the artifacts are loaded successfully

### Step 2: Configure CoPilot Integration

Ensure your CoPilot instance is properly configured to communicate with Velociraptor:

1. **Velociraptor Configuration** in CoPilot:
   - Navigate to **Connectors** → **Velociraptor**
   - Configure your Velociraptor server URL and credentials
   - Test the connection to ensure proper communication

## Configuration

### Windows PowerShell Execution Policy

For Windows endpoints, ensure PowerShell execution policy allows script execution:

```powershell
# On target Windows machines (if needed)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
```

### Script Repository Configuration

By default, the artifacts are configured to use SOCFortress repositories:

- **Windows Scripts**: `https://raw.githubusercontent.com/socfortress/WINDOWS-FIREWALL-IP-UNBLOCK-ACTIVERESPONSE/main`
- **Linux Scripts**: `https://raw.githubusercontent.com/socfortress/Repo-to-hold-linux-activeresponse/main`

You can customize these repositories by modifying the `RepoURL` parameter when executing artifacts.

## Usage

### Windows Actions

Execute Windows-based actions using the `Windows.Execute.RemotePowerShellScript` artifact:

#### Example: Block an IP on Windows Firewall

```yaml
Artifact: Windows.Execute.RemotePowerShellScript
Parameters:
  RepoURL: https://raw.githubusercontent.com/socfortress/WINDOWS-FIREWALL-IP-BLOCK-ACTIVERESPONSE/main
  ScriptName: firewall-ip-block.ps1
  Arg1: "192.168.1.100"  # IP to block
  Arg2: "Threat_Block"   # Rule name
```

#### Example: Unblock an IP on Windows Firewall

```yaml
Artifact: Windows.Execute.RemotePowerShellScript
Parameters:
  RepoURL: https://raw.githubusercontent.com/socfortress/WINDOWS-FIREWALL-IP-UNBLOCK-ACTIVERESPONSE/main
  ScriptName: firewall-ip-unblock.ps1
  Arg1: "192.168.1.100"  # IP to unblock
```

### Linux Actions

Execute Linux-based actions using the `Linux.Execute.RemoteBashScript` artifact:

#### Example: Collect Suspicious Users and Groups

```yaml
Artifact: Linux.Execute.RemoteBashScript
Parameters:
  RepoURL: https://raw.githubusercontent.com/socfortress/Repo-to-hold-linux-activeresponse/main
  ScriptName: collect-suspicious-users-groups.sh
  Arg1: "audit"  # Operation type
```

#### Example: Block/Unblock IP on Linux

```yaml
Artifact: Linux.Execute.RemoteBashScript
Parameters:
  RepoURL: https://raw.githubusercontent.com/socfortress/Repo-to-hold-linux-activeresponse/main
  ScriptName: block-unblock-ip.sh
  Arg1: "block"          # Action (block/unblock)
  Arg2: "192.168.1.100"  # IP address
```

## Supported Actions

### Windows Actions
- **Firewall Management**: Block/unblock IP addresses
- **User Management**: List users, check privileges
- **Process Management**: Kill processes, list running services
- **System Information**: Collect system details, installed software
- **Registry Operations**: Query and modify registry keys
- **File Operations**: Search, collect, or delete files

### Linux Actions
- **Firewall Management**: IPTables rules for IP blocking/unblocking
- **User Management**: Enumerate users, groups, and privileges
- **Process Management**: Kill processes, service management
- **System Information**: System details, package information
- **Log Collection**: Gather system and security logs
- **File Operations**: Search, collect, or remove files

## Creating Custom Actions

You can create custom action scripts by following these guidelines:

### Windows PowerShell Scripts

```powershell
# Access arguments via predefined variables
param()

# Arguments are available as $Arg1, $Arg2, etc.
$TargetIP = $Arg1
$Action = $Arg2

# Your custom logic here
Write-Output "Executing action: $Action on IP: $TargetIP"

# Log to OSSEC active-responses.log
$LogPath = "C:\Program Files (x86)\ossec-agent\active-response\active-responses.log"
$LogEntry = "$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss') - Custom Action: $Action on $TargetIP"
Add-Content -Path $LogPath -Value $LogEntry
```

### Linux Bash Scripts

```bash
#!/bin/bash

# Arguments are available as environment variables ARG1, ARG2, etc.
TARGET_IP="$ARG1"
ACTION="$ARG2"

# Your custom logic here
echo "Executing action: $ACTION on IP: $TARGET_IP"

# Log to OSSEC active-responses.log
LOG_PATH="/var/ossec/active-response/active-responses.log"
echo "$(date '+%Y-%m-%d %H:%M:%S') - Custom Action: $ACTION on $TARGET_IP" >> "$LOG_PATH"
```

### Script Repository Structure

1. **Create a public GitHub repository** for your scripts
2. **Upload your scripts** to the repository
3. **Configure the artifact** with your repository URL
4. **Test the scripts** on target endpoints

## Troubleshooting

### Common Issues

1. **Script Download Failures**
   - Verify internet connectivity on target endpoints
   - Check repository URL and script name accuracy
   - Ensure scripts are publicly accessible

2. **PowerShell Execution Errors**
   - Check PowerShell execution policy on Windows endpoints
   - Verify script syntax and PowerShell compatibility

3. **Permission Denied Errors**
   - Ensure Velociraptor client runs with appropriate privileges
   - Check file system permissions for script execution

4. **Velociraptor Connection Issues**
   - Verify Velociraptor server accessibility
   - Check CoPilot-Velociraptor integration configuration
   - Review Velociraptor client logs

### Debug Mode

Enable verbose logging in artifacts by modifying the query to include debug output:

```yaml
# Add debug information to script execution
SELECT timestamp(epoch=now()),
       Stdout,
       Stderr,
       Status,
       script_url AS SourceScript,
       "DEBUG: " + Arg1 + ", " + Arg2 AS DebugInfo
```

## Help

You can reach us on [Discord](https://discord.gg/UN3pNBzaEQ) or by [📧](mailto:info@socfortress.co) if you have any question, issue or idea!

Check out our full video tutorial series on [![YouTube](https://img.shields.io/badge/YouTube-%23FF0000.svg?style=for-the-badge&logo=YouTube&logoColor=white)](https://www.youtube.com/watch?v=qQbex2zAhWI&list=PLB6hQ_WpB6U0e5oSLXJMcxmSzz7n3zvD-&ab_channel=TaylorWalton)

### Related Resources

- [SOCFortress CoPilot Documentation](https://github.com/socfortress/CoPilot)
- [Velociraptor Documentation](https://docs.velociraptor.app/)
- [CoPilot Action Video Tutorials](https://www.youtube.com/@taylorwalton_socfortress/videos)

## License

The contents of this repository is available under [AGPL-3.0 license](LICENSE).

## Sponsoring

If you like this project and want to support it, you can consider becoming a sponsor to help us continue maintaining it and adding new features.

[![GitHub Sponsors](https://img.shields.io/badge/sponsor-30363D?style=for-the-badge&logo=GitHub-Sponsors&logoColor=#EA4AAA)](https://github.com/sponsors/taylorwalton)

---

<div align="center">
<strong>🚀 Automate your security responses with CoPilot Action!</strong>
</div>