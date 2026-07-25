# Outlook Account Creator 📧

A high-performance, multi-threaded **Microsoft Outlook account creator** that automates the full signup flow using HTTP requests (no browser automation). Solves Arkose Labs FunCaptcha challenges via third-party services and supports proxy rotation.

## Features

- **Fully automated signup** — mimics the complete Microsoft Live signup flow with TLS fingerprinting (`tls_client`)
- **Multi-threaded** — configurable thread count for bulk account creation
- **Captcha solving** — supports **CapSolver**, **EZ-CAPTCHA**, and **CapBypass** for Arkose Labs FunCaptcha
- **Proxy support** — rotates proxies from a text file; supports authenticated proxies
- **TLS fingerprinting** — uses `tls_client` with `chrome126` identifier to bypass TLS-level detection
- **RSA password encryption** — replicates Microsoft's client-side `CipherValue` encryption via `cipher_value.js`
- **Telemetry spoofing** — sends realistic client telemetry events to avoid detection
- **Account output** — saves successfully created accounts to `output/Genned.txt`

## Prerequisites

- Python 3.8+
- A captcha service API key (CapSolver, EZ-CAPTCHA, or CapBypass)
- Working HTTP/HTTPS proxies

## Installation

```bash
git clone https://github.com/saifurrdev/outlook.git
cd outlook
pip install -r requirements.txt
```

## Configuration

Edit `config.yml`:

```yaml
threads: 5                           # Number of concurrent workers
solver: "CAPSOLVER"                  # CAPSOLVER, EZ-CAPTCHA, or CAPBYPASS
capKey: "CAP-..."                    # Your captcha service API key
```

### Captcha Solvers

| Solver | Type | Notes |
|--------|------|-------|
| `CAPSOLVER` | Proxyless | Uses `capsolver.com` API |
| `EZ-CAPTCHA` | Proxyless | Uses `ez-captcha.com` API |
| `CAPBYPASS` | Proxy-based | Uses `capbypass.com` API with your proxy |

## Usage

1. **Prepare proxies** — add one proxy per line in `input/proxies.txt`:
   ```
   user:pass@1.2.3.4:8080
   user:pass@5.6.7.8:3128
   ```

2. **Set your captcha API key** in `config.yml`

3. **Run the creator**:
   ```bash
   python main.py
   ```

Created accounts are saved to `output/Genned.txt` in `email:password` format.

## File Structure

```
outlook/
├── main.py              # Main account creation script
├── config.yml           # Configuration (threads, solver, API key)
├── cipher_value.js      # RSA encryption for password cipher value
├── requirements.txt     # Python dependencies
├── LICENSE              # Apache License 2.0
├── input/
│   └── proxies.txt      # HTTP/HTTPS proxy list
├── output/
│   └── Genned.txt       # Successfully created accounts
└── README.md            # This file
```

## How It Works

1. **Session init** — fetches the signup page, parses cookies (`amsc`, `uaid`), extracts API tokens (`apiCanary`, `hpgid`, `scid`, `SKI`, `Key`, `randomNum`)
2. **Fingerprinting** — requests a fingerprint token from `fpt.live.com`, extracts `txnId`, `ticks`, `rid`, `authKey`, `cid`
3. **Email availability check** — calls `CheckAvailableSigninNames` with the generated email
4. **Client telemetry** — sends `ReportClientEvent` telemetry to look like a real browser
5. **Account creation attempt** — calls `CreateAccount` with RSA-encrypted password; may trigger FunCaptcha
6. **Captcha solving** — if Arkose blob is returned, solves it via the configured service
7. **Enforcement flow** — sends `LoadEnforcement` and `CompleteEnforcement` events with the captcha token
8. **Final account creation** — retries `CreateAccount` with the solved captcha token

## Account Details

- **Email format**: `random10chars@outlook.com`
- **Password**: randomly generated
- **First name**: `justmanooo`
- **Last name**: `exploited7`
- **Birth date**: `1999-11-17`
- **Country**: `EG` (Egypt) or `TR` (Turkey)

## Dependencies

- `httpx`, `requests` — HTTP clients
- `tls-client` — TLS fingerprinting
- `beautifulsoup4` — HTML parsing
- `colorama` — colored console output
- `capsolver` — captcha solving
- `pyyaml` — YAML config parsing
- `pyexecjs` — JavaScript execution for RSA encryption

## License

Apache License 2.0 — see [LICENSE](LICENSE).
