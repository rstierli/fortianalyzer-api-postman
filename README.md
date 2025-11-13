# FortiAnalyzer API Postman Collection

<p align="center">
  <img src="https://img.shields.io/badge/FortiAnalyzer-API-red?style=for-the-badge&logo=fortinet" alt="FortiAnalyzer API"/>
  <img src="https://img.shields.io/badge/Postman-Collection-orange?style=for-the-badge&logo=postman" alt="Postman Collection"/>
  <img src="https://img.shields.io/badge/Version-1.1-blue?style=for-the-badge" alt="Version"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License"/>
</p>

<p align="center">
  <strong>Postman collection for FortiAnalyzer JSON-RPC API</strong><br>
  100+ ready-to-use API requests for log management, reporting, device management, and security operations
</p>

---

## 🚀 Quick Start

### 1. Import Collection

**Option A: Direct Import to Postman**

Click this button to import directly into Postman:

[![Run in Postman](https://run.pstmn.io/button.svg)](https://god.gw.postman.com/run-collection/:collection_id)

**Option B: Manual Download**

```bash
# Download collection
curl -O https://raw.githubusercontent.com/rstierli/fortianalyzer-api-postman/main/collections/FortiAnalyzer_Master_Collection_V1.1.postman_collection.json

# Download environment template
curl -O https://raw.githubusercontent.com/rstierli/fortianalyzer-api-postman/main/environments/example.postman_environment.json
```

### 2. Configure Environment

1. Import `example.postman_environment.json` into Postman
2. Duplicate it and rename to your environment (e.g., "My FortiAnalyzer")
3. Update these variables:
   - `fqdn`: Your FortiAnalyzer hostname/IP
   - `user`: Your admin username
   - `password`: Your admin password
   - `faz-api-token`: Your API key (recommended)

### 3. Start Using

- **Session Auth**: Run "Login" → Use other requests → "Logout"
- **API Key Auth**: Set `faz-api-token` → Use any request directly

📖 **Full setup guide:** [SETUP.md](SETUP.md)

---

## 📚 What's Included

The collection includes **100+ API requests** organized by category:

<details>
<summary><b>🔐 Authentication & Session Management</b></summary>

- Login (Session-based)
- Logout
- API Key authentication examples
</details>

<details>
<summary><b>🔍 Log Management (LogView)</b></summary>

**Search Operations:**
- Search by IP address (source/destination)
- Search by attack signature
- Search by malware detection
- Search by application control
- Search by web filter
- Search by botnet detection
- Search by session ID
- Cancel search tasks
- Fetch search results

**Features:**
- Two-step async pattern support
- Advanced filter syntax
- Time range queries
- Pagination support
</details>

<details>
<summary><b>📊 Reports</b></summary>

- Generate reports from templates
- Schedule report generation
- Download generated reports
- Manage report folders
- Report layouts and charts
- Custom report filters
- Export/Import report templates
</details>

<details>
<summary><b>🖥️ Device Management</b></summary>

**ADOM Operations:**
- Create/Delete ADOMs
- Enable/Disable ADOM mode
- Clone ADOMs
- Get ADOM list with filters

**Device Operations:**
- Register devices
- Get device list (filtered/unfiltered)
- Add unregistered devices
- Device status monitoring
</details>

<details>
<summary><b>📈 FortiView Analytics</b></summary>

- **Top Sources** - Bandwidth top talkers
- **Top Threats** - Security threat analysis
- **Top Applications** - Application usage statistics (with policy filters)
- **SD-WAN Analytics**:
  - Interface bandwidth monitoring
  - Application usage over SD-WAN
  - Health overview
  - Top talkers
  - Audio MOS score
</details>

<details>
<summary><b>🚨 Security Operations</b></summary>

- **IOC Analysis** - Indicator of Compromise detection
- **Event Handlers** - Automated incident response
- **Automation Connectors** - Fabric connector setup
- **Alert Management** - IPS alerts, SD-WAN alerts
- **Subnet Management** - Subnet groups and objects
</details>

<details>
<summary><b>⚙️ System Operations</b></summary>

- System status monitoring
- Performance metrics
- Admin user management
- Certificate operations
- Fabric of FortiAnalyzer (distributed deployments)
- Log forwarding configuration
</details>

---

## 🤖 Smart Automation Features

This collection includes powerful **pre-request** and **post-response** scripts that automate repetitive tasks:

### ✅ Automatic Time Range Calculation
- Set `time_range_days` once (default: 30 days)
- Time ranges automatically calculated on every request
- Always uses current timestamps - no manual updates needed

### ✅ Automatic Variable Extraction
- **Session IDs** - Auto-extracted from login responses
- **Task IDs (TID)** - Auto-saved for async operations (LogView, Reports, FortiView)
- **Layout IDs** - Auto-extracted from report operations
- **PDF Data** - Auto-captured from report downloads

### ✅ Seamless Multi-Step Workflows
```
1. Create Search Task → TID automatically saved
2. Fetch Results → Uses {{taskID}} automatically
3. No manual copying needed!
```

📖 **Full details:** [SETUP.md - Collection Automation Features](SETUP.md#-collection-automation-features)

---

## 🔐 Authentication Methods

### Method 1: Session-Based (Username/Password)

**Best for:** Interactive testing, short-lived operations

```
1. Run "Login" request → Session ID auto-saved
2. Run any API request → Uses session automatically
3. Run "Logout" when done
```

### Method 2: API Key (Recommended)

**Best for:** Automation, CI/CD, long-running scripts

```
1. Generate API key in FortiAnalyzer (see SETUP.md)
2. Set faz-api-token in environment
3. Run any request → No login/logout needed
```

📖 **Full guide:** [How to Create API Keys](SETUP.md#create-api-user-on-fortianalyzer)

---

## 📋 Environment Variables

| Variable | Description | Required | Example |
|----------|-------------|----------|---------|
| `fqdn` | FortiAnalyzer hostname/IP | ✅ Yes | `faz.example.com` |
| `tcp` | HTTPS port | ✅ Yes | `443` |
| `user` | Admin username | 🔐 Session auth | `admin` |
| `password` | Admin password | 🔐 Session auth | `yourpassword` |
| `faz-api-token` | API key (Bearer token) | 🔑 API key auth | `abc123...` |
| `adom` | ADOM name | ✅ Yes | `root` |
| `session` | Session ID | 🔄 Auto | (auto-filled) |
| `taskID` | Task ID for async ops | 🔄 Auto | (auto-filled) |

📖 **Complete list:** [SETUP.md - Environment Variables](SETUP.md#environment-variables-explained)

---

## 🎯 Usage Examples

### Example 1: Search Logs by IP Address

```
1. Authenticate (login or API key)
2. Open: LogView → "Create Search Task for IP Dst"
3. Edit the filter field with your IP address
4. Click Send → taskID automatically saved to environment
5. Open: LogView → "Fetch Log Search Result by Task ID"
6. Click Send → Uses {{taskID}} automatically
7. View results
```

**Note:** The collection automatically extracts and saves the Task ID (TID) from responses, so no manual copying is needed!

### Example 2: Generate Security Report

```
1. Authenticate
2. Open: Reports → "Run Report"
3. Click Send → taskID and time ranges handled automatically
4. Wait 30-60 seconds for report generation
5. Open: Reports → "Download Report"
6. Click Send → Uses saved taskID automatically
```

**Note:** Time ranges are automatically calculated based on `time_range_days` environment variable (default: 30 days).

### Example 3: FortiView Top Threats

```
1. Authenticate
2. Open: FortiView Top Threats → "Create Task"
3. Click Send → taskID auto-saved
4. Open: FortiView Top Threats → "Fetch Result by Task"
5. Click Send → Uses {{taskID}} automatically to get threat statistics
```

---

## 🛠️ Prerequisites

- **FortiAnalyzer** v7.4.0+ (tested on v7.4.8, v7.6.4, v8.0.0)
- **Postman** Desktop or Postman CLI (Newman)
- **Network Access** to FortiAnalyzer via HTTPS
- **Admin privileges** or dedicated API user account

---

## 📖 Documentation

- **Setup Guide**: [SETUP.md](SETUP.md) - Complete installation and configuration
- **API Documentation**: [FortiAnalyzer API Docs](https://docs.fortinet.com/document/fortianalyzer/latest/json-rpc-api-reference/)
- **Full Documentation**: [How to FortiAnalyzer API](https://your-readthedocs-url.io) *(if published)*

---

## 🔒 Security Best Practices

✅ **Use API Keys** for automation (no timeout issues)
✅ **Rotate credentials** regularly
✅ **Use dedicated API users** (don't use admin)
✅ **Store secrets securely** (Postman Vault, environment variables)
✅ **Limit API user permissions** (custom profiles)
❌ **Never commit** environment files with real credentials
✅ **Monitor API usage** in FortiAnalyzer audit logs
✅ **Use HTTPS** always (verify certificates in production)

---

## 🤖 CI/CD Integration (Newman)

Run collections in CI/CD pipelines using Newman:

```bash
# Install Newman
npm install -g newman

# Run collection with environment
newman run collections/FortiAnalyzer_Master_Collection_V1.1.postman_collection.json \
  --environment environments/my-faz.postman_environment.json \
  --reporters cli,json

# Run specific folder
newman run collections/FortiAnalyzer_Master_Collection_V1.1.postman_collection.json \
  --folder "LogView" \
  --environment environments/my-faz.postman_environment.json
```

**GitHub Actions Example:**

See [.github/workflows/test-collection.yml](.github/workflows/test-collection.yml) for CI/CD integration example.

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/new-endpoint`
3. Add your changes
4. Test with your FortiAnalyzer
5. Commit: `git commit -m "Add new endpoint: XYZ"`
6. Push: `git push origin feature/new-endpoint`
7. Open a Pull Request

**Guidelines:**
- Use environment variables for all dynamic values
- Follow existing request naming conventions
- Add descriptions to new requests
- Test against FortiAnalyzer 7.4+

---

## 🐛 Issues & Support

- 📧 **Report Issues**: [GitHub Issues](https://github.com/rstierli/fortianalyzer-api-postman/issues)
- 💬 **Discussions**: [GitHub Discussions](https://github.com/rstierli/fortianalyzer-api-postman/discussions)
- 📚 **Documentation**: [SETUP.md](SETUP.md)

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🏷️ Version History

- **v1.1** (Current) - November 2025
  - 100+ API endpoints
  - Session and API key authentication
  - Complete LogView, Reports, FortiView, Device Management
  - SD-WAN analytics
  - IOC analysis and security operations

- **v1.0** - Initial release

---

## 🌟 Related Projects

- **Fortinet Docs** - [https://docs.fortinet.com](https://docs.fortinet.com)
- **Fortinet API Docs** - [https://fndn.fortinet.com](https://fndn.fortinet.com)
- **FortiAnalyzer IPS PCAP Downloader** - [https://github.com/rstierli/fortianalyzer-pcap-downloader](https://github.com/rstierli/fortianalyzer-pcap-downloader)

---

## 👏 Acknowledgments

Created with ❤️ by the Fortinet Community

Special thanks to all contributors and the FortiAnalyzer development team.

---

<p align="center">
  <sub>Built with Postman | Powered by FortiAnalyzer | Secured by Fortinet</sub>
</p>
