# The Stoic Letter

A collaborative team project that delivers a daily **Stoic wisdom newsletter** by email. Each day, the scheduler picks a random quote from a curated collection and sends it to the recipient at a configured time — no manual work required.

---

## What it does

- Sends one Stoic quote per day via email
- Runs on a background scheduler — set a time and forget about it
- Avoids repeating quotes until the full collection has been used
- Keeps configuration in a single `config.json` file

---

## Project structure

```
The-Stoic-Letter/
├── main.py              # Entry point — starts the scheduler
├── scheduler.py         # Daily job scheduling (schedule library)
├── config.json          # SMTP credentials, recipient, send time
├── mailer/
│   └── mailer.py        # Email composition and sending
└── quotes/
    ├── quotes.txt       # Stoic quotes collection
    └── quotes.py        # Random quote selection (no repeats)
```

---

## Requirements

- Python 3.8+
- A Gmail account (or any SMTP provider) with an [App Password](https://support.google.com/accounts/answer/185833) enabled

---

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/Aleksander-Mlynarski/The-Stoic-Letter.git
cd The-Stoic-Letter
```

### 2. Create and activate a virtual environment

**Windows (PowerShell):**

```powershell
python -m venv venv
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
.\venv\Scripts\Activate.ps1
```

**macOS / Linux:**

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install schedule
```

### 4. Configure `config.json`

Edit `config.json` with your own values:

```json
{
  "smtp": {
    "server": "smtp.gmail.com",
    "port": 587,
    "username": "your-email@gmail.com",
    "password": "your-app-password"
  },
  "recipient": "recipient@gmail.com",
  "send_time": "08:00"
}
```

| Field | Description |
|---|---|
| `smtp.username` | Sender email address |
| `smtp.password` | App password (not your regular Gmail password) |
| `recipient` | Who receives the daily quote |
| `send_time` | Daily send time in `HH:MM` format (24-hour clock) |

> **Note:** Never commit real credentials to the repository. Keep `config.json` with your personal settings locally, or use environment variables if you extend the project.

---

## Usage

With the virtual environment active, run:

```bash
python main.py
```

You should see:

```
Scheduled daily send at 08:00
```

The program runs continuously and sends the newsletter every day at the configured time.

To stop the scheduler, press `Ctrl+C`. A traceback on exit is expected — the scheduler runs in an infinite loop by design.

---

## How it works

1. **`main.py`** calls `start_scheduler()` from `scheduler.py`.
2. **`scheduler.py`** registers a daily job at `send_time` using the [`schedule`](https://pypi.org/project/schedule/) library and checks every second whether it is time to run.
3. **`quotes.py`** picks a random quote from `quotes.txt`, tracking used indexes in `quotes/used_quotes.txt` so no quote repeats until the pool is exhausted.
4. **`mailer.py`** builds a plain-text email and sends it via SMTP.

---

## Team project

This repository is maintained as a **team effort**. Each member can contribute quotes, adjust the send schedule, or improve the mailer and scheduler logic. When collaborating:

- Open a branch for your changes and submit a pull request
- Do not push personal SMTP credentials
- Add new quotes to `quotes/quotes.txt` — one quote per line

---

## License

This project is for educational and personal use by the team.
