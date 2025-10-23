# Security Policy

## Supported Versions

We release patches for security vulnerabilities in the following versions:

| Version | Supported          |
| ------- | ------------------ |
| Latest  | :white_check_mark: |
| < Latest| :x:                |

We recommend always using the latest version of Wavy WhatsApp CLI.

## Reporting a Vulnerability

We take security seriously. If you discover a security vulnerability, please follow these steps:

### How to Report

1. **DO NOT** open a public GitHub issue for security vulnerabilities
2. Email the maintainers at: [CONTACT_EMAIL_HERE] (or use GitHub Security Advisories)
3. Include the following information:
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact
   - Suggested fix (if any)

### What to Expect

- **Acknowledgment**: We will acknowledge receipt of your report within 48 hours
- **Investigation**: We will investigate and validate the vulnerability
- **Timeline**: We aim to provide an initial assessment within 7 days
- **Fix**: Critical vulnerabilities will be patched as soon as possible
- **Credit**: We will credit you in the security advisory (unless you prefer to remain anonymous)

### Responsible Disclosure

We request that you:
- Give us reasonable time to fix the vulnerability before public disclosure
- Do not exploit the vulnerability beyond what is necessary to demonstrate it
- Do not access, modify, or delete other users' data
- Act in good faith to avoid privacy violations and service disruptions

## Security Best Practices for Users

### 1. Data Storage Security

The WhatsApp session data is stored in `~/.local/share/wavy/client.db`. This database contains sensitive authentication tokens that could be used to hijack your WhatsApp session.

**Recommendations:**
- Ensure your home directory has appropriate permissions (700)
- Use full disk encryption on your system
- Never share your `client.db` file with anyone
- Never commit `client.db` to version control
- Regularly backup and securely delete old backups

### 2. System Security

**Recommendations:**
- Keep your operating system updated
- Use strong passwords/passphrases
- Enable a firewall
- Use antivirus/anti-malware software
- Avoid running wavy with elevated privileges (sudo)

### 3. Session Management

**Recommendations:**
- Regularly review linked devices in WhatsApp mobile app
- Remove wavy sessions when not actively using the tool
- Don't use wavy on shared/public computers
- Run `wavy setup` again if you suspect session compromise

### 4. Network Security

**Recommendations:**
- Use wavy on trusted networks
- Avoid using on public Wi-Fi without VPN
- Keep your network equipment firmware updated

### 5. Debug Mode Caution

When using `--debug` flag:
- Debug output may contain sensitive information (phone numbers, JIDs)
- Never share debug logs publicly
- Only use debug mode when necessary
- Review debug output before sharing for troubleshooting

### 6. Message Security

**Recommendations:**
- Be cautious when automating message sending
- Respect WhatsApp's rate limits to avoid account bans
- Don't send spam or unsolicited messages
- Verify recipient phone numbers before sending

### 7. Binary Verification

**Recommendations:**
- Only download binaries from official GitHub releases
- Verify checksums when available
- Check code signatures when available
- Build from source if you need maximum assurance

## Known Security Considerations

### 1. Unencrypted Local Database

**Issue**: The SQLite database storing session data is not encrypted by default.

**Risk**: Anyone with filesystem access can read your session data.

**Mitigation**: 
- Use operating system-level disk encryption
- Ensure proper file permissions (see below)
- Future versions may implement database encryption

### 2. File Permissions

**Default Behavior**: 
- Config directory: `~/.config/wavy/` (755)
- Data directory: `~/.local/share/wavy/` (755)
- Database file: `~/.local/share/wavy/client.db` (created by SQLite)

**Recommended Actions**:
```bash
chmod 700 ~/.config/wavy
chmod 700 ~/.local/share/wavy
chmod 600 ~/.local/share/wavy/client.db
```

### 3. QR Code Temporary Files

**Issue**: During setup, QR codes are temporarily saved as PNG files.

**Risk**: QR code contains pairing secret that could be used for session hijacking if recovered.

**Mitigation**:
- QR code is automatically deleted after authentication
- Use `Ctrl+C` to abort setup if needed (also deletes QR code)
- QR codes expire quickly

### 4. WhatsApp API Limits

**Issue**: WhatsApp has rate limits and anti-spam measures.

**Risk**: Excessive messaging could result in account ban.

**Mitigation**:
- Use responsibly
- Don't automate bulk messaging
- Respect WhatsApp's terms of service

## Security Features

### Current Security Features

