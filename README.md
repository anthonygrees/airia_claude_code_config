# Airia Code Configuration Guide

## Overview
Airia Code is a gateway-powered version of Claude Code that routes requests through an organization's Airia Gateway for enhanced security, monitoring, and control.

---

## Prerequisites

### All Platforms
- **Claude CLI** must be installed
- **Airia Gateway API Key** (format: `ak-*`)
- **Network access** to your chosen Airia Gateway endpoint

---

## Configuration Settings Required

### 1. Environment Variable: ANTHROPIC_BASE_URL

This variable tells Claude CLI where to route API requests.

**Available Gateway Endpoints:**
- **Development:** `https://dev.gateway.airiadev.ai/anthropic/`
- **Production North America:** `https://prod-na.gateway.airiadev.ai/anthropic/`
- **Production Australia:** `https://prodaus.gateway.airia.ai/anthropic/`
- **Production Europe 1:** `https://prod-eu1.gateway.airiadev.ai/anthropic/`
- **Production MENA:** `https://prod-mena.gateway.airiadev.ai/anthropic/`
- **Custom:** Your organization's custom gateway URL

### 2. API Key Storage

The API key must be stored securely and accessible to Claude CLI via the settings file.

**Settings File Location:**
- `~/.claude/settings.json` (Mac/Linux)
- `%USERPROFILE%\.claude\settings.json` (Windows)

---

## Mac/Linux Configuration

### Step 1: Install Claude CLI

**Option A: Using npm (Recommended)**
```bash
npm install -g @anthropic-ai/claude-code
```

**Option B: Using curl**
```bash
curl -fsSL https://claude.ai/cli/install.sh | bash
```

**Option C: Manual download**
Visit: https://github.com/anthropics/claude-code/releases

**Verify Installation:**
```bash
claude --version
```

### Step 2: Set ANTHROPIC_BASE_URL Environment Variable

**For Current Session Only:**
```bash
export ANTHROPIC_BASE_URL="https://prodaus.gateway.airia.ai"
```

**For Persistent Configuration:**

Determine your shell:
```bash
echo $SHELL
```

**For Zsh users (~/.zshrc):**
```bash
echo 'export ANTHROPIC_BASE_URL="https://prodaus.gateway.airia.ai"' >> ~/.zshrc
source ~/.zshrc
```

**For Bash users (~/.bashrc):**
```bash
echo 'export ANTHROPIC_BASE_URL="https://prodaus.gateway.airia.ai"' >> ~/.bashrc
source ~/.bashrc
```

**For other shells (~/.profile):**
```bash
echo 'export ANTHROPIC_BASE_URL="https://prodaus.gateway.airia.ai"' >> ~/.profile
source ~/.profile
```

### Step 3: Configure API Key Storage

**macOS - Using Keychain (Most Secure):**

1. Store API key in macOS Keychain:
```bash
security add-generic-password -s "airia-api-key" -a "$(whoami)" -w "YOUR_API_KEY_HERE" -U
```

2. Create Claude settings directory:
```bash
mkdir -p ~/.claude
```

3. Create settings file:
```bash
cat > ~/.claude/settings.json <<'EOF'
{
  "apiKeyHelper": "security find-generic-password -s airia-api-key -w"
}
EOF
```

**Linux - Using Environment Variable:**

1. Create Claude settings directory:
```bash
mkdir -p ~/.claude
```

