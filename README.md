# Caminos de Agua — Water Systems Maps & Dashboards

Three interactive tools showing water technology installations and community engagement across rural Guanajuato, Mexico.

| Tool | Live URL |
|------|----------|
| Community Map | https://Caminos-de-Agua.github.io/water-systems-map |
| Technology Registry | https://Caminos-de-Agua.github.io/water-systems-map/registry.html |
| Procesos Dashboard | https://Caminos-de-Agua.github.io/water-systems-map/procesos.html |

---

## Map 1: Water Systems by Community (`index.html`)

Historical overview of all rainwater harvesting, composting toilet, and Nuestra Agua installations by community, 2015–present.

**What it shows:**
- One pin per community, sized by number of systems installed
- Rainwater harvesting systems (ferrocement 12,000L and Rotoplas 5,000L)
- Composting toilets
- Nuestra Agua groundwater treatment systems (Los Ricos, Alonso Yáñez, San Elías)
- Cumulative totals by year — select a year to see everything installed through that point

**Data source:** `caminos_map_data.json` — built from Projects Private (2015–2023), Tecnologías registry (2024+), and Historic Data annual totals. Data is embedded directly in `index.html`.

**Data current through:** Projects Private May 24, 2026 · Tecnologías registry April 18, 2026 · Historic Data 2024 final / 2025 partial

**Note:** 49 RWH systems (3.3% of verified 2015–2024 total) have no community-level record and cannot be mapped. 5 communities have no GPS coordinates and appear in the sidebar only.

**Update method:** Manual annual rebuild — see SOP.

---

## Map 2: Technology Installation Registry (`registry.html`)

Individual system-level registry from the Tecnologías tracking sheet, covering 2020+.

**What it shows:**
- One dot per system installed (RWH, filters, composting toilets)
- Right sidebar: summary tables by technology, municipality, funder, and year; collapsible community list; community detail on click
- Filters by year, technology type, municipality, funder, collaborator, and project
- Photo links where available (Google Photos and Google Drive)

**Technology types:**
- SCALL — Ferrocemento (12,000L)
- SCALL — Rotoplas (5,000L)
- Filtro — Aguadapt
- Filtro — EOZ
- Filtro — Sawyer
- Sanitarios secos

**Data source:** `registry_data.json` — pushed nightly from the Registros - Tecnologías: Clean sheet via `registry_sync.gs` in Apps Script. `registry.html` fetches this file at runtime.

**Note:** Not all systems have GPS coordinates. Map shows placed systems only; all systems appear in the sidebar summary and search regardless of GPS.

**Update method:** Automatic nightly sync — no manual steps required. See SOP for fallback rebuild procedure.

---

## Dashboard: Procesos Comunitarios (`procesos.html`)

Community engagement and STAS/Nuestra Agua process log, 2023–present.

**What it shows:**
- One card per community with recorded process activity
- Last activity date with recency indicator (green/yellow/red)
- Activity breakdown by category (STAS Social, Técnico, Implementación, Taller, M&E, Reunión, etc.)
- Latest STAS/Nuestra Agua phase for communities in that process
- Partners and funders per community
- Full session log with notes, facilitator, participant count, and document links
- Filters by municipio, clase, partner, funder, and recency

**Data source:** `procesos_data.json` — pushed nightly from the Registros - Procesos Comunitarios sheet via `procesos_sync.gs` (called `syncToGitHub`) in Apps Script. `procesos.html` fetches this file at runtime.

**Note:** Covers 2023–present only. ~308 rows with missing municipio are flagged in the dashboard.

**Update method:** Automatic nightly sync — no manual steps required.

---

## Files in this repo

| File | Purpose | How updated |
|------|---------|-------------|
| `index.html` | Community map — HTML, CSS, and data embedded | Manual annual rebuild |
| `registry.html` | Registry map — fetches `registry_data.json` at runtime | Upload once; data is automatic |
| `registry_data.json` | Registry data | Automatic nightly (Apps Script) |
| `procesos.html` | Procesos dashboard — fetches `procesos_data.json` at runtime | Upload once; data is automatic |
| `procesos_data.json` | Procesos engagement data | Automatic nightly (Apps Script) |
| `caminos_map_data.json` | Data for community map (embedded in `index.html`) | Manual annual rebuild |
| `README.md` | This file | As needed |

---

## How to update

### Registry map and Procesos dashboard — automatic

Both tools update automatically every night via Apps Script triggers in the Caminos Project Dashboard Google Sheet. New data entered in either sheet tab appears on the live tool within 24 hours. No action required.

If a sync stops working, check the Apps Script execution log. Most common causes: GitHub token expired, sheet tab renamed, or column header changed. See the SOP for troubleshooting steps.

### Community map — manual, once per year

After Historic Data totals are finalized for the prior year, ask Claude to rebuild `caminos_map_data.json` and `index.html` using `build_final.py`. Upload both files to this repo. Do this once a year.

### Add/update content (Nuestra Agua details, community notes, photo links)

Ask Claude to update `caminos_map_data.json`, then rebuild `index.html`. Always upload both files together — data is embedded in the HTML, so updating the JSON alone has no effect.

---

## Embed on website (Squarespace)

Add a Code Block and paste the relevant iframe:

```html
<!-- Community map -->
<iframe src="https://Caminos-de-Agua.github.io/water-systems-map"
  width="100%" height="650px" style="border:none;border-radius:8px;"
  title="Mapa de sistemas de agua — Caminos de Agua"></iframe>

<!-- Technology registry -->
<iframe src="https://Caminos-de-Agua.github.io/water-systems-map/registry.html"
  width="100%" height="650px" style="border:none;border-radius:8px;"
  title="Registro de tecnologías — Caminos de Agua"></iframe>

<!-- Procesos dashboard -->
<iframe src="https://Caminos-de-Agua.github.io/water-systems-map/procesos.html"
  width="100%" height="650px" style="border:none;border-radius:8px;"
  title="Procesos comunitarios — Caminos de Agua"></iframe>
```

---

## Build scripts and sync files

### Stored in Claude project ("Caminos Project Records") — not needed on GitHub

- `build_final.py` — rebuilds `index.html` and `caminos_map_data.json`
- `build_registry.py` — manual fallback: rebuilds `registry.html` and `registry_data.json` from a CSV export
- `registry_template.html` — HTML/CSS/JS template used by `build_registry.py`

### Apps Script (bound to Caminos Project Dashboard Google Sheet)

- `registry_sync.gs` — nightly sync for registry map; mirrors `build_registry.py` transform logic
- `procesos_sync.gs` (`syncToGitHub`) — nightly sync for Procesos dashboard

Both scripts share one Apps Script project and one `GITHUB_TOKEN` script property.

---

Backup copies of all HTML, JSON, and script files are kept in Google Drive:  
**Communications → Sitio Web → Website 2.0 → Water Systems Maps**

Full operating procedures, troubleshooting, and maintenance checklist:  
**Water Systems Maps & Dashboards — SOP (Google Drive, same folder)**
