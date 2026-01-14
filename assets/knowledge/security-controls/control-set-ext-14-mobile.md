<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.0 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# Control Set 15: Mobile Security (MOBILE)

**Domain**: MOBILE - Mobile Application Security
**Version**: 1.0
**Last Updated**: 2025-12-30

---

## Overview

Mobile application security control set, applicable to iOS/Android native and cross-platform applications. Automatically loaded when the following trigger conditions are detected:
- iOS projects (*.xcodeproj, Podfile)
- Android projects (build.gradle, AndroidManifest.xml)
- React Native (react-native.config.js)
- Flutter (pubspec.yaml)

---

## Core Controls

### MOBILE-01: Secure Local Storage
**Control Requirement**: Local storage must be encrypted and protected

- Use platform secure storage (Keychain/Keystore)
- Encrypt sensitive data at rest
- Prohibit plaintext credential storage
- Clean up data on application uninstall

### MOBILE-02: Transport Security
**Control Requirement**: Network transport must be secure

- Enforce HTTPS communication
- Certificate pinning
- Prohibit insecure TLS configurations
- Detect proxy/man-in-the-middle attacks

### MOBILE-03: Code Protection
**Control Requirement**: Application code must be protected

- Code obfuscation (ProGuard/R8/SwiftShield)
- Prevent decompilation and debugging
- Resource file encryption
- Implement sensitive logic server-side

### MOBILE-04: Runtime Protection
**Control Requirement**: Runtime environment must be secure

- Detect jailbroken/rooted devices
- Prevent dynamic injection
- Integrity verification
- Debug detection and prevention

### MOBILE-05: Authentication Security
**Control Requirement**: Mobile authentication must be secure

- Biometric integration (Face ID/Touch ID)
- Secure session management
- Device binding and registration
- Offline authentication strategy

### MOBILE-06: Data Leakage Prevention
**Control Requirement**: Prevent data leakage

- Prohibit sensitive data on clipboard
- Screenshot protection
- Log sanitization
- Third-party SDK audit

---

## L4 References

Detailed practice guides in the `references/` directory:
- reference-set-15-mobile-security.md
- reference-set-15-certificate-pinning.md

---

## STRIDE Mapping

| STRIDE | Applicable Controls |
|--------|---------------------|
| S | MOBILE-04, MOBILE-05 |
| T | MOBILE-03, MOBILE-04 |
| R | MOBILE-05 |
| I | MOBILE-01, MOBILE-02, MOBILE-06 |
| D | MOBILE-04 |
| E | MOBILE-04, MOBILE-05 |
