# Contributing to FortiAnalyzer API Postman Collection

Thank you for your interest in contributing! This document provides guidelines for contributing to this project.

## 🎯 Ways to Contribute

- 🐛 Report bugs and issues
- 💡 Suggest new API endpoints to add
- 📝 Improve documentation
- ✨ Add new requests to the collection
- 🔧 Fix bugs or improve existing requests
- 📖 Write tutorials or guides

## 🚀 Getting Started

1. **Fork the repository**
   ```bash
   # Click "Fork" button on GitHub
   ```

2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR_USERNAME/fortianalyzer-api-postman.git
   cd fortianalyzer-api-postman
   ```

3. **Create a branch**
   ```bash
   git checkout -b feature/my-new-endpoint
   ```

## 📝 Contribution Guidelines

### Adding New Requests

When adding new API requests to the collection:

1. **Use Environment Variables**
   - Never hardcode credentials, IPs, or sensitive data
   - Use `{{variable_name}}` for all dynamic values
   - Document new variables in SETUP.md

2. **Follow Naming Convention**
   - Clear, descriptive names: `Get System Status`, `Create Log Search Task`
   - Use title case
   - Group related requests in folders

3. **Add Descriptions**
   - Provide clear description of what the request does
   - Include example use cases
   - Document required parameters

4. **Example Request Structure**
   ```json
   {
     "name": "Get Device List",
     "request": {
       "method": "POST",
       "header": [...],
       "body": {
         "mode": "raw",
         "raw": "{\n  \"method\": \"get\",\n  \"params\": [...]\n}"
       },
       "url": {
         "raw": "https://{{fqdn}}:{{tcp}}/jsonrpc",
         ...
       },
       "description": "Retrieves list of managed devices from FortiAnalyzer..."
     }
   }
   ```

5. **Test Your Changes**
   - Test against FortiAnalyzer 7.4+
   - Verify with both session and API key auth
   - Ensure no credentials are hardcoded

### Updating Documentation

When updating documentation:

1. **Keep it Clear**
   - Use simple, straightforward language
   - Provide examples where helpful
   - Use screenshots sparingly (prefer code examples)

2. **Update SETUP.md** if you:
   - Add new environment variables
   - Change authentication flow
   - Add new prerequisites

3. **Update README.md** if you:
   - Add new API categories
   - Change quick start steps
   - Update version information

### Code Style

- Use consistent JSON formatting
- Use 2-space indentation
- Keep requests organized in logical folders
- Use meaningful variable names

## 🔍 Testing

Before submitting:

1. **Import your collection** into Postman
2. **Test with FortiAnalyzer** (v7.4+ recommended)
3. **Verify all requests work** with example environment
4. **Check for hardcoded values**
5. **Test both auth methods** (session and API key)

## 📤 Submitting Changes

1. **Commit your changes**
   ```bash
   git add .
   git commit -m "Add new endpoint: Get Fabric Connectors"
   ```

2. **Push to your fork**
   ```bash
   git push origin feature/my-new-endpoint
   ```

3. **Create Pull Request**
   - Go to GitHub and click "New Pull Request"
   - Provide clear description of changes
   - Reference any related issues
   - Include testing details

### Pull Request Template

```markdown
## Description
Brief description of what this PR does

## Type of Change
- [ ] New API request(s)
- [ ] Bug fix
- [ ] Documentation update
- [ ] Enhancement

## Testing
- [ ] Tested on FortiAnalyzer v7.4.8
- [ ] Tested on FortiAnalyzer v7.6.4
- [ ] Tested with session auth
- [ ] Tested with API key auth
- [ ] No hardcoded credentials

## Checklist
- [ ] Uses environment variables
- [ ] Added/updated documentation
- [ ] Follows naming conventions
- [ ] Tested successfully
```

## 🐛 Reporting Issues

When reporting issues:

1. **Use GitHub Issues**
2. **Provide Details**:
   - FortiAnalyzer version
   - Postman version
   - Steps to reproduce
   - Expected vs actual behavior
   - Error messages (redact sensitive info)

3. **Use Issue Template**:
   ```markdown
   **Description**
   Clear description of the issue

   **Environment**
   - FortiAnalyzer version: 7.4.8
   - Postman version: 11.0.0
   - Auth method: API Key

   **Steps to Reproduce**
   1. Import collection
   2. Run "XYZ" request
   3. See error

   **Expected Behavior**
   What should happen

   **Actual Behavior**
   What actually happens

   **Error Messages**
   Any error messages (redact credentials)
   ```

## 🔐 Security

- **Never commit credentials** or API keys
- **Redact sensitive data** in issues and PRs
- **Report security vulnerabilities** privately via email

## 💬 Questions?

- Open a [GitHub Discussion](https://github.com/YOUR_USERNAME/fortianalyzer-api-postman/discussions)
- Check [SETUP.md](SETUP.md) for usage questions
- Review [existing issues](https://github.com/YOUR_USERNAME/fortianalyzer-api-postman/issues)

## 📄 License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

Thank you for contributing! 🎉
