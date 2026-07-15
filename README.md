# ApsilonElectronics

Single-page inventory app for Apsilon Electronics.

## Run locally

This project is static HTML, so no build is needed.

1. Open `Inventory.html` directly in your browser, or
2. Serve the folder with a local server:

```bash
cd /workspaces/ApsilonElectronics
python3 -m http.server 8080
```

Then open `http://localhost:8080`.

## Publish on GitHub Pages

1. Commit and push your files to the `main` branch.
2. In GitHub, go to `Settings` -> `Pages`.
3. Under `Build and deployment`:
	- `Source`: Deploy from a branch
	- `Branch`: `main`
	- `Folder`: `/ (root)`
4. Click `Save`.
5. Wait for deployment to finish. Your site URL will be shown on the same page.

### Notes

- `index.html` redirects to `Inventory.html`, so GitHub Pages loads your app at the root URL.
- Your app depends on the configured Google Apps Script URL in `Inventory.html`.

## One-command publish (automatic)

This repository now includes a workflow at `.github/workflows/deploy-pages.yml`.

After GitHub Pages is enabled once, publish updates with:

```bash
git add . && git commit -m "Update site" && git push origin main
```

Each push to `main` triggers automatic deployment to GitHub Pages.
