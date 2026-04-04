# 🎮 Free Games Auto Notifier

> Automatically tracks **Epic Games** and **Steam** free game offers and sends you a beautiful HTML email notification whenever new free games appear — powered by GitHub Actions.

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-Automated-success?logo=github-actions&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)
![Schedule](https://img.shields.io/badge/Runs%20Every-6%20Hours-orange?logo=clockify&logoColor=white)

---

## ✨ Features

- 🛒 **Epic Games** — Fetches currently free + upcoming free games via the official API
- 🎮 **Steam** — Scrapes "Free to Keep" deals and "Free Weekend" events
- 📧 **Rich HTML Email** — Dark-themed, card-style email with game images, dates, and claim links
- 🧠 **Smart Change Detection** — Only sends an email when the game lineup actually changes (no spam!)
- 🤖 **Fully Automated** — Runs every 6 hours via GitHub Actions, zero manual effort
- 🔒 **Secure** — Credentials stored as GitHub Secrets, never hardcoded

---

## 📁 Project Structure

```
📦 free-games-notifier/
├── epic.PY          # Epic Games notifier — fetches API, builds email, sends notification
├── steam.py         # Steam notifier — scrapes store, detects free games, sends notification
├── free.txt         # State file: stores last known Epic Games lineup (auto-updated by CI)
├── free-steam.txt   # State file: stores last known Steam lineup (auto-updated by CI)
└── .github/
    └── workflows/
        └── main.yml # GitHub Actions workflow — runs both scripts every 6 hours
```

---

## 🚀 Getting Started

### 1. Fork or Clone this Repository

```bash
git clone https://github.com/your-username/free-games-notifier.git
cd free-games-notifier
```

### 2. Install Dependencies Locally (Optional)

```bash
pip install requests beautifulsoup4
```

### 3. Configure GitHub Secrets

Go to **Settings → Secrets and variables → Actions → New repository secret** and add:

| Secret Name | Description |
|-------------|-------------|
| `EMAIL` | Your Gmail address (sender) |
| `PASSWORD` | Your Gmail **App Password** (not your login password) |
| `TO_EMAIL` | The email address to receive notifications |

> **⚠️ Important:** You must use a [Gmail App Password](https://support.google.com/accounts/answer/185833), not your regular Gmail password. Enable 2-Step Verification first, then generate an App Password under Security settings.

---

## 🔧 How It Works

### Epic Games (`epic.PY`)

1. Calls the Epic Games Store promotions API
2. Parses currently free and upcoming free games
3. Converts all dates to **IST (Indian Standard Time)**
4. Generates a signature of current game titles
5. Compares against `free.txt` — if changed, sends an email and updates the state file

### Steam (`steam.py`)

1. Scrapes Steam's search page for `Free to Keep` deals (100% discounted)
2. Also checks Steam's featured categories API for `Free Weekend` events
3. Generates a signature of all found game titles
4. Compares against `free-steam.txt` — sends email only on changes

### Email Format

Both scripts send a beautifully formatted dark-mode HTML email containing:
- 🖼️ Game banner image
- 🎮 Game title with a direct link
- 📅 Start and end dates (IST)
- ✅ "Claim Now" / "Open in Steam" button

---

## ⚙️ GitHub Actions Workflow

The workflow runs automatically:

| Trigger | Details |
|---------|---------|
| ⏰ Schedule | Every **6 hours** (`0 */6 * * *`) |
| 🖱️ Manual | Via **workflow_dispatch** (run anytime from the Actions tab) |

**Steps:**
1. Checks out the repository
2. Sets up Python 3.11
3. Installs `requests` and `beautifulsoup4`
4. Runs `epic.PY` with email secrets injected
5. Runs `steam.py` with email secrets injected
6. Commits and pushes updated state files (`free.txt`, `free-steam.txt`) if changed

---

## 🏃 Running Manually

```bash
# Set environment variables
export EMAIL="you@gmail.com"
export PASSWORD="your-app-password"
export TO_EMAIL="recipient@example.com"

# Run Epic Games notifier
python epic.PY

# Run Steam notifier
python steam.py
```

On Windows (PowerShell):

```powershell
$env:EMAIL="you@gmail.com"
$env:PASSWORD="your-app-password"
$env:TO_EMAIL="recipient@example.com"

python epic.PY
python steam.py
```

---

## 📦 Dependencies

| Package | Used By | Purpose |
|---------|---------|---------|
| `requests` | Both scripts | HTTP API calls & web scraping |
| `beautifulsoup4` | `steam.py` | HTML parsing for Steam store page |
| `smtplib` | Both scripts | Sending emails via Gmail SMTP |

---

## 🛡️ Security Notes

- Credentials are **never** stored in the repository
- All sensitive values are injected at runtime via [GitHub Secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
- State files (`free.txt`, `free-steam.txt`) are committed by the GitHub Actions bot — they contain only game titles, no personal data

---

## 🤝 Contributing

Pull requests are welcome! If you'd like to add support for another platform (GOG, Humble Bundle, etc.), feel free to open an issue or submit a PR.

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

<p align="center">Made with ❤️ and automated with 🤖 GitHub Actions</p>

## Use emailjs without Gmail access
* https://www.emailjs.com/

* https://dashboard.emailjs.com/admin/templates/0pjcevb
```
{{subject}}
{{{html}}}
{{to_email}}
```