2. Add API key to your shell profile (replace with your shell's config file):
```bash
echo 'export ANTHROPIC_API_KEY="YOUR_API_KEY_HERE"' >> ~/.bashrc
source ~/.bashrc
```

3. Create settings file to reference environment variable:
```bash
cat > ~/.claude/settings.json <<'EOF'
{
  "apiKeyHelper": "echo $ANTHROPIC_API_KEY"
}
EOF
```

**Alternative Linux Method - Direct storage (Less Secure):**
```bash
mkdir -p ~/.claude
cat > ~/.claude/settings.json <<EOF
{
  "apiKeyHelper": "echo YOUR_API_KEY_HERE"
}
EOF
```

**Set proper permissions:**
```bash
chmod 600 ~/.claude/settings.json
```

### Step 4: Verify Configuration

```bash
# Check environment variable
echo $ANTHROPIC_BASE_URL

# Check settings file exists
cat ~/.claude/settings.json

# Test API key retrieval (macOS)
security find-generic-password -s airia-api-key -w

# Test API key retrieval (Linux with env var)
echo $ANTHROPIC_API_KEY
```

### Step 5: Launch Claude Code

```bash
cd /path/to/your/project
claude
```

---

## Windows Configuration

### Step 1: Install Claude CLI

**Option A: Using npm (Recommended)**
```powershell
npm install -g @anthropic-ai/claude-code
```

**Option B: Manual download**
1. Visit: https://github.com/anthropics/claude-code/releases
2. Download the Windows executable
3. Add to your PATH environment variable

**Verify Installation:**
```powershell
claude --version
```

### Step 2: Set ANTHROPIC_BASE_URL Environment Variable

**For Current PowerShell Session Only:**
```powershell
$env:ANTHROPIC_BASE_URL = "https://prodaus.gateway.airia.ai"
```

**For Current Command Prompt Session Only:**
```cmd
set ANTHROPIC_BASE_URL=https://prodaus.gateway.airia.ai
```

**For Persistent Configuration (User Level):**

**Using PowerShell:**
```powershell
[System.Environment]::SetEnvironmentVariable('ANTHROPIC_BASE_URL', 'https://prodaus.gateway.airia.ai', 'User')
```

**Using Command Prompt (Run as Administrator):**
```cmd
setx ANTHROPIC_BASE_URL "https://prodaus.gateway.airia.ai"
```

**Using GUI:**
1. Right-click "This PC" → Properties
2. Click "Advanced system settings"
3. Click "Environment Variables"
4. Under "User variables", click "New"
5. Variable name: `ANTHROPIC_BASE_URL`
6. Variable value: `https://prodaus.gateway.airia.ai`
7. Click OK

### Step 3: Configure API Key Storage

**Create Claude settings directory:**
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude"
```

**Method A: Using Environment Variable (Recommended)**

1. Set API key as environment variable:
```powershell
[System.Environment]::SetEnvironmentVariable('ANTHROPIC_API_KEY', 'YOUR_API_KEY_HERE', 'User')
```

2. Create settings file to reference it:
```powershell
@"
{
  "apiKeyHelper": "powershell.exe -Command `$env:ANTHROPIC_API_KEY"
}
"@ | Out-File -FilePath "$env:USERPROFILE\.claude\settings.json" -Encoding UTF8
```

**Method B: Using Windows Credential Manager (More Secure)**

1. Store API key in Credential Manager:
```powershell
cmdkey /generic:airia-api-key /user:airia /pass:YOUR_API_KEY_HERE
```

2. Create settings file:
```powershell
@"
{
  "apiKeyHelper": "powershell.exe -Command \"(cmdkey /list:airia-api-key | Select-String 'Password').ToString().Split(':')[1].Trim()\""
}
"@ | Out-File -FilePath "$env:USERPROFILE\.claude\settings.json" -Encoding UTF8
```

**Method C: Direct Storage (Least Secure)**
```powershell
@"
{
  "apiKeyHelper": "echo YOUR_API_KEY_HERE"
}
"@ | Out-File -FilePath "$env:USERPROFILE\.claude\settings.json" -Encoding UTF8
```

**Set file permissions to restrict access:**
```powershell
$acl = Get-Acl "$env:USERPROFILE\.claude\settings.json"
$acl.SetAccessRuleProtection($true, $false)
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($env:USERNAME, "FullControl", "Allow")
$acl.SetAccessRule($accessRule)
Set-Acl "$env:USERPROFILE\.claude\settings.json" $acl
```

### Step 4: Verify Configuration

**PowerShell:**
```powershell
# Check environment variable
$env:ANTHROPIC_BASE_URL

# Check settings file exists
Get-Content "$env:USERPROFILE\.claude\settings.json"

# Test API key retrieval (environment variable method)
$env:ANTHROPIC_API_KEY

# Test API key retrieval (credential manager method)
cmdkey /list:airia-api-key
```

**Command Prompt:**
```cmd
# Check environment variable
echo %ANTHROPIC_BASE_URL%

# Check settings file exists
type "%USERPROFILE%\.claude\settings.json"

# Test API key retrieval
echo %ANTHROPIC_API_KEY%
```

### Step 5: Launch Claude Code

**PowerShell:**
```powershell
cd C:\path\to\your\project
claude
```

**Command Prompt:**
```cmd
cd C:\path\to\your\project
claude
```

---

## Configuration Summary Table

| Component | Mac/Linux | Windows |
|-----------|-----------|---------|
| **Settings Directory** | `~/.claude/` | `%USERPROFILE%\.claude\` |
| **Settings File** | `~/.claude/settings.json` | `%USERPROFILE%\.claude\settings.json` |
| **Shell Profile** | `~/.zshrc`, `~/.bashrc`, or `~/.profile` | User Environment Variables |
| **Secure Storage** | macOS Keychain (Mac) / Environment Variables (Linux) | Windows Credential Manager or Environment Variables |
| **Set Env Variable** | `export VAR=value` in shell profile | `setx VAR "value"` or System Properties GUI |

---

## Testing Your Configuration

### Test Gateway Connection

**Mac/Linux:**
```bash
curl -s --max-time 10 "$ANTHROPIC_BASE_URL"
```

**Windows (PowerShell):**
```powershell
Invoke-WebRequest -Uri $env:ANTHROPIC_BASE_URL -TimeoutSec 10 -UseBasicParsing
```

**Windows (Command Prompt with curl):**
```cmd
curl -s --max-time 10 %ANTHROPIC_BASE_URL%
```

### Test Claude CLI

```bash
claude --version
```

### Test Complete Setup

Launch Claude in any project directory:
```bash
cd /path/to/test/project
claude
```

---

## Troubleshooting

### Common Issues

**1. Claude command not found**
- Verify installation: `claude --version`
- Check PATH includes npm global bin directory
- Restart terminal/PowerShell after installation

**2. Environment variable not set**
- Mac/Linux: Run `echo $ANTHROPIC_BASE_URL`
- Windows: Run `echo %ANTHROPIC_BASE_URL%` (CMD) or `$env:ANTHROPIC_BASE_URL` (PowerShell)
- Ensure you restarted your terminal/PowerShell after setting
- Source your profile file: `source ~/.zshrc` or `source ~/.bashrc`

**3. API key not working**
- Verify key format starts with `ak-` or `agk-`
- Check settings file exists and is readable
- Test apiKeyHelper command manually
- Ensure no extra spaces or quotes in API key

**4. Gateway connection fails**
- Test gateway URL is reachable
- Verify firewall/proxy settings
- Check correct environment URL is selected
- Ensure URL ends with `/anthropic/`

**5. Permission denied on settings file (Linux)**
```bash
chmod 600 ~/.claude/settings.json
```

**6. Permission denied on settings file (Windows)**
```powershell
icacls "$env:USERPROFILE\.claude\settings.json" /inheritance:r /grant:r "$env:USERNAME:(F)"
```

---

## Cleanup/Uninstall

### Mac/Linux

**Remove environment variable:**
```bash
# Edit your shell profile and remove the export line
nano ~/.zshrc  # or ~/.bashrc or ~/.profile

# Unset from current session
unset ANTHROPIC_BASE_URL
```

**Remove API key from macOS Keychain:**
```bash
security delete-generic-password -s "airia-api-key" -a "$(whoami)"
```

**Remove settings:**
```bash
rm -rf ~/.claude
```

### Windows

**Remove environment variable:**

**PowerShell:**
```powershell
[System.Environment]::SetEnvironmentVariable('ANTHROPIC_BASE_URL', $null, 'User')
[System.Environment]::SetEnvironmentVariable('ANTHROPIC_API_KEY', $null, 'User')
```

**GUI:**
1. System Properties → Environment Variables
2. Delete `ANTHROPIC_BASE_URL` and `ANTHROPIC_API_KEY` variables

**Remove API key from Credential Manager:**
```powershell
cmdkey /delete:airia-api-key
```

**Remove settings:**
```powershell
Remove-Item -Recurse -Force "$env:USERPROFILE\.claude"
```

---

## Security Best Practices

1. **Never commit API keys to version control**
2. **Use secure storage methods:**
   - macOS: Keychain
   - Linux: Environment variables with restricted shell profile permissions
   - Windows: Credential Manager or User environment variables
3. **Set restrictive file permissions on settings.json:**
   - Mac/Linux: `chmod 600 ~/.claude/settings.json`
   - Windows: Use ACLs to restrict to current user only
4. **Rotate API keys regularly**
5. **Use production gateway URLs for production work**
6. **Test configuration with development gateway first**

---

## Quick Start Command Reference

### Mac/Linux One-Liner Setup
```bash
# Set gateway URL
export ANTHROPIC_BASE_URL="https://prodaus.gateway.airia.ai"
echo 'export ANTHROPIC_BASE_URL="https://prodaus.gateway.airia.ai"' >> ~/.zshrc

# Create settings (replace YOUR_KEY)
mkdir -p ~/.claude
echo '{"apiKeyHelper": "echo YOUR_API_KEY_HERE"}' > ~/.claude/settings.json
chmod 600 ~/.claude/settings.json
```

### Windows One-Liner Setup (PowerShell)
```powershell
# Set gateway URL
[System.Environment]::SetEnvironmentVariable('ANTHROPIC_BASE_URL', 'https://prodaus.gateway.airia.ai', 'User')

# Set API key (replace YOUR_KEY)
[System.Environment]::SetEnvironmentVariable('ANTHROPIC_API_KEY', 'YOUR_API_KEY_HERE', 'User')

# Create settings
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude"
'{"apiKeyHelper": "powershell.exe -Command $env:ANTHROPIC_API_KEY"}' | Out-File -FilePath "$env:USERPROFILE\.claude\settings.json" -Encoding UTF8
```

---

## Support

For issues or questions:
- Check Claude CLI installation: `claude --version`
- Verify configuration: Review files and environment variables above
- Test gateway connection using curl/Invoke-WebRequest
- Contact your Airia Gateway administrator for API key or gateway URL issues

---

## File Locations Reference

### Mac/Linux
| File | Location |
|------|----------|
| Settings | `~/.claude/settings.json` |
| Zsh Profile | `~/.zshrc` |
| Bash Profile | `~/.bashrc` |
| Generic Profile | `~/.profile` |
| Keychain | System Keychain (accessible via `security` command) |

### Windows
| File | Location |
|------|----------|
| Settings | `%USERPROFILE%\.claude\settings.json` |
| Environment Variables | User Environment Variables (System Properties) |
| Credential Manager | Control Panel → Credential Manager → Windows Credentials |

---

## Version Information

This guide is based on the Airia Code Manager script for configuring Claude CLI with Airia Gateway integration.

**Supported Platforms:**
- macOS (Darwin)
- Linux (various distributions)
- Windows 10/11 (PowerShell 5.1+, Command Prompt)

**Required Tools:**
- Claude CLI
- curl (for connection testing)
- npm (optional, for installation)
