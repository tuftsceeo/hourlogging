# CEEO Hour Logging

A self-hosted time tracking system for CEEO student employees.

## What this is

- **index.html** — the student-facing web app (deploy to GitHub Pages at hours.tuftsceeo.org)
- **Code.gs** — the Google Apps Script backend (handles auth, data storage, weekly report)

---

## Setup (one time)

### Step 1 — Set up the Google Apps Script

1. Go to [script.google.com](https://script.google.com) and click **New project**
2. Delete any existing code and paste in the contents of `Code.gs`
3. Click the floppy disk icon to save (name it "CEEO Hours")
4. In the toolbar, select the function **setupSpreadsheet** from the dropdown
5. Click **Run** and accept the permissions when prompted
   - This creates the Google Sheet with all tabs and fake student data
   - It also sets up the Sunday 7am weekly report trigger
6. Check **Logs** (View > Logs) to find the URL of the newly created Sheet

### Step 2 — Deploy the web app

1. Click **Deploy > New deployment**
2. Click the gear icon next to "Type" and select **Web app**
3. Set:
   - **Execute as:** Me
   - **Who has access:** Anyone
4. Click **Deploy**
5. Copy the **Web app URL** that appears

### Step 3 — Connect the frontend

1. Open `index.html`
2. Find the line: `const SCRIPT_URL = 'PASTE_YOUR_APPS_SCRIPT_URL_HERE';`
3. Replace `PASTE_YOUR_APPS_SCRIPT_URL_HERE` with the URL from Step 2

### Step 4 — Deploy to GitHub Pages

1. Create a GitHub repository (e.g. `ceeo-hours`)
2. Upload `index.html` to the repository
3. Go to Settings > Pages, set source to main branch
4. Point `hours.tuftsceeo.org` DNS to GitHub Pages (CNAME record)

---

## Demo account

Email: `test@tuftsceeo.org`  
Password: `password`

---

## Managing students

Open the Google Sheet that was created. The **Students** tab has:

| Column | Purpose |
|--------|---------|
| Email | Student's Tufts email |
| Name | Display name |
| Affiliation | e.g. "Student employee" |
| Project1 | Primary project (required) |
| Project2 | Second project (optional) |
| Project3 | Third project (optional) |
| Password | Only needed for demo/test accounts |

Students only see the projects assigned to them. Add/remove rows any time.

---

## Weekly report

Every Sunday at 7am, an email is sent to the admin address set in `Code.gs`:

```
var REPORT_EMAIL = 'admin@tuftsceeo.org';
```

Change this to the real admin email before going live. The report matches the format of the Kuali report: one row per person per project, with hours.

---

## Updating the deployed web app

If you ever need to update `Code.gs`:
1. Make your changes in the Apps Script editor
2. Click **Deploy > Manage deployments**
3. Click the pencil icon on your existing deployment
4. Change version to **New version**
5. Click **Deploy**

The URL stays the same.
