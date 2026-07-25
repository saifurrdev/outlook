# outlook

Automated Outlook account creator with captcha solving support. Multi-threaded, proxy-aware, and requests-based.

## Features

- Create Outlook accounts quickly
- Multi-threaded (configurable thread count)
- Captcha solving via CapSolver or CapBypass
- Proxy support
- Fully requests-based (no browser automation)

## Setup

1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Edit `config.yml` with your solver choice and API key.
3. Add proxies to `input/proxies.txt` (format: `user:pass@ip:port`).
4. Run:
   ```bash
   python main.py
   ```

## Configuration

Edit `config.yml`:

| Key | Description |
|-----|-------------|
| `threads` | Number of concurrent threads |
| `solver` | Captcha solver: `CAPSOLVER` or `CAPBYPASS` |
| `capKey` | Your captcha service API key |

## Files

| File | Purpose |
|------|---------|
| `main.py` | Main account creator script |
| `config.yml` | Configuration file |
| `cipher_value.js` | Cipher value calculation for Outlook signup |
| `input/proxies.txt` | Proxy list |
| `requirements.txt` | Python dependencies |
