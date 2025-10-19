# Log Parser & Alert Script (Mini Cybersecurity Project)

A small, hands-on Python project that parses sample SSH and Apache logs, applies detection rules, and triggers simple threshold-based alerts.
Use it to learn regex matching, log timestamp parsing, and basic alert aggregation.

## Features
- Supports **Apache access logs** and **Linux auth (sshd)** logs
- Regex-based **rules** (configurable in `config.json`)
- **Sliding window** thresholding (e.g., 5 failed SSH logins by the same IP in 5 minutes)
- Outputs:
  - `output/findings.csv`: all matches with timestamp, IP, user, rule_id
  - `output/alerts.json`: threshold-triggered alerts

## Repo Structure
```
log-parser/
├── config.json
├── log_parser.py
├── sample_logs/
│   ├── apache_access.log
│   └── auth.log
└── output/            # results written here after running
```

## Quick Start
1) Ensure you have Python 3.9+
2) From the `log-parser` folder, run:
```bash
python3 log_parser.py --config config.json --report_csv output/findings.csv --alerts_json output/alerts.json --verbose
```
3) Open `output/findings.csv` and `output/alerts.json` to review results.

## Config
Edit `config.json` to add/remove rules or thresholds. Example threshold included:
```json
{
  "name": "SSH_FAIL_per_ip_per_5min",
  "rule_id":"SSH_FAIL",
  "group_by":"ip",
  "window_minutes":5,
  "count":5,
  "action":"ALERT"
}
```

## Notes
- Timestamps for Apache are parsed from `[dd/Mon/yyyy:HH:MM:SS -TZ]`.
- Timestamps for SSH are assumed to be current year and treated as naive UTC for simplicity.
- The sample logs are intentionally small. Replace with real logs to experiment (be mindful of sensitive data).

## Next Steps (Ideas)
- Add email/webhook notifications for alerts
- Add GeoIP lookup (requires a library and DB)
- Add JSON/YAML rule loading with nested field extraction
- Package as a CLI (`pipx`-installable) with tests
