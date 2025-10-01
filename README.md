# Flask URL Shortener

A tiny URL shortener web app written in Python with Flask.

## Features
- Create short codes for long URLs
- List all existing short links
- Redirects automatically
- Stores mappings in a JSON file (no database needed)

## Run
```bash
python -m venv .venv && . .venv/bin/activate  # (Windows: .venv\Scripts\activate)
pip install flask
python py_flask_url_shortener.py
# Open http://127.0.0.1:5000
