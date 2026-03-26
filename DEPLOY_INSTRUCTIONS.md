# Helm Add-ons — Deployment Instructions

This folder contains two Home Assistant add-ons that work together with your Helm Smart Home Dashboard:

- **Helm Bridge** (`helm-bridge/`) — Connects Home Assistant to Helm for device sync and control
- **Helm Supervisor** (`helm_supervisor/`) — Monitors Home Assistant health, logs, and diagnostics

---

## Step 1: Upload to GitHub

Your GitHub repository needs the exact folder structure shown here:

```
(your repo root)
├── repository.yaml
├── helm-bridge/
│   ├── config.yaml
│   ├── Dockerfile
│   ├── run.sh
│   ├── README.md
│   ├── translations/
│   │   └── en.yaml
│   └── rootfs/
│       └── usr/share/helm-bridge/
│           ├── package.json
│           ├── public/
│           │   └── index.html
│           └── dist/
│               └── index.js
└── helm_supervisor/
    ├── config.yaml
    ├── Dockerfile
    ├── build.yaml
    ├── package.json
    ├── README.md
    ├── CHANGELOG.md
    ├── icon.png
    ├── logo.png
    └── src/
        ├── index.js
        ├── config.js
        ├── ha-collector.js
        └── sync-client.js
```

**How to upload:**

1. Download this `helm-bridge-deploy` folder to your computer (or extract the zip)
2. Open your GitHub repository: `https://github.com/Mjolnir357/helm-bridge-addon`
3. Upload **all files from inside this folder** to the **root** of the repository
   - `repository.yaml` goes at the repo root
   - The `helm-bridge/` folder goes at the repo root
   - The `helm_supervisor/` folder goes at the repo root
4. Commit the changes with a message like `"Add Helm add-ons"`

> **Important:** `repository.yaml` must be at the repository root, not inside any subfolder.

---

## Step 2: Add the Repository to Home Assistant

1. Open Home Assistant
2. Go to **Settings** → **Add-ons** → **Add-on Store**
3. Click the three-dot menu (⋮) in the top right corner
4. Select **Repositories**
5. Add this URL:
   ```
   https://github.com/Mjolnir357/helm-bridge-addon
   ```
6. Click **Add**, then **Close**
7. Refresh the page — both add-ons will now appear in the store

---

## Step 3: Install and Configure Helm Bridge

The Helm Bridge connects your devices to the Helm dashboard.

1. Find **Helm Bridge** in the add-on store and click **Install**
2. Go to the **Configuration** tab and verify:
   - `cloud_url`: `https://helm-by-nautilink.replit.app` (default is correct)
   - `log_level`: `info` (default is fine)
3. Click **Save**
4. Go to the **Info** tab and click **Start**
5. Click **Log** and wait for a **pairing code** to appear (looks like `XXXX-XXXX`)

**Pairing with the Helm Dashboard:**

1. Open [https://helm-by-nautilink.replit.app](https://helm-by-nautilink.replit.app)
2. Log in to your account
3. Go to **Integrations** → **Home Assistant**
4. Click **Add Bridge**
5. Enter the pairing code from the add-on logs
6. Click **Pair** — your devices will begin syncing

> **Note:** Pairing codes expire after 10 minutes. Restart the add-on to get a new one.

---

## Step 4: Install and Configure Helm Supervisor

The Helm Supervisor sends monitoring data (logs, metrics, device states) to the Helm dashboard.

1. Find **Helm Supervisor** in the add-on store and click **Install**
2. Go to the **Configuration** tab and set:
   - `helm_url`: `https://helm-by-nautilink.replit.app`
   - `api_key`: Get this from the Helm dashboard under **Helm Supervisor** → **Register Instance**
   - `sync_interval`: `60` (seconds between syncs, range 10–600)
   - Leave the other toggles enabled
3. Click **Save**
4. Go to the **Info** tab and click **Start**

---

## Troubleshooting

### "No add-ons found" after adding repository
- Check that `repository.yaml` is at the **root** of the repo (not inside a subfolder)
- Confirm the folder names are exactly `helm-bridge` and `helm_supervisor` (note the underscore)
- Try removing and re-adding the repository URL in Home Assistant

### Bridge won't connect
- Check add-on logs for error messages
- Verify your internet connection from the Pi
- Confirm the `cloud_url` is correct
- Restart the add-on

### Pairing code not appearing
- Wait 30 seconds after starting — it appears after the first connection attempt
- If the code is missing, restart the add-on and check logs again

### Supervisor not syncing
- Verify both `helm_url` and `api_key` are set in the Configuration tab
- Check add-on logs for "No Helm URL or API key" warnings
- Re-register the instance in Helm to get a fresh API key

---

## Add-on Versions

| Add-on | Version |
|--------|---------|
| Helm Bridge | 1.3.3 |
| Helm Supervisor | 1.0.3 |
