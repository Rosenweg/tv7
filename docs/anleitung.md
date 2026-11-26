# TV-Playlist – Anleitung

So wird die Playlist in einer IPTV-App genutzt (aus dem README hierher verschoben).

## Playlist verwenden

### 🎯 Option 1: Xtream Codes API Server (EMPFOHLEN für Vidaa OS!)

**Vorteile:**
- ✅ Login mit Username/Password statt URL
- ✅ Automatische Kategorisierung
- ✅ Bessere User Experience in IPTV Apps
- ✅ Komplett kostenlos (Cloudflare Workers)

**Setup:**
1. Siehe [`cloudflare-worker/README.md`](../cloudflare-worker/README.md) für Deployment
2. Nach Deployment erhalten Sie eine URL: `https://xtream-api.ihre-subdomain.workers.dev`
3. In IPTV Smarters Pro:
   - Wählen Sie **"Login with Xtream Codes API"**
   - Server: `https://xtream-api.ihre-subdomain.workers.dev`
   - Username/Password: (wie in worker.js konfiguriert)

### 📺 Option 2: M3U Playlist URL

**Für alle IPTV Apps (VLC, Kodi, TiviMate, etc.)**

1. In IPTV Smarters Pro: Wählen Sie "Load Your Playlist or File/URL"
2. Geben Sie die URL ein: `https://raw.githubusercontent.com/Rosenweg/tv7/main/playlist.m3u`

### URL zur generierten Playlist

Die Playlist ist unter folgender URL verfügbar:

```
https://raw.githubusercontent.com/Rosenweg/tv7/main/playlist.m3u
```

Diese URL funktioniert in allen IPTV-Apps (VLC, Kodi, TiviMate, etc.)

## Init7 TV-Kanäle

Die Playlist enthält alle verfügbaren Init7 TV-Kanäle:

- 🇩🇪 **Deutsch**: SRF 1, SRF zwei, SRF info
- 🇫🇷 **Français**: RTS 1, RTS 2
- 🇮🇹 **Italiano**: RSI LA 1, RSI LA 2

Und weitere Kanäle je nach Init7-Verfügbarkeit.

## Links

- [Init7 TV](https://www.init7.net/de/tv/)
- [Init7 Support](https://www.init7.net/de/support/)
