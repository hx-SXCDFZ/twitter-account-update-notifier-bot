# Stalk Tanji

A lightweight Python monitor for `https://x.com/<username>/with_replies`.

It opens X in Chrome through Selenium, tracks recent posts/replies from a target
account, stores seen post IDs locally, alerts in the terminal, plays a local beep,
and can optionally send Feishu or DingTalk webhook notifications.

## Features

- Watches a target X account's `with_replies` timeline.
- Tracks multiple recent post IDs to reduce duplicate alerts after restarts.
- Rebuilds the Chrome driver after recoverable browser failures.
- Uses a temporary writable Chrome profile by default.
- Can reuse an existing Chrome profile when X login state is required.
- Supports optional Feishu and DingTalk webhook notifications.

## Requirements

- Python 3.10+
- Google Chrome
- Windows, macOS, or Linux

## Setup

Install dependencies:

```bash
pip install -r requirements.txt
```

Create a local environment file:

```bash
copy .env.example .env
```

On macOS or Linux:

```bash
cp .env.example .env
```

Edit `.env` and set at least:

```dotenv
TARGET_USERNAME=jack
```

Do not commit `.env`; it may contain local paths, login profile locations, and
webhook secrets.

## Run

```bash
python monitor.py
```

On the first run, if no previous state exists, the current recent posts are saved
as the baseline and future changes trigger alerts.

## Configuration

All configuration is read from `.env`.

| Variable | Default | Description |
| --- | --- | --- |
| `TARGET_USERNAME` | required | X username to monitor, with or without `@`. |
| `CHECK_INTERVAL_SECONDS` | `60` | Base polling interval. |
| `CHECK_JITTER_SECONDS` | `10` | Random jitter added to the polling interval. |
| `PAGE_LOAD_TIMEOUT_SECONDS` | `45` | Selenium page load timeout. |
| `RECENT_POST_COUNT` | `5` | Number of recent authored posts to track. |
| `STATE_FILE` | `state/last_seen.json` | Local state file path. |
| `LOG_FILE` | `logs/monitor.log` | Local log file path. |
| `ENABLE_BEEP` | `true` | Enables local sound alerts. |
| `CHROME_USER_DATA_DIR` | empty | Optional Chrome user data directory for reusing login state. |
| `CHROME_PROFILE_DIRECTORY` | empty | Optional Chrome profile name, such as `Default` or `Profile 1`. |
| `STARTUP_LOGIN_WAIT_SECONDS` | `0` | Optional startup wait for manual login before the first scrape. |
| `FEISHU_WEBHOOK` | empty | Optional Feishu bot webhook URL. |
| `DINGTALK_WEBHOOK` | empty | Optional DingTalk bot webhook URL. |

## Notes

X may require login or change its page structure at any time. If the page shows a
login wall, set `CHROME_USER_DATA_DIR` and `CHROME_PROFILE_DIRECTORY` to a Chrome
profile where you are already logged in.

Use this project responsibly and make sure your usage complies with X's terms and
applicable laws.

## License

MIT
