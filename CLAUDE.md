# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-page family budget management web app (Italian UI) with no build tools. To run either version, just open the file in a browser.

| File | Theme |
|---|---|
| `index.html` | Dark theme only |
| `index_white.html` | Light/dark switchable (default light) |

## Architecture

**Single-file SPA:** All application logic, styles, and markup are embedded in each HTML file (~640 lines).

**Backend:** Google Apps Script (GAS) acts as a REST-like API backed by Google Sheets. The endpoint URL is hardcoded in `index.html`. API calls use `apiGet(action, params)` and `apiPost(action, payload)`.

**Persistence:** `localStorage` is the primary data store (offline-first). Data syncs to the GAS backend on read/write operations.

| localStorage key | Contents |
|---|---|
| `fb_tipi` | Expense categories (id, nome, colore, emoji) |
| `fb_spese2` | Expenses keyed by year |
| `fb_pw_cache` | Cached password hash |
| `fb_theme` | Active theme (`light` or `dark`), used by `index_white.html` |

**Expense record shape:**
```js
{ id, mese, tipoId, desc, importo, periodo, persona, ts }
```

## Application Tabs

| Tab | Description | Key Functions |
|---|---|---|
| Spese | Add/edit/delete expenses | `addSpesa()`, `editSpesa()`, `delSpesa()`, `renderTable()` |
| Report | Charts & analytics filtered by month/category | `buildReport()` |
| Storico | Year-over-year stacked bar chart | `renderStorico()`, `jumpYear()` |
| Admin | Category CRUD, import/export JSON/CSV | `saveTipo()`, `esportaJSON()`, `importaJSON()`, `esportaCSV()` |
| KPI | Monthly/annual totals summary | `renderKPI()` |

## Session Management

Login is validated against a password fetched from GAS (`getPassword` action). Sessions are stored in `sessionStorage` and expire after 24 hours. Key functions: `checkSession()`, `saveSession()`, `clearSession()`, `doLogin()`, `doLogout()`.

## Theme Switching (`index_white.html` only)

`index_white.html` supports light/dark switching via a dropdown in the topbar. The active theme is stored in `localStorage` (`fb_theme`) and applied as `data-theme="light|dark"` on `<html>`. CSS variables are defined in `:root` (light defaults) and overridden by `[data-theme="dark"]`. Key function: `setTheme(t)`. When editing CSS colors, always update both `:root` and `[data-theme="dark"]` blocks to keep the two themes in sync.

## Charts

Chart.js 4.4.2 is used for all visualizations. Call `killChart(varName)` before re-rendering any chart to avoid canvas reuse errors.

## Key Utilities

- `uid()` — generate unique IDs for new records
- `fmt(n)` — format numbers as currency (€)
- `getTipo(id)` — look up a category by ID
- `toast(msg)` — show a Bootstrap toast notification
- `speseDiAnno(year)` — get expenses for a specific year
- `tutteLeSpese()` — get all expenses across all years
- `anniUsati()` — list years that have expense data
