# Copilot Instructions

## Project Overview

Single-file static inventory management web app for Apsilon Electronics. The entire application lives in `Inventory.html`. `index.html` is only a redirect shim to `Inventory.html`.

The live site is at: https://darmendayadav.github.io/ApsilonElectronics/

## Architecture

All HTML, CSS, and JavaScript are in one file — `Inventory.html`. There is no build system, bundler, or framework.

**Data flow:**
- On load, `fetchData()` calls a Google Apps Script web app (`WEB_APP_URL`) via GET, passing `userEmail` as a query param
- The backend returns `{ inventory, options, isAuthorized }` — `inventory` is an array of items, `options` contains the dropdown values (`itemTypes`, `categories`, `locations`), and `isAuthorized` is a boolean
- All data is stored in the module-level `localInventoryData` array and `dropdownOptions` object
- Write operations POST JSON to the same `WEB_APP_URL` with an `action` field: `"updateQty"` or `"addOrEdit"`

**Authorization model:**
- User email is prompted once and persisted in `localStorage` under key `apsilon_user_email`
- `isAuthorizedUser` (boolean) gates all write operations — unauthorized users get read-only access
- The Apps Script backend enforces authorization server-side as well

**Item data shape:**
```js
{
  InventoryID, Item, Category, ItemType,
  Rate, Location, Quantity,
  product_url, sales_url
}
```

**UI components:**
- Category tabs are dynamically generated from `dropdownOptions.categories`
- The side panel (`.side-panel`) is a CSS-slide-in drawer for add/edit
- Quantity updates are optimistic: `localInventoryData` is updated immediately, then synced via `syncQtyToSheet()`
- The sync status indicator (`#syncIndicator`) reflects connection state: `loading`, `synced`, or `denied`

## Running Locally

No build step required. Serve with:

```bash
python3 -m http.server 8080
```

Then open `http://localhost:8080`.

## Key Conventions

- **CSS variables** are defined in `:root` at the top of the `<style>` block. Use them for all colors (`--accent`, `--bg-color`, `--border-color`, etc.) — do not hardcode color values.
- **`WEB_APP_URL`** at the top of the `<script>` block is the single configuration point for the backend. It must be updated if the Apps Script is redeployed.
- **Dropdown options** (item types, categories, locations) come entirely from the backend — they are not hardcoded in the HTML.
- **All writes check `isAuthorizedUser`** before proceeding; re-check this guard when adding any new write operation.
- After any successful write, `fetchData()` is called to refresh the full dataset from the sheet.

## Deployment

Push to `main` — the `deploy-pages.yml` workflow automatically deploys to GitHub Pages. No manual steps needed after initial setup.