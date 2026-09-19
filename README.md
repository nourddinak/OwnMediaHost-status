# Decoupled Out-of-Band Public Status Platform

An ultra-pro, zero-dependency, open-source status page and telemetry dashboard designed to survive primary infrastructure downtime, server crashes, and network blackouts. Deployable in 60 seconds to **GitHub Pages** (or Cloudflare Pages / Vercel / Netlify / VPS).

---

## Why Out-of-Band?

Hosting your status page on the same server as your application is an anti-pattern: when your application crashes or your VPS goes down, your status page crashes with it.

This status platform runs completely decoupled on **GitHub Pages' global CDN**:
- **Zero Hosting Costs**: 100% free hosting with unlimited global traffic on GitHub Pages.
- **Survives Datacenter Outages**: Operates independently of your primary server, cloud provider, or database.
- **Real-Time Client Probing**: Continuously probes your application backend (`/health` and `/health/ready`) to measure true round-trip network latency, database query times, and process uptime.
- **Unified 90-Day Timeline**: Slender, elegant 90-day operational timeline with calendar hover tooltips.
- **Zero Build Step**: Pure HTML5, CSS, and Vanilla JavaScript. No node_modules, no npm build commands, no server runtimes.

> [!TIP]
> **No Forking Required!** You can use this official hosted status page directly for your own OwnMediaHost instance without hosting anything:  
> `https://nourddinak.github.io/OwnMediaHost-status/?api=https://media.yourdomain.com`  
> Simply whitelist `https://nourddinak.github.io` on your server (or run `sudo bash /opt/ownmediahost/scripts/connect-status.sh "https://nourddinak.github.io/OwnMediaHost-status/"`).

## 🚀 Quickstart: Fork & Deploy Your Own (Optional)

### Step 1: Fork or Use as Template
1. Click **Use this template** or **Fork** at the top right of this repository.

### Step 2: Enable GitHub Pages
1. In your forked repository, go to **Settings** -> **Pages**.
2. Under **Build and deployment** -> **Source**, select **GitHub Actions**.
3. Go to the **Actions** tab -> click **Deploy Status Page to GitHub Pages** -> click **Run workflow**.
4. Your public status page is now live at `https://<your-username>.github.io/<repo-name>/`!

### Step 3 (Optional): Configure Custom Domain
1. In **Settings** -> **Pages** -> **Custom domain**, enter your domain (e.g. `status.yourdomain.com`).
2. Add a `CNAME` record in your DNS provider pointing `status.yourdomain.com` to `<your-username>.github.io`.

---

## 🔗 Connecting Your Web Application or API

The status page can monitor any backend API. You can connect it in 4 flexible ways:

### 1. Automated Script (OwnMediaHost VPS)
If you are running OwnMediaHost, execute the 1-click status connection tool on your server to automatically configure CORS origins, update environment variables, and run live probe tests:
```bash
sudo bash /opt/ownmediahost/scripts/connect-status.sh "https://<your-username>.github.io/OwnMediaHost-status/"
```

### 2. Preconfigure in `config.json`
Edit [`config.json`](config.json) in your repository:
```json
{
  "app_name": "My App Status",
  "api_endpoint": "https://api.yourdomain.com",
  "poll_interval_seconds": 30,
  "github_repo": "yourusername/status"
}
```

### 3. Connect Interactively in the Browser
1. Open your status page.
2. Click the **Target** pill in the top navigation bar (or the setup banner).
3. Paste your backend URL (e.g. `https://api.yourdomain.com`) and click **Test & Connect**.
4. The target is verified and saved to your browser session.

### 4. URL Query Parameter
Link users directly with your target API specified:
```text
https://yourusername.github.io/status/?api=https://api.yourdomain.com
```

### 5. Link from Your Main Application
In your primary application (settings page, footer, or 502/503 error handlers), provide a direct link to your out-of-band status page:
```html
<a href="https://status.yourdomain.com" target="_blank" rel="noopener">
  System Status
</a>
```

---

## 🛠 Backend API Compatibility

The status page expects a standard `/health` JSON endpoint with CORS headers (`Access-Control-Allow-Origin: *` or your status domain).

### Recommended Minimal Payload:
```json
{
  "status": "operational",
  "version": "1.0.0",
  "uptime_seconds": 3600,
  "database": {
    "status": "connected",
    "query_latency_ms": 0.35,
    "total_media_count": 42
  }
}
```

If your API returns a standard HTTP `200 OK`, the status engine will automatically measure round-trip latency and report all systems operational.

---

## 📢 Incident Feeds & Post-Mortems

To publish an active incident or historical post-mortem, edit [`incidents.json`](incidents.json):

```json
{
  "incidents": [
    {
      "id": "inc_2026_09_database",
      "title": "Scheduled Database Engine Upgrade",
      "status": "investigating",
      "severity": "maintenance",
      "updates": [
        {
          "timestamp": "2026-09-20 01:00 UTC",
          "status": "scheduled",
          "message": "Routine database maintenance in progress."
        }
      ]
    }
  ],
  "past_incidents": []
}
```

Commit and push to `main`. The GitHub Actions workflow will instantly update your live status page!

---

## 🛡 License
MIT License. Free for personal, commercial, and open-source use.
