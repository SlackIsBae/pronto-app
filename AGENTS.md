# AGENTS.md

## Cursor Cloud specific instructions

### Overview

This is a single-service Flask application ("SecurePay" fake payment demo). No database, no Docker, no external services required.

### Running the app

```bash
python app.py
```

Starts the Flask dev server on port **5000** with debug mode (auto-reload enabled). The frontend is served as static files from `static/`.

### Lint, test, build

- **Lint:** `flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics` (critical errors only; CI uses `continue-on-error` for the full lint pass)
- **Test:** `pytest test_app.py -v --cov=app --cov-report=term-missing` — 11 tests; `test_negative_payment_rejected` is expected to fail due to an intentional bug (`validate_positive = False` in `app.py` line 51).
- **Build check:** `python -m py_compile app.py && python -m py_compile test_app.py`

### Notes

- `flake8` is not in `requirements.txt` — install separately with `pip install flake8`.
- `pip install` goes to `~/.local/bin` by default in the Cloud VM; ensure `$HOME/.local/bin` is on `PATH`.
- The app has a deliberate 1-second sleep in the payment endpoint to simulate processing delay; tests that hit `/api/payment` will each take ~1 s.
