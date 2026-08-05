# Jackson College IT Asset Manager

Campus-wide hardware tracking system for the Jackson College IT department. Tracks assets through their full lifecycle — intake, deployment, repair, retirement — and runs the loaner desk and imaging bench alongside it.

Single-file web app (all HTML/CSS/JS in one file) backed by Firebase Firestore for real-time sync.

## Features

**Asset tracking** — add, edit, deploy, duplicate, and bulk-edit assets. Custom categories, statuses, and storage bins, all reorderable via drag-and-drop.

**Barcode scanning** — USB HID scanners (Honeywell, Zebra, etc.) work plug-and-play as keyboard input, no drivers needed. Camera-based QR/barcode scanning works on phones and tablets. Configurable barcode prefix rules (e.g. `730823` → `N730823`) so scans normalize automatically.

**Bulk Scan Import** — set a shared template (model, category, building), then scan a batch of new assets in sequence. Duplicate tags are rejected automatically.

**Check-out desk / loaner pool** — short-term items (webcams, chargers, hotspots) with check-out/check-in tracking, borrower name, notes, and due dates. Handled exclusively through the Student Worker Portal.

**Student Worker Portal** — separate entry point from the login screen for student staff to run loaner check-in/out without full asset-manager access.

**Bench imaging queue** — tracks devices in the imaging/loading pipeline, assigned to a bench tech.

**Role-based access** — Admin, Technician, Viewer, and Guest roles, plus custom roles with granular permission mixes. Supervisor lock on protected assets (bypassable by Supervisor/Lead Tech roles, or via PIN).

**Reports & export** — live inventory snapshot (totals, value, category/status breakdowns, checked-out loaners), CSV export, and full JSON database backup.

**Danger Zone** — irreversible admin actions (clear activity feed, force check-in all loaners, delete all assets), password-gated.

**Audit tool** — scan-driven asset audit workflow.

**Activity feed** — logs check-ins/outs and other key events in real time.

## Tech Stack

- Vanilla JS, HTML, CSS — no build step, no framework
- Firebase Firestore (`firebase-firestore-compat`) for data storage and real-time listeners
- Firebase Auth (`firebase-auth-compat`)
- Loaded via Firebase JS SDK v10.12.0 from CDN (`gstatic.com/firebasejs`)

## Getting Started

1. Clone the repo.
2. Open `Jackson_College_IT_Asset_Manager_20.html` and drop in your Firebase project config (`§ Firebase Config` section near the top of the script).
3. Open the file in a browser — no build/install step required. For production use, host it behind a proper web server (not `file://`) so Firebase auth and camera scanning work correctly.
4. Set an admin password and configure staff/roles under **Settings → Staff & Roles**.

## Project Structure

This is intentionally a single-file app. The script is organized into labeled sections (search for `§` to jump between them):

```
Theme Init → Firebase Config → Global App State → USB Scanner Setup
→ Scan Routing → Firebase Listeners/Helpers → View Navigation
→ Loading Tracker → Dashboard → Asset CRUD → Bulk Actions → Selection
→ Triage → Loaner CRUD → Check-In/Out → Asset Detail Panel
→ Settings Lists → Status Manager → Export & Backup → CSV Import
→ Bulk Scan Import → Horizontal Tabs → Helpers & Renderers
→ Drag & Drop → Assignee Tags
```

## Roles

| Role | Access |
|---|---|
| Admin | Full access — settings, delete, manage users, Danger Zone |
| Technician | Add, edit, deploy, repair, retire, imaging bench, audits — no admin settings or delete |
| Viewer | Browse inventory and reports only |
| Guest | Read-only, no account needed — always available on login screen |

Custom roles can be created with any combination of permissions.

## Notes

- No backend beyond Firebase — all app logic runs client-side.
- Barcode prefix rules and quick-scan behavior are configurable in Settings.
- Full database backups (assets, loaners, imaging queue, staff directory, settings) can be exported as JSON from Settings.
