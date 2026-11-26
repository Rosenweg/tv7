# TV-Playlist – Technik

Technische Details, aus dem README hierher verschoben.

## Features

- ✅ **Xtream Codes API Server** - Eigene API auf Cloudflare Workers (serverless & kostenlos!)
- ✅ **Xtream Codes Client** - Integration von IPTV-Anbietern mit Xtream API
- ✅ **Init7 TV** - Automatischer Import aller Init7 TV-Kanäle
- ✅ **Lokale M3U-Dateien** - Beliebige M3U-Dateien aus `sources/` Ordner
- ✅ **go2rtc Streams** - Integration eigener Streams
- ✅ **GitHub Actions** - Automatische tägliche Aktualisierung
- ✅ **TVG-Attribute** - Unterstützung für Logos, Gruppen, EPG-IDs
- ✅ **Einfache Konfiguration** - Alles über YAML konfigurierbar

## Struktur

```
.
├── .github/
│   └── workflows/
│       ├── generate-playlist.yml    # Playlist erzeugen (täglich, bei Push, manuell)
│       └── deploy-worker.yml        # Cloudflare Worker deployen (bei Push auf cloudflare-worker/)
├── cloudflare-worker/               # Xtream Codes API Server (Cloudflare Workers)
│   ├── worker.js                    # Serverless API Implementation
│   ├── wrangler.toml               # Worker Konfiguration
│   ├── README.md                    # Deployment-Anleitung
│   └── GITHUB_SECRETS_SETUP.md      # Secrets für das automatische Deployment
├── sources/                         # M3U-Quelldateien (beliebige *.m3u Dateien)
│   ├── TV7_HLS.m3u                 # M3U-Quelle (HLS)
│   ├── srg-fhd-hls.m3u             # M3U-Quelle (HLS)
│   └── README.md                    # Dokumentation
├── config.yaml                      # Konfigurationsdatei
├── generate_playlist.py             # Generator-Script
├── requirements.txt                 # Python-Dependencies
└── playlist.m3u                     # Generierte Playlist (automatisch erstellt)
```

## Verwendung

### Automatisch (GitHub Actions)

Die Playlist wird automatisch generiert:
- **Täglich** um 6:00 UTC
- Bei **Push** auf main/master (wenn config.yaml oder generate_playlist.py geändert wurden)
- **Manuell** über GitHub Actions UI

Die generierte `playlist.m3u` wird automatisch ins Repository committed.

Der Cloudflare Worker wird von `deploy-worker.yml` automatisch deployed, sobald
sich etwas unter `cloudflare-worker/` ändert (oder manuell). Die nötigen
GitHub Secrets stehen in
[`cloudflare-worker/GITHUB_SECRETS_SETUP.md`](../cloudflare-worker/GITHUB_SECRETS_SETUP.md).

### Lokal

```bash
# Dependencies installieren
pip install -r requirements.txt

# Playlist generieren
python generate_playlist.py
```

## Konfiguration

Bearbeiten Sie `config.yaml` um die Playlist anzupassen:

### Init7 Streams (API)

```yaml
init7:
  enabled: true
  url: "https://api.init7.net/tvchannels.m3u?rp=true"
```

**Hinweis:** Die Init7 API ist oft nur für Init7-Kunden erreichbar. Verwenden Sie stattdessen lokale M3U-Dateien im `sources/` Ordner.

### Lokale M3U-Dateien

```yaml
sources:
  enabled: true
  directory: "sources"
```

Legen Sie beliebige `*.m3u` Dateien im `sources/` Verzeichnis ab. Alle Dateien werden automatisch eingelesen.

### Xtream Codes API

Falls Sie einen IPTV-Anbieter mit Xtream Codes API haben:

```yaml
xtream:
  enabled: true
  server: "http://your-server.com:8080"
  username: "your_username"
  password: "your_password"
  include_live: true      # Live TV Kanäle
  include_vod: false      # Video on Demand
  include_series: false   # Serien
```

**Verwendung in IPTV Smarters Pro:**
- Diese Daten können Sie direkt in IPTV Smarters Pro eingeben
- Oder Sie nutzen die generierte M3U-Playlist (siehe unten)

### go2rtc Streams (optional)

Falls Sie go2rtc verwenden, können Sie zusätzliche Streams in der `config.yaml` konfigurieren. Details siehe `config.yaml`.
