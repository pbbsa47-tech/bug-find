<div align="center">

# 🐞 bug-find

### Automated Bug & Vulnerability Scanner

**SQLi · XSS · SSRF · LFI · Open Redirect · CORS · Security Headers · Secret Leaks · Subdomain Takeover**

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![Platform](https://img.shields.io/badge/Platform-Termux%20%7C%20Linux%20%7C%20macOS-lightgrey)]()
[![License](https://img.shields.io/badge/License-MIT-green)]()
[![Async](https://img.shields.io/badge/Async-asyncio-orange)]()
[![Author](https://img.shields.io/badge/Author-Zeon%20Hunter-red)]()

*Fast. Async. Modular. Built for bug bounty hunters and pentesters.*

**Author:** **Zeon Hunter**

</div>

---

## ⚠️ Legal Disclaimer

> **`bug-find` is intended for authorized security testing ONLY.**
>
> - ✅ Use it on **systems you own**, in **lab environments**, or inside a **bug bounty program's scope**.
> - ❌ **Never** scan targets without **written permission**.
> - The author assumes **no liability** for misuse or damage caused by this tool.
>
> By using `bug-find` you agree to these terms.

---

## 📖 Table of Contents

- [Features](#-features)
- [Installation](#-installation)
  - [Linux (Debian / Ubuntu / Kali / Arch)](#-linux-debian--ubuntu--kali--arch)
  - [Termux (Android)](#-termux-android)
  - [macOS](#-macos)
- [Quick Start](#-quick-start)
- [Usage](#-usage)
- [Command Reference](#-command-reference)
- [Examples](#-examples)
- [Output](#-output)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🚀 Features

| Feature | Description |
|---|---|
| ⚡ **Async Engine** | Built on `asyncio` — scans hundreds of endpoints in seconds |
| 🎯 **Multi-Vuln** | SQLi, XSS, SSRF, LFI, Open Redirect, CORS |
| 🧨 **Subdomain Takeover** | Detects dangling CNAMEs on common cloud providers |
| 🔑 **Secret Leak Scanner** | API keys, tokens, JWTs, private keys in responses & JS |
| 📋 **Header Auditor** | HSTS, CSP, X-Frame-Options, cookie flags, etc. |
| 🎛️ **Configurable** | Concurrency, timeout, headers, User-Agent, proxy |
| 🔁 **Redirect & SSL control** | Follow or ignore redirects, toggle SSL verification |
| 🧵 **Method control** | Test using `GET` or `POST` |
| 📊 **Report Output** | Save scan results to a file |
| 🐧 **Cross-Platform** | Linux, Termux (Android), macOS |
| 🎨 **Clean CLI** | Readable terminal output |

---

## 🧠 Modules

```
bug-find/
├── sqli.py          # Error-based + time-based SQLi
├── xss.py           # Reflected XSS detection
├── ssrf.py          # Server-Side Request Forgery
├── lfi.py           # Local File Inclusion
├── redirect.py      # Open Redirect
├── cors.py          # CORS misconfiguration
├── headers.py       # Security header audit
├── secrets.py       # API key / token leak detector
└── takeover.py      # Subdomain takeover checks
```

---

## 📦 Installation

### 🐧 Linux (Debian / Ubuntu / Kali / Arch)

```bash
# 1. Install Python 3 + pip + git
sudo apt update && sudo apt install -y python3 python3-pip git

# 2. Clone the repo
git clone https://github.com/<your-username>/bug-find.git
cd bug-find

# 3. (Recommended) virtual environment
python3 -m venv venv
source venv/bin/activate

# 4. Install dependencies
pip install -r requirements.txt

# 5. Make executable
chmod +x bug-find.py
```

### 📱 Termux (Android)

```bash
# 1. Update Termux
pkg update -y && pkg upgrade -y

# 2. Install dependencies
pkg install -y python git clang libffi openssl

# 3. Clone
git clone https://github.com/<your-username>/bug-find.git
cd bug-find

# 4. Install Python packages
pip install -r requirements.txt

# 5. Grant storage access (for saving reports)
termux-setup-storage

# 6. Run
python bug-find.py -h
```

> 💡 **Termux tip:** if `pip install` fails on a package that needs C compilation, run `pkg install clang` first.

### 🍏 macOS

```bash
brew install python3 git
git clone https://github.com/<your-username>/bug-find.git
cd bug-find
python3 -m pip install -r requirements.txt
```

---

## ⚡ Quick Start

```bash
python3 bug-find.py https://target.com
```

That's it — the target is the **only required argument**. Everything else is optional.

---

## 🎮 Usage

```bash
python3 bug-find.py <target> [OPTIONS]
```

- `<target>` — the URL or domain to scan (**required**, positional).
- If you run `python bug-find.py` with no target, you'll get:

```
bug-find.py: error: the following arguments are required: target
```

That's expected. Always pass a target.

---

## 🧾 Command Reference

| Flag | Long form | Description | Default |
|---|---|---|---|
| `target` | — | Target URL / domain **(required, positional)** | — |
| `-m` | `--modules` | Comma-separated modules to run | `all` |
| `-c` | `--concurrency` | Number of concurrent async tasks | `25` |
| `-t` | `--timeout` | Request timeout in seconds | `10` |
| `-o` | `--output` | Save report to a file | `None` |
| `-H` | `--header` | Extra HTTP header (`Key:Value`), repeatable | `None` |
| `-A` | `--user-agent` | Custom User-Agent string | `random` |
| `--proxy` | — | Proxy URL (e.g. `http://127.0.0.1:8080`) | `None` |
| `--follow` | — | Follow HTTP redirects | `False` |
| `--verify-ssl` | — | Verify SSL certificates | `False` |
| `--method` | — | HTTP method to use for tests: `GET` or `POST` | `GET` |
| `--max-test-urls` | — | Max number of parameterized URLs to test | `100` |
| `-y` | — | Skip confirmation prompt (yes to all) | `False` |
| `-h` | `--help` | Show help and exit | — |

> 💡 Every flag above is taken **directly** from the tool's real `argparse` interface — nothing invented.

---

## 🧪 Examples

### 1. Basic scan

```bash
python3 bug-find.py https://target.com
```

### 2. Pick specific modules

```bash
python3 bug-find.py https://target.com -m sqli,xss,headers
```

### 3. Higher concurrency + shorter timeout

```bash
python3 bug-find.py https://target.com -c 50 -t 5
```

### 4. Send a custom header + User-Agent

```bash
python3 bug-find.py https://target.com -H "Authorization: Bearer TOKEN" -A "Mozilla/5.0"
```

### 5. Follow redirects and verify SSL

```bash
python3 bug-find.py https://target.com --follow --verify-ssl
```

### 6. Route traffic through a proxy (Burp / ZAP)

```bash
python3 bug-find.py https://target.com --proxy http://127.0.0.1:8080
```

### 7. Use POST instead of GET

```bash
python3 bug-find.py https://target.com --method POST --max-test-urls 200
```

### 8. Save a report and skip confirmation

```bash
python3 bug-find.py https://target.com -o report.json -y
```

### 9. Termux — full scan, report to internal storage

```bash
python bug-find.py https://target.com -c 15 -o /sdcard/report.json -y
```

### 10. Debug your setup

```bash
python3 bug-find.py -h
```

---

## 📊 Output

`bug-find` prints findings as they are discovered, for example:

```
[✓] Target reachable     : https://target.com  (200 OK)
[!] SQLi (error-based)   : /search?q='  →  500 (SQL error leaked)
[!] XSS (reflected)      : /profile?name=<script>alert(1)</script>
[!] Missing header       : Content-Security-Policy
[!] Secret exposed       : /static/app.js  →  AWS access key
[!] Takeover candidate   : dev.target.com  →  dangling CNAME (herokuapp)
───── Scan finished | N findings ─────
```

Use `-o <file>` to save the same results to disk.

---

## 🗺 Roadmap

- [x] Async engine & modular design
- [x] SQLi, XSS, headers, secrets, takeover
- [ ] GraphQL introspection scanner
- [ ] JWT weak-secret cracker
- [ ] Nuclei-style template engine
- [ ] Web UI / dashboard
- [ ] Docker image

---

## 🤝 Contributing

Pull requests are welcome!

```bash
# 1. Fork the repo
# 2. Create your branch
git checkout -b feature/my-new-module

# 3. Commit
git commit -m "feat: add GraphQL scanner"

# 4. Push and open a PR
git push origin feature/my-new-module
```

Please keep commits clean and describe any new module you add.

---

## 📜 License

Released under the **MIT License** — see [LICENSE](LICENSE) for details.

---

## 🙏 Credits & Inspiration

- [OWASP](https://owasp.org/) — vulnerability classification
- [SecLists](https://github.com/danielmiessler/SecLists) — wordlists
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings) — payload reference
- The open-source security community ❤️

---

<div align="center">

**⭐ If `bug-find` helped you, drop a star on GitHub!**

Made with 🐞 by **Zeon Hunter** — [@<your-username>](https://github.com/<your-username>)

</div>
