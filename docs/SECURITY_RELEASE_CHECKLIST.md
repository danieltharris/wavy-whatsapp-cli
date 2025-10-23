# Security Pre-Release Checklist

Use this checklist before creating a new release to ensure security best practices are followed.

## Pre-Release Security Review

### Code Review
- [ ] All code changes have been peer-reviewed
- [ ] No secrets, credentials, or API keys in code
- [ ] No sensitive information in comments
- [ ] All TODO/FIXME security items addressed
- [ ] Security-critical changes have additional review

### Dependency Security
- [ ] Run `govulncheck ./...` - no vulnerabilities found
- [ ] All dependencies are up to date
- [ ] Check for security advisories on dependencies
- [ ] Review `go.sum` for unexpected changes
- [ ] No deprecated or unmaintained dependencies

### Testing
- [ ] All unit tests pass
- [ ] All integration tests pass
- [ ] Security-focused tests added for new features
- [ ] Manual testing completed
- [ ] No debug code or logging left enabled

### Static Analysis
- [ ] Run `go vet ./...` - no issues
- [ ] Run `gosec -conf .gosec.json ./...` - no critical issues
- [ ] Run `staticcheck ./...` - no issues
- [ ] Run `gofmt -l .` - all files formatted
- [ ] Code passes all CI/CD checks

### Build Security
- [ ] Clean build from fresh clone succeeds
- [ ] Build process doesn't require elevated privileges
- [ ] No build warnings or errors
- [ ] Binary size is reasonable (no bloat)
- [ ] Binary works on all target platforms

### Documentation
- [ ] CHANGELOG.md updated with security fixes
- [ ] README.md reflects current security status
- [ ] SECURITY.md is up to date
- [ ] Release notes mention security improvements
- [ ] Breaking changes documented

### Release Artifacts
- [ ] Generate SHA256 checksums for all binaries
- [ ] Sign release with GPG (if available)
- [ ] Tag follows semantic versioning
- [ ] Release notes are clear and accurate
- [ ] All artifacts uploaded successfully

### Post-Release
- [ ] Verify download links work
- [ ] Test installation on clean system
- [ ] Monitor for issues in first 24 hours
- [ ] Update security documentation if needed
- [ ] Announce security fixes (if applicable)

## Security-Specific Releases

For security-focused releases (patches, vulnerabilities):

- [ ] CVE requested if applicable
- [ ] Security advisory drafted
- [ ] Affected versions clearly documented
- [ ] Upgrade path is clear
- [ ] Backports considered for older versions
- [ ] Coordinated disclosure followed
- [ ] Credits given to security researchers

## Critical Security Release

For critical security issues:

- [ ] Fast-track review process initiated
- [ ] Minimal changes (only the fix)
- [ ] Expedited testing
- [ ] Security advisory prepared
- [ ] Release notes emphasize urgency
- [ ] Notification plan for users
- [ ] Consider hotfix branch

## Rollback Plan

- [ ] Previous version still available
- [ ] Rollback instructions documented
- [ ] Known issues documented
- [ ] Support plan for users

## Verification Commands

```bash
# Clean checkout
git clone https://github.com/danieltharris/wavy-whatsapp-cli.git
cd wavy-whatsapp-cli

# Security checks
govulncheck ./...
gosec -conf .gosec.json ./...
staticcheck ./...
go vet ./...

# Build and test
go test ./...
mage build

# Generate checksums
cd bin
sha256sum * > checksums.txt
```

## Notes

- This checklist should be updated as security practices evolve
- Not all items may apply to every release
- Use judgment for minor vs. major releases
- Document any skipped items and reasoning
