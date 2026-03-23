# SFTP Deploy: Ecosystem Report Pages + News & Resources Update

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Deploy the updated News & Resources page and three new pages (ecosystem-report, press-release-ecosystem-report, ecosystem-report-faq) from the GitHub repo to njcivicinfo.org via SFTP, without touching any WordPress files.

**Architecture:** Pull the latest source from GitHub (the remote `_config.yml` uses `baseurl: "/njcivicinfo"` for GitHub Pages) → create a production config override setting `baseurl: ""` → build Jekyll locally → SFTP only the specific page directories + shared assets to the server's document root. WordPress's `.htaccess` serves static files/dirs before WP routing, so uploaded static pages take precedence automatically.

**Tech Stack:** Jekyll 4.3, Ruby 2.6.10, standard `sftp`/`scp` (macOS), Bundler

---

## File Map

| Action | Path |
|--------|------|
| Create | `docs/_config.production.yml` — production URL overrides |
| Build output (upload) | `docs/_site/news-resources/` |
| Build output (upload) | `docs/_site/ecosystem-report/` |
| Build output (upload) | `docs/_site/press-release-ecosystem-report/` |
| Build output (upload) | `docs/_site/ecosystem-report-faq/` |
| Build output (upload) | `docs/_site/assets/` (CSS + images — shared by all static pages) |
| **NEVER TOUCH** | `wp-admin/`, `wp-content/`, `wp-config.php`, `.htaccess`, `index.php` |

---

## SFTP Credentials

- **Host:** 37.27.121.163
- **Port:** 4377
- **User:** njcivicinfo
- **Pass:** (in 1Password / session context)

---

### Task 0: Accept the server's SSH host key (one-time setup)

This must happen before any `sftp` or `scp` command or they will hang waiting for interactive input.

- [ ] **Step 1: Add host key to known_hosts**

```bash
ssh-keyscan -p 4377 37.27.121.163 >> ~/.ssh/known_hosts
```

Expected: One or more lines printed (key fingerprints). No error. If `ssh-keyscan` hangs for more than 10 seconds, the host is unreachable — stop and report to the user.

---

### Task 1: Pull latest from GitHub

**Files:**
- Modify (local state): `~/Desktop/Playground/njcivicinfo/`

- [ ] **Step 1: Pull latest commits**

```bash
cd ~/Desktop/Playground/njcivicinfo
git pull origin Main
```

Expected: 4 new commits pulled. The output should mention files including `ecosystem-report.html`, `press-release-ecosystem-report.html`, `ecosystem-report-faq.html`.

- [ ] **Step 2: Verify the new pages exist**

```bash
ls docs/ecosystem-report.html docs/press-release-ecosystem-report.html docs/ecosystem-report-faq.html
```

Expected: All three files listed with no errors. **If any file is missing, stop and report to the user before proceeding.** Do not continue to Task 2.

---

### Task 2: Create production Jekyll config

**Files:**
- Create: `docs/_config.production.yml`

After `git pull`, `docs/_config.yml` will have `baseurl: "/njcivicinfo"` and `url: "https://madi-mccool.github.io"` (GitHub Pages settings). The production override below replaces those values for the local build so all links resolve correctly on njcivicinfo.org.

- [ ] **Step 1: Create the override config**

```bash
cat > ~/Desktop/Playground/njcivicinfo/docs/_config.production.yml << 'EOF'
url: "https://njcivicinfo.org"
baseurl: ""
EOF
```

- [ ] **Step 2: Verify it exists**

```bash
cat ~/Desktop/Playground/njcivicinfo/docs/_config.production.yml
```

Expected:
```
url: "https://njcivicinfo.org"
baseurl: ""
```

---

### Task 3: Install Jekyll dependencies and build the site

**Files:**
- Build target: `docs/_site/`

- [ ] **Step 1: Update Bundler and ensure the new version is on PATH**

```bash
gem install bundler --user-install
export PATH="$(ruby -e 'puts Gem.user_dir')/bin:$PATH"
bundler --version
```

Expected: Bundler version 2.x.x printed. If the install fails with a permissions error, try without `--user-install`:
```bash
sudo gem install bundler
```

- [ ] **Step 2: Install gems**

```bash
cd ~/Desktop/Playground/njcivicinfo/docs
bundle install
```

Expected: All gems installed with no errors. If you see `Could not find compatible versions`, delete the lock file and retry:
```bash
rm Gemfile.lock
bundle install
```

- [ ] **Step 3: Build with production config**

```bash
cd ~/Desktop/Playground/njcivicinfo/docs
bundle exec jekyll build --config _config.yml,_config.production.yml
```

Expected: Terminal output ending with `done in X seconds` or `Build complete!`. The `_site/` folder is rebuilt.

- [ ] **Step 4: Confirm new page directories were built**

```bash
ls _site/ecosystem-report/index.html \
   _site/press-release-ecosystem-report/index.html \
   _site/ecosystem-report-faq/index.html \
   _site/news-resources/index.html
```

Expected: All four files listed. **If any are missing, the build failed — stop and report before proceeding.**

- [ ] **Step 5: Verify production URLs in built output**

```bash
grep "assets/css" _site/ecosystem-report/index.html
```

Expected:
```html
<link rel="stylesheet" href="/assets/css/style.css">
```
There must be NO `/njcivicinfo/` prefix in this path. If you see `/njcivicinfo/assets/...`, the production config was not applied — re-run Step 3 and check that `_config.production.yml` exists.

---

### Task 4: Explore server structure via SFTP