1. **Local Operation**: All operations are local, no data sent to third parties
2. **Official WhatsApp Protocol**: Uses the official whatsmeow library
3. **XDG Compliance**: Follows XDG Base Directory specification
4. **No Telemetry**: No analytics or tracking
5. **Open Source**: Code is publicly auditable

### Planned Security Enhancements

1. **Database Encryption**: Encrypt session database
2. **Secure File Deletion**: Securely wipe sensitive files
3. **Release Signing**: Sign releases with GPG/cosign
4. **Checksum Verification**: Publish SHA256 checksums
5. **Dependency Scanning**: Automated vulnerability scanning
6. **Rate Limiting**: Built-in rate limiting for message sending

## Security Audit

A comprehensive security audit has been performed and documented in [SECURITY_AUDIT.md](SECURITY_AUDIT.md). This audit includes:

- Static code analysis
- Dependency vulnerability assessment
- Authentication and session management review
- Data storage security analysis
- Input validation assessment
- Operational security evaluation

## Compliance

### Data Protection

Wavy WhatsApp CLI:
- Stores data locally only
- Does not transmit data to third parties (except WhatsApp servers)
- Does not collect analytics or telemetry
- Users are responsible for their data under WhatsApp's terms

### Privacy

- Contact information is stored locally in SQLite database
- Message content is not persisted by wavy
- WhatsApp end-to-end encryption is maintained
- Users should review WhatsApp's privacy policy

## Dependencies

Wavy relies on several third-party libraries. We monitor these for security updates:

- `go.mau.fi/whatsmeow` - WhatsApp Web API library
- `github.com/mattn/go-sqlite3` - SQLite driver
- `github.com/spf13/cobra` - CLI framework
- `github.com/skip2/go-qrcode` - QR code generation

We recommend users also monitor these dependencies and report any concerns.

## Threat Model

### Assumed Threats

1. **Malware on User System**: Malware could access session database
2. **Physical Access**: Attacker gains physical access to user's computer
3. **Network Eavesdropping**: Attacker monitors network traffic
4. **Backup Theft**: Encrypted or unencrypted backups are stolen
5. **Supply Chain Attack**: Compromised dependencies or binaries

### Out of Scope

1. **WhatsApp Server Compromise**: We rely on WhatsApp's server security
2. **Mobile Device Compromise**: We don't control the linked mobile device
3. **Operating System Vulnerabilities**: We rely on OS security

### Trust Boundaries

1. **Local Filesystem**: We trust the local filesystem security
2. **Go Standard Library**: We trust Go's standard library
3. **whatsmeow Library**: We trust the whatsmeow library
4. **Operating System**: We trust the underlying OS

## Security Checklist for Contributors

If you're contributing to this project, please:

- [ ] Never commit secrets, tokens, or credentials
- [ ] Validate all user inputs
- [ ] Use secure random number generation when needed
- [ ] Avoid hardcoding sensitive paths or data
- [ ] Use parameterized queries for database operations
- [ ] Handle errors gracefully without exposing sensitive details
- [ ] Add tests for security-critical code
- [ ] Document security implications of changes
- [ ] Run `go vet` and fix all warnings
- [ ] Consider OWASP Top 10 when making changes

## Security Tools

We recommend contributors and users use these tools:

### For Development
- `go vet` - Built-in static analyzer
- `gosec` - Security-focused static analyzer
- `govulncheck` - Vulnerability scanner
- `staticcheck` - Static analysis tool

### For Users
- `checksec` - Binary security checker (when available)
- `rkhunter` - Rootkit detection
- `lynis` - Security auditing tool

## Updates and Patches

### Update Notifications

We recommend:
- Watching this repository for releases
- Subscribing to GitHub notifications
- Following the project for announcements

### Applying Updates

To update wavy:
```bash
# Download latest binary from GitHub releases
# Or rebuild from source:
git pull origin main
mage build
mage install
```

## Contact

For security-related questions or concerns:
- GitHub Security Advisories: [Enable in repository settings]
- Email: [CONTACT_EMAIL_HERE]
- General Issues: [GitHub Issues](https://github.com/danieltharris/wavy-whatsapp-cli/issues) (non-security only)

## Acknowledgments

We appreciate the security research community and responsible disclosure. Security researchers who report valid vulnerabilities will be acknowledged in our security advisories (unless they prefer anonymity).

---

**Last Updated:** October 23, 2025  
**Version:** 1.0
