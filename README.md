# Linux Server Hardening Automation

A modular shell-based security automation suite designed to transform a freshly installed Linux server into a secure, production-grade node.

This repository automates system security baseline configurations, SSH key-only access enforcement, firewall setup (`ufw`), intrusion prevention (`fail2ban`), automated security patching, kernel parameter hardening, and auditing.

---

## Features

- Non-interactive and interactive execution modes
- Automated deployment of `ufw` firewall rules and default-deny ingress policy
- SSH daemon hardening (disables root login, password authentication, and custom port configuration)
- `fail2ban` deployment with jail configurations for SSH brute-force protection
- Kernel network stack hardening via `/etc/sysctl.d/99-security-hardening.conf`
- Unattended automatic security updates configuration
- System compliance verification and audit reporting script

---

## Repository Structure

```text
.
├── harden.sh                  # Main automated execution engine
├── audit.sh                   # System security compliance checker
├── config/
│   ├── sshd_config.d/         # Custom SSH hardening drops
│   ├── sysctl.d/              # Kernel security flags
│   └── fail2ban/              # Custom jail configurations
└── docs/
    └── hardening_checklist.md # Manual audit and compliance documentation
```

---

## Installation & Usage

### 1. Clone the Repository

```bash
git clone https://github.com/Duncan1738/Linux-Server-Hardening-Automation.git
cd Linux-Server-Hardening-Automation
```

### 2. Make Scripts Executable

```bash
chmod +x harden.sh audit.sh
```

### 3. Run Pre-Hardening Audit

Verify current security vulnerabilities before applying changes:

```bash
sudo ./audit.sh
```

### 4. Execute Server Hardening

Run the main execution engine with root privileges:

```bash
sudo ./harden.sh
```

---

## Script Modules Executed

1. System Package Update & Security Tools (`ufw`, `fail2ban`, `unattended-upgrades`, `auditd`)
2. User Security & SSH Hardening:
   - Disables root password and direct root SSH login (`PermitRootLogin no`)
   - Restricts login to SSH key authentication (`PasswordAuthentication no`)
3. Firewall Configuration:
   - Sets default incoming to `deny` and default outgoing to `allow`
   - Opens SSH port (Default: 22 or custom port) and specified service ports
4. Intrusion Prevention (`fail2ban`):
   - Bans IP addresses after 3 failed authentication attempts for 1 hour
5. Sysctl Kernel Hardening:
   - Disables IP forwarding, ICMP redirects, source routing, and enables SYN cookies
6. Automatic Updates:
   - Configures `unattended-upgrades` to automatically install critical OS security patches

---

## System Compliance Verification

After running `harden.sh`, verify system posture with the audit tool:

```bash
sudo ./audit.sh
```

---

## Supported Operating Systems

- Ubuntu 20.04 LTS / 22.04 LTS / 24.04 LTS
- Debian 11 (Bullseye) / 12 (Bookworm)

---

## Author & License

Maintained by **Duncan1738**.

This project is open-source under the [MIT License](LICENSE).