Before uploading, confirm the document root, note which static dirs already exist, and pre-create any missing target directories.

- [ ] **Step 1: Connect and find the document root**

```bash
sftp -P 4377 njcivicinfo@37.27.121.163
```

At the `sftp>` prompt:
```
ls
```

Look for `public_html/`, `www/`, or `html/`. Enter it:
```
cd public_html
ls
```

Expected: WordPress files present — `wp-admin/`, `wp-content/`, `wp-config.php`, `.htaccess`, `index.php`. Also note any existing static directories (e.g., `news-resources/`, `assets/`).

- [ ] **Step 2: Record the document root full path**

```
pwd
```

Note this path exactly (e.g., `/home/njcivicinfo/public_html`). This is `SERVER_ROOT` used in Task 5.

- [ ] **Step 3: Check .htaccess will allow static files**

```
get .htaccess /tmp/server-htaccess.txt
```

Then in a separate terminal (keep sftp open):
```bash
grep "REQUEST_FILENAME" /tmp/server-htaccess.txt
```

Expected: Two lines containing `!-f` and `!-d`. If missing, flag to the user — static files may not take precedence over WordPress routing.

- [ ] **Step 4: Pre-create all target directories**

Back at the `sftp>` prompt (still inside `public_html`):
```
mkdir assets
mkdir news-resources
mkdir ecosystem-report
mkdir press-release-ecosystem-report
mkdir ecosystem-report-faq
```

Ignore errors for directories that already exist. Then exit:
```
exit
```

---

### Task 5: Upload pages via SFTP

Replace `SERVER_ROOT` with the path confirmed in Task 4 Step 2 (e.g., `/home/njcivicinfo/public_html`).

**CRITICAL SAFETY RULES:**
- Only upload into these five subdirectories: `news-resources/`, `ecosystem-report/`, `press-release-ecosystem-report/`, `ecosystem-report-faq/`, `assets/`
- NEVER upload to: `wp-admin/`, `wp-content/`, `wp-config.php`, `.htaccess`, `index.php`, or the document root directly

- [ ] **Step 1: Upload assets (CSS + images)**

Must go first — pages reference these files.

```bash
scp -P 4377 -r \
  ~/Desktop/Playground/njcivicinfo/docs/_site/assets/ \
  njcivicinfo@37.27.121.163:SERVER_ROOT/assets/
```

**Note:** Overwrites existing Jekyll assets. Safe — WP theme assets live in `wp-content/themes/`, not at root `/assets/`.

- [ ] **Step 2: Upload news-resources page**

```bash
scp -P 4377 -r \
  ~/Desktop/Playground/njcivicinfo/docs/_site/news-resources/ \
  njcivicinfo@37.27.121.163:SERVER_ROOT/news-resources/
```

- [ ] **Step 3: Upload ecosystem-report page**

```bash
scp -P 4377 -r \
  ~/Desktop/Playground/njcivicinfo/docs/_site/ecosystem-report/ \
  njcivicinfo@37.27.121.163:SERVER_ROOT/ecosystem-report/
```

- [ ] **Step 4: Upload press-release-ecosystem-report page**

```bash
scp -P 4377 -r \
  ~/Desktop/Playground/njcivicinfo/docs/_site/press-release-ecosystem-report/ \
  njcivicinfo@37.27.121.163:SERVER_ROOT/press-release-ecosystem-report/
```

- [ ] **Step 5: Upload ecosystem-report-faq page**

```bash
scp -P 4377 -r \
  ~/Desktop/Playground/njcivicinfo/docs/_site/ecosystem-report-faq/ \
  njcivicinfo@37.27.121.163:SERVER_ROOT/ecosystem-report-faq/
```

---

### Task 6: Verify live pages

- [ ] **Step 1: Check news-resources loads with updated content**

Open in browser: `https://njcivicinfo.org/news-resources/`

Verify: The March 23, 2026 Ecosystem Report press release appears. CSS loads (Mulish font, NJCIC branding).

- [ ] **Step 2: Check new ecosystem-report page**

Open in browser: `https://njcivicinfo.org/ecosystem-report/`

Verify: Page loads with report content. Navigation links go to correct paths (no `/njcivicinfo/` prefix).

- [ ] **Step 3: Check new press-release page**

Open in browser: `https://njcivicinfo.org/press-release-ecosystem-report/`

Verify: Press release content present. Any "Read the full report" link points to `/ecosystem-report/` (not `/njcivicinfo/ecosystem-report/`).

- [ ] **Step 4: Check ecosystem-report-faq page**

Open in browser: `https://njcivicinfo.org/ecosystem-report-faq/`

Verify: FAQ page loads correctly.

- [ ] **Step 5: Spot-check WordPress pages are untouched**

Open in browser: `https://njcivicinfo.org/` and `https://njcivicinfo.org/grantmaking/` and `https://njcivicinfo.org/wp-admin/`

Verify: Homepage and Grantmaking still load as WordPress pages. WP admin login is accessible.

---

## Rollback Plan

If a page looks broken after upload:

**Option A — Delete the static directory (restores WP routing to that slug):**
```bash
ssh -p 4377 njcivicinfo@37.27.121.163 "rm -rf ~/public_html/news-resources"
```
Replace `news-resources` with whichever page to roll back. WordPress will then serve its own page for that slug again (if a WP page with that slug existed).

**Option B — Fix broken links (if `/njcivicinfo/` prefixes appear in links):**
The production config wasn't applied during the build. Re-run Task 3 in full, then re-run Task 5 for the affected pages only.
