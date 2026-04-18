# eterno.nz / GitHub Pages

`curl -I https://eternonz.github.io` returning **404 "Site not found"** means GitHub has **no published Pages build** for this repository yet (not a bug in the HTML files).

## One-time GitHub setup

1. Push this repo to GitHub as **`eternonz.github.io`** under the **`eternonz`** user or org (name must match for `*.github.io`).
2. **Settings → Pages → Build and deployment**
   - **Source:** **GitHub Actions** (not “Deploy from a branch” unless you prefer that and disable this workflow).
3. **Settings → Pages → Custom domain:** `eterno.nz` (optional but required for that hostname).
4. DNS at your registrar:
   - **Apex** `eterno.nz`: GitHub’s [documented A records](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain) for Pages.
   - Or **CNAME** `www` → `eternonz.github.io` if you only use `www`.
5. Merge `.github/workflows/deploy-pages.yml` to **`main`** so the workflow runs once; check **Actions** for a green run, then **Pages** for the live URL.

After the first successful deploy, `https://eternonz.github.io/` and `https://eterno.nz/` (once DNS + custom domain verify) should return **200** with `index.html`.
