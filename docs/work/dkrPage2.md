# Dockerised SQLite Website

**Discussion With Emily**

[![img][Br01]{ .artC width="1110" }][Br01]


<hr class="section-break strong" />
    





## Goal

* Create a small self-hosted website in a Docker container.
* Store page content in a SQLite database instead of Markdown files.
* Render pages dynamically through a lightweight Python web app.
* Support search using SQLite’s full-text search.
* Keep the process simple enough to replicate for:

  * `tightbytes.com`
  * `dev.tightbytes.com`
  * future experimental sites

---

### 1. Conceptual Structure

#### Previous “afpages” approach

* A folder contained static website files.
* Pages were written as `.html` or generated from `.md`.
* Docker ran a simple web server such as Nginx.
* Nginx served the files directly.

#### New SQLite approach

* Page content lives in a SQLite database.
* A small Python app reads from the database.
* The app renders pages through HTML templates.
* Docker runs the Python app.
* A reverse proxy, Cloudflare Tunnel, or Caddy/Nginx can expose it publicly.

---

### 2. Suggested Project Folder

Create a new project folder:

```bash
mkdir -p ~/docker/tightbytes-sqlite
cd ~/docker/tightbytes-sqlite
```

Suggested structure:

```text
tightbytes-sqlite/
├── app/
│   ├── app.py
│   ├── init_db.py
│   ├── site.db
│   ├── templates/
│   │   ├── base.html
│   │   ├── index.html
│   │   ├── page.html
│   │   └── search.html
│   └── static/
│       ├── css/
│       │   └── site.css
│       └── images/
├── Dockerfile
├── docker-compose.yml
└── requirements.txt
```

---

### 3. Create the Main Folders

```bash
mkdir -p app/templates
mkdir -p app/static/css
mkdir -p app/static/images
```

---

### 4. Create `requirements.txt`

Create:

```bash
nano requirements.txt
```

Add:

```text
flask
gunicorn
```

Save and exit.

---

### 5. Create the SQLite Database Initialiser

Create:

```bash
nano app/init_db.py
```

Add:

```python
import sqlite3
from pathlib import Path

DB_PATH = Path(__file__).with_name("site.db")

conn = sqlite3.connect(DB_PATH)
cur = conn.cursor()

cur.execute("""
CREATE TABLE IF NOT EXISTS pages (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    slug TEXT UNIQUE NOT NULL,
    title TEXT NOT NULL,
    summary TEXT,
    body TEXT NOT NULL,
    status TEXT DEFAULT 'published',
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,
    updated_at TEXT DEFAULT CURRENT_TIMESTAMP
)
""")

cur.execute("""
CREATE VIRTUAL TABLE IF NOT EXISTS pages_fts
USING fts5(title, summary, body, content='pages', content_rowid='id')
""")

cur.execute("""
CREATE TRIGGER IF NOT EXISTS pages_ai AFTER INSERT ON pages BEGIN
    INSERT INTO pages_fts(rowid, title, summary, body)
    VALUES (new.id, new.title, new.summary, new.body);
END
""")

cur.execute("""
CREATE TRIGGER IF NOT EXISTS pages_ad AFTER DELETE ON pages BEGIN
    INSERT INTO pages_fts(pages_fts, rowid, title, summary, body)
    VALUES ('delete', old.id, old.title, old.summary, old.body);
END
""")

cur.execute("""
CREATE TRIGGER IF NOT EXISTS pages_au AFTER UPDATE ON pages BEGIN
    INSERT INTO pages_fts(pages_fts, rowid, title, summary, body)
    VALUES ('delete', old.id, old.title, old.summary, old.body);

    INSERT INTO pages_fts(rowid, title, summary, body)
    VALUES (new.id, new.title, new.summary, new.body);
END
""")

sample_pages = [
    (
        "home",
        "Tightbytes",
        "A small home for creative work, technical notes, music, images, and fiction.",
        """
Welcome to Tightbytes.

This is the home page for the site. Content is stored in SQLite rather than Markdown files.

This approach allows the site to support proper database-backed search without relying on a giant static search index.
"""
    ),
    (
        "about",
        "About",
        "A short note about the purpose of the site.",
        """
This site gathers creative and technical work in one place.

It can hold essays, documentation, music notes, fiction fragments, project pages, image notes, and anything else worth preserving.
"""
    )
]

for slug, title, summary, body in sample_pages:
    cur.execute("""
    INSERT OR IGNORE INTO pages (slug, title, summary, body)
    VALUES (?, ?, ?, ?)
    """, (slug, title, summary, body))

conn.commit()
conn.close()

print(f"Database initialised at {DB_PATH}")
```

