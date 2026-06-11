# IT Tracker Template

A lightweight, single-file tracker app (IT issues / equipment orders / phishing alerts) designed to be updated automatically by Claude via the GitHub API.

## Setup (10 minutes)
1. Create a new **public** GitHub repo (e.g. `my-tracker`)
2. Copy `template_index.html` into the repo as `index.html`
   - Replace `{{COMPANY_NAME}}`, `{{TRACKER_NAME}}`, and `{{PASSWORD}}` with your values
3. Copy `template_data.json` into the repo as `data.json`
4. Repo Settings -> Pages -> Deploy from branch -> main / root
5. Your app is live at `https://<username>.github.io/<repo>/`

## Claude auto-update setup
1. Create a fine-grained GitHub Personal Access Token:
   - Repository access: only the tracker repo
   - Permissions: Contents -> Read and write
2. Give the token to Claude in your project
3. Tell Claude to scan and push updates to `data.json` -- the app reads
   `data.json` (cache-busted) on every page load, so a refresh shows new data

## How it works
- `index.html` -- the whole app (UI, password gate, status menus)
- `data.json` -- the data; the ONLY file that changes day to day
- Status changes made in the browser are stored per-device in localStorage
  as overrides; Claude-pushed data is the source of truth
- Data structure per record type:
  - it:        id, name, description, date, status, threadUrl
  - equipment: id, assignedTo, eqType, description, status, date, notes, threadUrl
  - phishing:  id, name, attackerEmail, issue, date, status, threadUrl
- Statuses: IT = Open / In Progress / Closed; Equipment = Needs Ordering /
  Ordered / Received; Phishing = Open / Resolved

## Adapting to other use cases
The three tabs are just categories. To repurpose (e.g. safety incidents,
training requests, vehicle maintenance): rename the tab labels in index.html,
adjust the status lists at the top of the script, and keep the same data.json
shape.
