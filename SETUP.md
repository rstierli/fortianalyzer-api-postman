# FortiAnalyzer Postman Collection - Setup Guide

Complete guide to setting up and using the FortiAnalyzer API Postman collection.

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Import Collection](#import-collection)
- [Setup Environment](#setup-environment)
- [Authentication Methods](#authentication-methods)
  - [Method 1: Session-Based (Username/Password)](#method-1-session-based-usernamepassword)
  - [Method 2: API Key (Recommended)](#method-2-api-key-recommended)
- [Create API User on FortiAnalyzer](#create-api-user-on-fortianalyzer)
- [Environment Variables Explained](#environment-variables-explained)
- [Testing Your Setup](#testing-your-setup)
- [Troubleshooting](#troubleshooting)

---

## Prerequisites

Before you begin, ensure you have:

1. ✅ **FortiAnalyzer Access**
   - FortiAnalyzer 7.4.0 or higher
   - HTTPS access (port 443)
   - Administrative privileges

2. ✅ **Postman Installed**
   - [Download Postman Desktop](https://www.postman.com/downloads/)
   - Or install Postman CLI: `npm install -g newman`

3. ✅ **Network Connectivity**
   - HTTPS access to FortiAnalyzer
   - Firewall rules allowing connections

---

## Import Collection

### Option 1: Direct Import

1. Open Postman Desktop
2. Click **Import** button (top-left)
3. Select **Link** tab
4. Paste: `https://raw.githubusercontent.com/your-org/FAZ_howto_API/main/postman/collections/FortiAnalyzer_Master_Collection_V1.1.postman_collection.json`
5. Click **Continue** → **Import**

### Option 2: Manual Download

```bash
# Download collection
curl -O https://raw.githubusercontent.com/your-org/FAZ_howto_API/main/postman/collections/FortiAnalyzer_Master_Collection_V1.1.postman_collection.json

# Download environment template
curl -O https://raw.githubusercontent.com/your-org/FAZ_howto_API/main/postman/environments/example.postman_environment.json

# Import in Postman
# File → Import → Select files
```

---

## Setup Environment

### Step 1: Duplicate Example Environment

1. Import `example.postman_environment.json`
2. In Postman, go to **Environments** (left sidebar)
3. Click the example environment
4. Click **...** → **Duplicate**
5. Rename to: `My FortiAnalyzer`

### Step 2: Configure Variables

Click **Edit** on your new environment and update these values:

| Variable | What to Enter | Example |
|----------|---------------|---------|
| `fqdn` | Your FortiAnalyzer hostname or IP | `faz-prod.example.com` or `192.168.1.100` |
| `tcp` | HTTPS port (usually 443) | `443` |
| `user` | Admin username | `admin` |
| `password` | Admin password | `YourStrongPassword!` |
| `adom` | ADOM name | `root` (default) or your ADOM name |

**Leave these blank (auto-populated):**
- `session` - Filled automatically after login
- `taskID` - Filled automatically by async operations
- `access_token` - Only fill if using API key auth

### Step 3: Select Environment

In Postman top-right corner, select your environment from the dropdown.

---

## Authentication Methods

FortiAnalyzer supports two authentication methods. Choose the one that fits your use case.

### Method 1: Session-Based (Username/Password)

**Best for:** Interactive testing, short-lived sessions

**Workflow:**
```
1. Run "Login" request → Get session ID
2. Run API requests (auto-use session ID)
3. Run "Logout" request when done
```

**Setup:** Already configured if you set `user` and `password` in environment.

---

### Method 2: API Key (Recommended)

**Best for:** Automation, scripts, CI/CD pipelines, long-running operations

**Benefits:**
- ✅ No session timeout issues
- ✅ No need to login/logout
- ✅ More secure for automation
- ✅ Easy credential rotation

---

## Create API User on FortiAnalyzer

Follow these steps to create a dedicated API user with an API key.

### Step 1: Access FortiAnalyzer GUI

1. Open browser: `https://your-faz-ip`
2. Login with admin credentials

### Step 2: Navigate to Admin Users

```
System Settings → Administrators → Create New
```

Or via CLI:
```bash
ssh admin@your-faz-ip
```

### Step 3: Create API User (GUI Method)

1. **Click "Create New"** in the Administrators section

2. **Configure User Settings:**
   ```
   Username:           api_user
   Type:              Local User
   Password:          <strong-password>
   Confirm Password:  <strong-password>

   Administrator Profile: Super_User
   (or create custom profile with needed permissions)

   ✅ Enable: REST API Access
   ✅ Enable: JSON API Access
   ```

3. **Generate API Key:**
   - Under **REST API** section
   - Click **"Generate New Key"**
   - ⚠️ **IMPORTANT:** Copy the API key immediately - it won't be shown again!

   Example API key format:
   ```
   jfhd83hfksjdhf93hf8sdhf983hf9s8dhf9s8dhf98sdhf98sdhf98sdhf9
   ```

4. **Set Permissions:**
   - Select **Profile**: `Super_User` (full access)
   - Or create custom profile:
     ```
     Read/Write Access to:
     - Log & Report
     - Device Manager
     - System Settings
     ```

5. **Click "OK"** to save

### Step 4: Create API User (CLI Method)

```bash
# SSH to FortiAnalyzer
ssh admin@your-faz-ip

# Create API user
config system admin user
    edit "api_user"
        set profileid "Super_User"
        set password "YourStrongPassword123!"
        set rpc-permit read-write
    next
end

# Generate API key (FortiAnalyzer 7.0+)
execute api-user generate-key api_user
```

**Output will show the API key:**
```
API Key: jfhd83hfksjdhf93hf8sdhf983hf9s8dhf9s8dhf98sdhf98sdhf98sdhf9
```

⚠️ **Save this key securely!** You cannot retrieve it again.

### Step 5: Add API Key to Postman Environment

1. Go to **Environments** in Postman
2. Edit your environment
3. Set `access_token` to your API key:
   ```
   access_token = jfhd83hfksjdhf93hf8sdhf983hf9s8dhf9s8dhf98sdhf98sdhf98sdhf9
   ```
4. **Save** the environment

### Step 6: Test API Key

In the collection, find any request under **"Login and Logout"** → Try the **"Get System Status"** request:

1. Ensure your environment is selected
2. The request will automatically use the `access_token`
3. Click **Send**
4. ✅ If successful, you'll get system info back

---

## Environment Variables Explained

### Core Connection Variables

| Variable | Purpose | Where It's Used | Auto-Populated? |
|----------|---------|-----------------|-----------------|
| `fqdn` | FortiAnalyzer hostname/IP | All requests URL | ❌ Manual |
| `tcp` | HTTPS port | All requests URL | ❌ Manual |
| `user` | Admin username | Login request | ❌ Manual |
| `password` | Admin password | Login request | ❌ Manual |
| `access_token` | API key | Authorization header | ❌ Manual |

### Session & Authentication

| Variable | Purpose | Auto-Populated? |
|----------|---------|-----------------|
| `session` | Session ID from login | ✅ Yes (from login response) |
| `access_token` | API key (alternative to session) | ❌ Manual |

### Operational Variables

| Variable | Purpose | Auto-Populated? |
|----------|---------|-----------------|
| `adom` | Administrative Domain | ❌ Manual (default: `root`) |
| `taskID` | Task ID for async operations | ✅ Yes (from task creation) |
| `layoutID` | Report layout ID | ✅ Yes (from report operations) |
| `devname` | Device name | ❌ Manual (as needed) |

### Time Range Variables

| Variable | Purpose | Default | Auto-Populated? |
|----------|---------|---------|-----------------|
| `time_range_days` | Default query period | `30` | ❌ Manual |
| `time_range_start` | Query start timestamp | - | 🔄 Calculated |
| `time_range_end` | Query end timestamp | - | 🔄 Calculated |

### Pre/Post Scripts

**Current Status:** The collection has **minimal pre/post scripts**.

Most variables are currently:
- ✅ Pre-configured in requests
- 📋 Manually copied from responses (e.g., copy `session` from login response)
- 🔄 Auto-set only for session ID after login

**Future Enhancement:** Scripts could auto-extract:
- Task IDs from async operations
- Report IDs from report generation
- Time ranges from calculations

---

## Testing Your Setup

### Test 1: Session-Based Auth

1. **Select your environment** (top-right dropdown)
2. **Open:** `Login and Logout` → `Login`
3. **Click Send**
4. **Verify Response:**
   ```json
   {
     "result": [{
       "status": {"code": 0, "message": "OK"},
       "url": "sys/login/user"
     }],
     "session": "abc123sessionid..."
   }
   ```
5. **Check Environment:** The `session` variable should now have a value

### Test 2: API Key Auth

1. **Set `access_token`** in environment
2. **Open:** `System Settings` → `Get System Status`
3. **Click Send**
4. **Verify Response:** Should return FortiAnalyzer info without login

### Test 3: LogView Search (Async Operation)

1. **Authenticate** (either method)
2. **Open:** `LogView` → `Create Search Task for IP Dst`
3. **Edit request** - set a valid IP in the filter
4. **Click Send**
5. **Copy `tid`** from response
6. **Open:** `LogView` → `Fetch Log Search Result by Task ID`
7. **Paste `tid`** into URL (replace `{{taskID}}`)
8. **Click Send** - should return log results

---

## Troubleshooting

### Issue: "Session Timeout" or "Invalid Session"

**Cause:** Session expired (default 300 seconds)

**Solution:**
1. Run the **Login** request again
2. Or switch to API key authentication (no timeout)

---

### Issue: "No permission for the resource" (Error -11)

**Cause:** API user lacks required permissions

**Solution:**
```bash
# Grant Super_User profile
config system admin user
    edit "api_user"
        set profileid "Super_User"
    next
end
```

---

### Issue: "Connection Refused" or "Timeout"

**Cause:** Network/firewall blocking connection

**Solution:**
1. Verify FortiAnalyzer is accessible: `ping your-faz-ip`
2. Test HTTPS: `curl -k https://your-faz-ip`
3. Check firewall rules
4. Verify HTTPS is enabled on FortiAnalyzer

---

### Issue: Variables Not Populating

**Cause:** No pre/post scripts in collection

**Current Workaround:**
1. Run the request
2. Manually copy values from response
3. Paste into environment variables

**Example:** After login, copy `session` from response and paste into `session` variable.

---

### Issue: "SSL Certificate Error"

**Cause:** Self-signed certificate on FortiAnalyzer

**Solution:**
1. In Postman: **Settings** → **General**
2. Disable **"SSL certificate verification"**
3. (For production, use valid certificates)

---

## Security Best Practices

1. ✅ **Use API Keys** for automation
2. ✅ **Rotate credentials** regularly
3. ✅ **Use dedicated API users** (don't use admin account)
4. ✅ **Store credentials securely** (use Postman Vault)
5. ✅ **Limit permissions** (create custom profiles)
6. ❌ **Never commit** environment files with real credentials to git
7. ✅ **Use HTTPS** always
8. ✅ **Monitor API usage** in FortiAnalyzer logs

---

## Next Steps

- ✅ **Explore Requests:** Browse the collection folders
- 📚 **Read API Docs:** [Full Documentation](https://your-docs-url.readthedocs.io)
- 🧪 **Test Operations:** Try LogView searches, report generation
- 🤖 **Automate:** Use Newman CLI for CI/CD integration
- 🤝 **Contribute:** Submit improvements via GitHub

---

## Additional Resources

- **Full API Documentation:** [https://your-docs-url.readthedocs.io](https://your-docs-url.readthedocs.io)
- **Postman Documentation:** [https://learning.postman.com](https://learning.postman.com)
- **FortiAnalyzer Admin Guide:** [Fortinet Docs](https://docs.fortinet.com)

---

**Need Help?**
- 📧 Open an issue: [GitHub Issues](https://github.com/your-org/FAZ_howto_API/issues)
- 💬 Join discussion: [GitHub Discussions](https://github.com/your-org/FAZ_howto_API/discussions)