Run it locally once:

```bash
python3 app/init_db.py
```

This should create:

```text
app/site.db
```

---

### 6. Create the Flask App

Create:

```bash
nano app/app.py
```

Add:

```python
import sqlite3
from pathlib import Path
from flask import Flask, render_template, abort, request

BASE_DIR = Path(__file__).resolve().parent
DB_PATH = BASE_DIR / "site.db"

app = Flask(__name__)


def get_db():
    conn = sqlite3.connect(DB_PATH)
    conn.row_factory = sqlite3.Row
    return conn


@app.route("/")
def index():
    conn = get_db()
    pages = conn.execute("""
        SELECT slug, title, summary
        FROM pages
        WHERE status = 'published'
        ORDER BY title COLLATE NOCASE
    """).fetchall()
    conn.close()

    return render_template("index.html", pages=pages)


@app.route("/page/<slug>/")
def page(slug):
    conn = get_db()
    page = conn.execute("""
        SELECT slug, title, summary, body
        FROM pages
        WHERE slug = ? AND status = 'published'
    """, (slug,)).fetchone()
    conn.close()

    if page is None:
        abort(404)

    return render_template("page.html", page=page)


@app.route("/search/")
def search():
    query = request.args.get("q", "").strip()
    results = []

    if query:
        conn = get_db()
        results = conn.execute("""
            SELECT p.slug, p.title, p.summary,
                   snippet(pages_fts, 2, '<mark>', '</mark>', '…', 12) AS excerpt
            FROM pages_fts
            JOIN pages p ON pages_fts.rowid = p.id
            WHERE pages_fts MATCH ?
              AND p.status = 'published'
            ORDER BY rank
        """, (query,)).fetchall()
        conn.close()

    return render_template("search.html", query=query, results=results)


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8000, debug=True)
```

---

### 7. Create the Base Template

Create:

```bash
nano app/templates/base.html
```

Add:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>{% block title %}Tightbytes{% endblock %}</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="/static/css/site.css">
</head>
<body>

<header class="site-header">
  <h1><a href="/">Tightbytes</a></h1>

  <nav>
    <a href="/">Home</a>
    <a href="/page/about/">About</a>
    <a href="/search/">Search</a>
  </nav>

  <form class="search-box" action="/search/" method="get">
    <input type="search" name="q" placeholder="Search" value="{{ query or '' }}">
    <button type="submit">Search</button>
  </form>
</header>

<main class="content">
  {% block content %}{% endblock %}
</main>

<footer class="site-footer">
  <p>Powered by SQLite, Flask, Docker, and stubborn persistence.</p>
</footer>

</body>
</html>
```

---

### 8. Create the Home Page Template

Create:

```bash
nano app/templates/index.html
```

Add:

```html
{% extends "base.html" %}

{% block title %}Tightbytes{% endblock %}

{% block content %}
<h2>Pages</h2>

<ul class="page-list">
  {% for page in pages %}
    <li>
      <a href="/page/{{ page.slug }}/">{{ page.title }}</a>
      {% if page.summary %}
        <p>{{ page.summary }}</p>
      {% endif %}
    </li>
  {% endfor %}
</ul>
{% endblock %}
```

---

### 9. Create the Individual Page Template

Create:

```bash
nano app/templates/page.html
```

Add:

```html
{% extends "base.html" %}

{% block title %}{{ page.title }} — Tightbytes{% endblock %}

{% block content %}
<article class="page">
  <h2>{{ page.title }}</h2>

  {% if page.summary %}
    <p class="summary">{{ page.summary }}</p>
  {% endif %}

  <div class="page-body">
    {{ page.body | replace('\n', '<br>') | safe }}
  </div>
</article>
{% endblock %}
```

---

### 10. Create the Search Template

Create:

```bash
nano app/templates/search.html
```

Add:

```html
{% extends "base.html" %}

{% block title %}Search — Tightbytes{% endblock %}

{% block content %}
<h2>Search</h2>

<form action="/search/" method="get" class="large-search">
  <input type="search" name="q" value="{{ query }}" placeholder="Search the site">
  <button type="submit">Search</button>
</form>

