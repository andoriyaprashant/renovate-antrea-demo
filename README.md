# Antrea LFX Mentorship Renovate Demo

This is a demo project for the LFX Mentorship under Antrea. It demonstrates usage of Renovate to detect and fix vulnerabilities in Go dependencies.

## What It Does

- Fetches the current price of Bitcoin from the CoinGecko API.
- Generates a JWT token using the `github.com/dgrijalva/jwt-go` library.

## Why It's Vulnerable

I intentionally use a vulnerable version of the `jwt-go` library (v2.2.0) which is affected by **CVE-2020-26160**:

More info: [https://nvd.nist.gov/vuln/detail/CVE-2020-26160](https://nvd.nist.gov/vuln/detail/CVE-2020-26160)

## How to Run

```bash
go run main.go
```
## Renovate Configuration
This project includes a renovate.json file configured to:

- Enable gomod manager for Go modules.

- Only create pull requests when an update addresses known vulnerabilities.

- Apply updates of any type (major, minor, patch) that fix security issues.

- Use clear commit labeling and messages for better traceability.

```json
{
    "$schema": "https://docs.renovatebot.com/renovate-schema.json",
    "extends": ["config:base"],
    "enabledManagers": ["gomod"],
    "packageRules": [
      {
        "matchManagers": ["gomod"],
        "matchPackagePatterns": [".*"],
        "matchUpdateTypes": ["major", "minor", "patch"],
        "vulnerabilityAlertsOnly": true,
        "enabled": true
      }
    ],
    "labels": ["dependencies", "security"],
    "commitMessagePrefix": "fix(deps): "
  }
  
```

## Expected Behavior

- Renovate will scan the repository for vulnerable Go dependencies.

- If it finds a known CVE (like CVE-2020-26160), it will automatically open a pull request to upgrade the package.

- This project serves as a simplified proof of concept for how Antrea could configure Renovate for better control and security over dependency updates across both main and active release branches.