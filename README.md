# PAIGE Scanning Catalog

Public catalog of scan-app install info for **PAIGE** (Printer AI Guided Engineer), the printer
installer/troubleshooter app. Fetched at runtime by the app's Scanning screen, so a stale or wrong
value here can be fixed by editing a file and pushing — no app release needed.

This repo holds no source code, just one plain-text file per vendor. Nothing here is sensitive —
these are public vendor download pages/URLs and Microsoft Store product IDs, the same information
anyone could find by visiting each vendor's site directly. It's public specifically so the app can
fetch it via `raw.githubusercontent.com` without needing any credentials baked into the app.

## File format

One file per vendor, named `<vendor-key>.txt`, plain `Key=Value` lines. Blank lines and lines
starting with `#` are comments/ignored.

```
Mechanism=Winget | Download | Page
Id=<winget package id>              # Mechanism=Winget only
Url=<url>                           # Mechanism=Download or Page
Source=<winget source, e.g. msstore># Mechanism=Winget only, optional
```

- **Winget** — the vendor's app is distributed via the Microsoft Store; `winget install` resolves
  it straight to Microsoft's own servers.
- **Download** — the vendor ships a confirmed-stable, directly-downloadable installer file; the app
  downloads it and launches it (interactively, not silently).
- **Page** — no confirmed stable direct-download link; the app just opens this URL in the user's
  browser instead of guessing a file link that might be wrong.

**Not here anymore**: whether a vendor's app is already installed used to be a field in this file
(`AppSearchName`). Removed 8/27, after a real bug — Windows Scan's stored search term ("Windows
Scan") turned out to be wrong (its actual Start Menu name is just "Scan"), and a plain-text file
gave no way to catch a bad guess before it shipped. That check now lives in the app's own code
(`ScanAppInstallService.InstallChecks`) instead, since a confirmed install-check (ideally an exact
AppUserModelId) is a stable technical fact, unlike a download URL — it doesn't go stale on the
vendor's own schedule, so it doesn't need to be editable without a rebuild the way this catalog does.

See the source repo (`AI-Printer-Tool`, private) for the app code that reads this —
`ScanAppInstallService`.