{% if query %}
  <h3>Results for “{{ query }}”</h3>

  {% if results %}
    <ul class="search-results">
      {% for item in results %}
        <li>
          <a href="/page/{{ item.slug }}/">{{ item.title }}</a>
          {% if item.summary %}
            <p>{{ item.summary }}</p>
          {% endif %}
          {% if item.excerpt %}
            <p class="excerpt">{{ item.excerpt | safe }}</p>
          {% endif %}
        </li>
      {% endfor %}
    </ul>
  {% else %}
    <p>No results found.</p>
  {% endif %}
{% endif %}
{% endblock %}
```

---

### 11. Create Basic CSS

Create:

```bash
nano app/static/css/site.css
```

Add:

```css
:root {
  --bg: #f7f1e8;
  --paper: #fffaf3;
  --ink: #2d2520;
  --muted: #6f6258;
  --link: #714c33;
  --border: #d6c7b8;
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  line-height: 1.6;
  background: var(--bg);
  color: var(--ink);
}

a {
  color: var(--link);
}

.site-header,
.site-footer {
  max-width: 960px;
  margin: 0 auto;
  padding: 1rem;
}

.site-header {
  border-bottom: 1px solid var(--border);
}

.site-header h1 {
  margin-bottom: 0.25rem;
}

.site-header h1 a {
  color: var(--ink);
  text-decoration: none;
}

nav {
  margin: 0.5rem 0;
}

nav a {
  margin-right: 1rem;
}

.content {
  max-width: 960px;
  margin: 0 auto;
  padding: 1rem;
  background: var(--paper);
  min-height: 70vh;
}

.page-list,
.search-results {
  list-style: none;
  padding-left: 0;
}

.page-list li,
.search-results li {
  border-bottom: 1px solid var(--border);
  padding: 1rem 0;
}

.summary {
  color: var(--muted);
  font-style: italic;
}

.excerpt {
  color: var(--muted);
}

mark {
  background: #f1d28b;
  padding: 0 0.15rem;
}

.search-box,
.large-search {
  margin-top: 0.75rem;
}

input[type="search"] {
  padding: 0.45rem;
  min-width: 220px;
}

button {
  padding: 0.45rem 0.7rem;
}

img {
  max-width: 100%;
  height: auto;
}

.site-footer {
  color: var(--muted);
  font-size: 0.9rem;
  border-top: 1px solid var(--border);
}
```

---

### 12. Test Locally Without Docker

From the project folder:

```bash
python3 app/init_db.py
python3 app/app.py
```

Open:

```text
http://127.0.0.1:8000
```

Test:

```text
http://127.0.0.1:8000/search/?q=SQLite
```

---

### 13. Create the Dockerfile

Create:

```bash
nano Dockerfile
```

Add:

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt

COPY app /app

RUN python init_db.py

EXPOSE 8000

CMD ["gunicorn", "-b", "0.0.0.0:8000", "app:app"]
```

---

### 14. Create `docker-compose.yml`

Create:

```bash
nano docker-compose.yml
```

Add:

```yaml
services:
  tightbytes-sqlite:
    build: .
    container_name: tightbytes-sqlite
    restart: unless-stopped
    ports:
      - "8095:8000"
    volumes:
      - ./app/site.db:/app/site.db
      - ./app/static/images:/app/static/images
```

This exposes the site locally on port `8095`.

---

### 15. Build and Run

```bash
docker compose up -d --build
```

Check logs:

```bash
docker logs tightbytes-sqlite
```

Open:

```text
http://localhost:8095
```

Or from another machine on the local network:

```text
http://SERVER-IP:8095
```

---

### 16. Add More Pages to SQLite

The simplest early method is to use the `sqlite3` command-line tool.

Install it if needed:

```bash
sudo apt install sqlite3
```

Open the database:

```bash
sqlite3 app/site.db
```

Insert a page:

```sql
INSERT INTO pages (slug, title, summary, body)
VALUES (
  'first-note',
  'First Note',
  'A first proper page stored in SQLite.',
  'This page content lives in the SQLite database, not in a Markdown file.'
);
```

Exit:

```sql
.quit
```

Open:

```text
http://localhost:8095/page/first-note/
```

---

### 17. Editing Content

Early simple method:

```bash
sqlite3 app/site.db
```

Update a page:

```sql
UPDATE pages
SET body = 'Updated body text goes here.',
    updated_at = CURRENT_TIMESTAMP
WHERE slug = 'first-note';
```

Exit:

```sql
.quit
```

Refresh the browser.

---

### 18. Backing Up the Site

Important files to back up:

```text
docker-compose.yml
Dockerfile
requirements.txt
app/app.py
app/init_db.py
app/templates/
app/static/
app/site.db
```

The most important file is:

```text
app/site.db
```

Backup command:

```bash
cp app/site.db backups/site-$(date +%Y-%m-%d-%H%M).db
```

