# eterno.nz / GitHub Pages

## What a 404 means

If `curl -I https://eternonz.github.io` and `curl -I https://eterno.nz` both return **404** with `server: GitHub.com`, DNS is usually fine but **GitHub has no live Pages deployment** for the user site `eternonz.github.io` (this is not an HTML bug in this folder).

Quick checks:

```bash
curl -sI https://eternonz.github.io | head -5
dig +short eterno.nz A
```

Apex `eterno.nz` should show GitHub’s four `185.199.*` A records (or a CNAME to `eternonz.github.io` for `www`).

## One-time GitHub setup (do this once)

### 1. Repository

Create (or restore) **`https://github.com/eternonz/eternonz.github.io`** — the name **must** be `USERNAME.github.io` for user `eternonz`.

- If the repo is **private**, GitHub Pages for a **user** site requires a **paid** plan (or make the repo **public**, which is the usual choice for a public marketing site).
- Push **this** directory as the default branch **`main`** (includes `index.html`, `assets/`, `CNAME`, `.nojekyll`, and `.github/workflows/deploy-pages.yml`).

Example (after `gh auth login` or SSH is working for `eternonz`):

```bash
cd eternonz.github.io
git remote -v   # should show eternonz/eternonz.github.io
git push -u origin main
```

If the remote repo is empty or new:

```bash
gh repo create eternonz/eternonz.github.io --public --source=. --remote=origin --push
```

(Adjust flags if the repo already exists; use `git remote set-url origin …` if needed.)

### 2. Turn on Pages (pick **one** method)

**Option A — fastest (no Actions):**

1. **Settings → Pages → Build and deployment**
2. **Source:** **Deploy from a branch**
3. Branch **`main`**, folder **`/` (root)**

GitHub will serve `index.html` from the repo root. No workflow run required.

**Option B — GitHub Actions (this repo’s workflow):**

1. **Settings → Pages → Build and deployment**
2. **Source:** **GitHub Actions**
3. Merge the workflow on **`main`**, open **Actions**, confirm **Deploy GitHub Pages** is green.
4. If the first run fails, use **Option A** once to confirm the site works, then switch back to Actions if you prefer.

### 3. Custom domain

1. **Settings → Pages → Custom domain:** `eterno.nz` (save; wait for DNS check if offered).
2. Keep the **`CNAME`** file in this repo (already contains `eterno.nz`).
3. Registrar DNS (if not already set):
   - **Apex** `eterno.nz`: GitHub’s [documented A records](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain) for Pages.
   - **`www`:** CNAME `www` → `eternonz.github.io`.

### 4. Verify

After a successful deploy:

```bash
curl -sI https://eternonz.github.io/ | head -3
curl -sI https://eterno.nz/ | head -3
```

Expect **200** and `content-type: text/html`.

## Source for the React site

The editable marketing app lives under **`eterno-website/`** in the KeaLedger monorepo (`npm run build` → `dist/`). Copy build output into this **`eternonz.github.io`** tree (root `index.html`, `assets/`, `404.html`, etc.) before committing, so what you push matches the Vite build.
