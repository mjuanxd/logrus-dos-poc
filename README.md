# logrus DoS Vulnerability - Proof of Concept

This directory contains proof-of-concept code and test scripts for the Denial of Service (DoS) vulnerability in `github.com/sirupsen/logrus`.

## Vulnerability Summary

**CVE**: Pending assignment  
**CWE**: CWE-400 (Uncontrolled Resource Consumption)  
**CVSS**: 7.5 (High)

**Affected Versions**:
- `< v1.8.3` (including v1.8.1, v1.8.2)
- `v1.9.0`
- `v1.9.2`

**Fixed Versions**:
- `v1.8.3`
- `v1.9.1`
- `v1.9.3+`

## Files

- **poc_logrus_dos_improved.go** - Standalone PoC demonstrating the vulnerability
- **poc_logrus_dos.go** - Simpler PoC version
- **test_versions.ps1** - PowerShell script to test multiple versions
- **test_versions.sh** - Bash script to test multiple versions
- **test_single_version.ps1** - Quick test for a single version

## Quick Start

### Test a Single Version

```powershell
# PowerShell
.\test_single_version.ps1 v1.8.1
```

### Test All Versions

```powershell
# PowerShell
.\test_versions.ps1
```

```bash
# Bash/Linux
chmod +x test_versions.sh
./test_versions.sh
```

### Manual PoC

```bash
go mod init poclogrus
go get github.com/sirupsen/logrus@v1.8.1
go run poc_logrus_dos_improved.go
```

## Test Results

Comprehensive testing confirms:

| Version | Status     |
|---------|------------|
| v1.8.1  | VULNERABLE |
| v1.8.2  | VULNERABLE |
| v1.8.3  | FIXED      |
| v1.9.0  | VULNERABLE |
| v1.9.1  | FIXED      |
| v1.9.2  | VULNERABLE |
| v1.9.3  | FIXED      |

## How It Works

The vulnerability occurs when `logrus.Writer()` receives a single line larger than 64KB without newlines. The internal `bufio.Scanner` fails with "token too long" error, causing the Writer's pipe to close and making it unusable.

**Vulnerable versions**: Writer becomes unusable (DoS)  
**Fixed versions**: Error is logged but Writer continues to function

## References

- GitHub Issue: https://github.com/sirupsen/logrus/issues/1370
- GitHub PR: https://github.com/sirupsen/logrus/pull/1376
- Snyk Advisory: https://security.snyk.io/vuln/SNYK-GOLANG-GITHUBCOMSIRUPSENLOGRUS-5564391
- **CVE**: Pending assignment