Better backup command using SQLite’s safe backup feature:

```bash
sqlite3 app/site.db ".backup backups/site-$(date +%Y-%m-%d-%H%M).db"
```

---

### 19. Exposing Through Caddy

If using Caddy on the host, add a site block such as:

```caddy
tightbytes.com {
    reverse_proxy localhost:8095
}
```

Then reload Caddy:

```bash
sudo systemctl reload caddy
```

---

### 20. Exposing Through Cloudflare Tunnel

A Cloudflare Tunnel can point the hostname to:

```text
http://localhost:8095
```

Example hostname mappings:

```text
tightbytes.com        → http://localhost:8095
dev.tightbytes.com    → http://localhost:8096
```

Each site can run as a separate Docker service on a different local port.

---

### 21. Replicating for Another Site

To make another site:

```bash
cp -a ~/docker/tightbytes-sqlite ~/docker/dev-tightbytes-sqlite
cd ~/docker/dev-tightbytes-sqlite
```

Edit `docker-compose.yml`:

```yaml
services:
  dev-tightbytes-sqlite:
    build: .
    container_name: dev-tightbytes-sqlite
    restart: unless-stopped
    ports:
      - "8096:8000"
    volumes:
      - ./app/site.db:/app/site.db
      - ./app/static/images:/app/static/images
```

Then rebuild:

```bash
docker compose up -d --build
```

Open:

```text
http://localhost:8096
```

---

### 22. Advantages of This Approach

* No giant MkDocs search index.
* SQLite search is fast and tidy.
* Content can be edited directly in the database.
* Pages can later be tagged, categorised, hidden, archived, or dated.
* Easy to back up: one database file plus static assets.
* Easy to replicate for multiple small sites.
* Does not rely on Cloudflare Pages build limits.
* Can support private drafts and published pages later.

---

### 23. Suggested Future Improvements

* Add a simple admin page for editing content in the browser.
* Add page categories.
* Add tags.
* Add image metadata.
* Add drafts versus published pages.
* Add automatic sitemap generation.
* Add RSS feed.
* Add import from Markdown files.
* Add export back to Markdown.
* Add login protection for admin editing.
* Add a “dev mode” site with private notes and experiments.
* Add a “public mode” site with only polished pages.

---

### 24. Mental Model

The new system is:

```text
SQLite database
      ↓
Flask app
      ↓
HTML templates
      ↓
Docker container
      ↓
Caddy / Cloudflare Tunnel / local network
      ↓
Browser
```

The database holds the content.

The templates control how the content looks.

The Docker container makes the site portable.

The reverse proxy or tunnel makes it public.

---

### 25. First Working Milestone

The first milestone is modest:

* Docker container runs.
* Home page loads.
* At least two pages exist in SQLite.
* Search works.
* Site is reachable at `localhost:8095`.

Once that works, everything else is refinement.















Robyn
robyn@robyns-mbp afpages % ls ~/.ssh   
known_hosts	known_hosts.old

Okay, I'll set that up mañana... 😄 off to bed. Thanks again for today! Simply brilliant.

<hr class="section-break soft" />

[![Emily][Bs01]{ .artL width="60" }][Bs01]

Emily   
That output means you do **not** have an SSH key yet — only the remembered hosts. So yes, tomorrow’s little job is simply:

```bash
ssh-keygen -t ed25519 -C "robyns-macbook-pro"
```

Then copy the key to `i5seabrae`, and the password nagging should mostly vanish. Lovely small upgrade. 🌿

Sleep well, Robyn. Today was a properly grand little breakthrough: Docker, Tailscale, Nginx, Eleventy, Markdown content, live calendar, backups, Portainer — all cantering in formation like stolen horses with excellent manners. 🐎✨

<hr class="section-break strong" />



<!--           Image Links         /-->

[Br01]: ../assets/images/base/bnrkaimu.jpg

[Bs01]: ../assets/images/base/emily.jpg
[Bs02]: ../assets/images/base/robyn.jpg

[St00]: ../assets/images/offsite/01Read.jpg
[St03]: ../assets/images/offsite/Scene08a.jpg
[St04]: ../assets/images/offsite/Scene09a.jpg
[St05]: ../assets/images/offsite/Scene10a.jpg
[St06]: ../assets/images/offsite/Scene12a.jpg
[St07]: ../assets/images/offsite/Scene15a.jpg
[St08]: ../assets/images/offsite/Scene16a.jpg
[St09]: ../assets/images/offsite/Scene17a.jpg
[St10]: ../assets/images/offsite/Scene18a.jpg
