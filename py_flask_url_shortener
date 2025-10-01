"""
Tiny URL shortener.
Run:
  python -m venv .venv && . .venv/bin/activate   # Windows: .venv\Scripts\activate
  pip install flask
  python py_flask_url_shortener.py
Then open: http://127.0.0.1:5000
"""
from flask import Flask, request, redirect, render_template_string, abort
import os, json, string, random

app = Flask(__name__)
PATH = "links.json"
def load():
    if not os.path.exists(PATH): return {}
    with open(PATH, encoding="utf-8") as f: return json.load(f)
def save(data):
    with open(PATH, "w", encoding="utf-8") as f: json.dump(data, f, indent=2)

HTML = """
<!doctype html><meta charset="utf-8">
<title>Shorty</title>
<style>
  body{font:16px/1.6 system-ui;background:#0b1220;color:#e6eef8;margin:0;display:grid;place-items:center;min-height:100vh}
  .card{background:#121a2b;border:1px solid #1e2a44;border-radius:16px;padding:22px;max-width:640px;width:92%}
  input{width:100%;padding:.6rem;border-radius:10px;border:1px solid #2a3c63;background:#12203a;color:#eef3fb}
  button{margin-top:8px;padding:.6rem .9rem;border-radius:10px;border:1px solid #2a3c63;background:#1d74e4;color:#fff}
  a{color:#7bb0ff}
  .list{margin-top:14px}
  .item{background:#0f1728;padding:.5rem .7rem;border-radius:10px;margin:.35rem 0;border:1px solid #1a2742}
</style>
<div class="card">
  <h1>Shorty</h1>
  <form method="post">
    <input name="url" placeholder="https://example.com/very/long/link" required>
    <button>Create</button>
  </form>
  {% if new_code %}<p>New: <a href="/{{new_code}}" target="_blank">{{ request.host_url ~ new_code }}</a></p>{% endif %}
  <div class="list">
    <h3>Existing</h3>
    {% for code, url in data.items() %}
      <div class="item"><a href="/{{code}}" target="_blank">{{ request.host_url ~ code }}</a> → <a href="{{url}}" target="_blank">{{url}}</a></div>
    {% else %}
      <p class="item">No links yet.</p>
    {% endfor %}
  </div>
</div>
"""

def gen_code(n=6):
    alphabet = string.ascii_letters + string.digits
    return "".join(random.choice(alphabet) for _ in range(n))

@app.route("/", methods=["GET","POST"])
def index():
    data = load()
    new_code = None
    if request.method == "POST":
        url = request.form.get("url","").strip()
        if not (url.startswith("http://") or url.startswith("https://")):
            url = "https://" + url
        code = gen_code()
        while code in data: code = gen_code()
        data[code] = url; save(data)
        new_code = code
    return render_template_string(HTML, data=data, new_code=new_code)

@app.get("/<code>")
def go(code):
    data = load()
    url = data.get(code)
    if not url: abort(404)
    return redirect(url, code=302)

if __name__ == "__main__":
    app.run(debug=True)
