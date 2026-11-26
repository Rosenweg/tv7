<p align="center"><img src="docs/logo.png" width="96" alt="Logo"></p>
<h1 align="center">Rosenweg TV-Playlist</h1>
<p align="center">Eine gemeinsame Senderliste (M3U) fürs Fernsehen im Rosenweg – für jede IPTV-App auf Fernseher, Handy oder PC.</p>
<p align="center"><a href="https://raw.githubusercontent.com/Rosenweg/tv7/main/playlist.m3u">Playlist</a> · <a href="docs/anleitung.md">Anleitung</a></p>

## Inhalt

- [Überblick](#überblick)
- [Plattformen](#plattformen)
- [Bedienung](#bedienung)
- [Sender](#sender)
- [Aufbau](#aufbau)
- [Playlist erzeugen](#playlist-erzeugen)
- [CI und Ausrollen](#ci-und-ausrollen)
- [Konfiguration](#konfiguration)
- [Lizenz](#lizenz)

## Überblick

Das Repo erzeugt jeden Tag eine fertige Playlist mit Schweizer Sendern und stellt sie auf zwei Arten bereit:

- **M3U-Adresse** – funktioniert in jeder IPTV-App.
- **Xtream-Codes-Server** auf Cloudflare Workers – Anmeldung mit Benutzer und Passwort statt einer langen URL, mit automatischen Kategorien. Empfohlen für Fernseher mit Vidaa OS.

Die Quellen lassen sich frei kombinieren: Init7 TV, eigene M3U-Dateien, ein Xtream-Anbieter und go2rtc-Streams. Senderlogos, Gruppen und EPG-Kennungen werden mitgeliefert.

## Plattformen

| Plattform | Stand |
|---|---|
| IPTV Smarters Pro (auch auf Vidaa-Fernsehern) | ja, per Xtream-Login oder M3U |
| VLC, Kodi, TiviMate und andere IPTV-Apps | ja, per M3U-Adresse |

## Bedienung

| Weg | So geht's |
|---|---|
| M3U-Adresse | In der App „Playlist/URL laden“ wählen und `https://raw.githubusercontent.com/Rosenweg/tv7/main/playlist.m3u` eingeben |
| Xtream-Login | In IPTV Smarters Pro „Login with Xtream Codes API“ wählen, als Server die Worker-Adresse eintragen, dazu Benutzer und Passwort |

Benutzer und Passwort für den Xtream-Login gibt die Technik heraus.

Mehr: [docs/anleitung.md](docs/anleitung.md).

## Sender

Aus Init7 TV, je nach Verfügbarkeit:

| Sprache | Sender |
|---|---|
| Deutsch | SRF 1, SRF zwei, SRF info |
| Französisch | RTS 1, RTS 2 |
| Italienisch | RSI LA 1, RSI LA 2 |

Dazu die Dateien in `sources/` (z.B. TV7 und SRG in Full HD) und weitere Kanäle je nach Init7-Angebot.

## Aufbau

| Teil | Inhalt |
|---|---|
| `generate_playlist.py` | Generator: liest alle Quellen und schreibt `playlist.m3u` |
| `config.yaml` | Welche Quellen aktiv sind, Ausgabe, Gruppen, TVG-Attribute |
| `sources/` | Beliebige `*.m3u`-Dateien, alphabetisch eingelesen ([sources/README.md](sources/README.md)) |
| `playlist.m3u` | Die erzeugte Playlist (wird automatisch eingecheckt) |
| `cloudflare-worker/` | Xtream-Codes-Server (`worker.js`, `wrangler.toml`), liest immer die neueste Playlist von GitHub ([Anleitung](cloudflare-worker/README.md)) |

## Playlist erzeugen

```bash
pip install -r requirements.txt
python generate_playlist.py
```

Hinweis: Die Init7-API ist oft nur für Init7-Kunden erreichbar – dann besser lokale M3U-Dateien in `sources/` verwenden.

## CI und Ausrollen

| Workflow | Auslöser | Ergebnis |
|---|---|---|
| `generate-playlist.yml` | täglich 6:00 UTC, Push mit Änderungen an `config.yaml` oder `generate_playlist.py`, von Hand | neue `playlist.m3u` wird eingecheckt |
| `deploy-worker.yml` | Push mit Änderungen unter `cloudflare-worker/`, von Hand | Worker wird auf Cloudflare ausgerollt |

## Konfiguration

**Quellen in `config.yaml`:**

| Abschnitt | Wozu |
|---|---|
| `init7` | Init7-Senderliste (`enabled`, `url`) |
| `sources` | Verzeichnis mit eigenen M3U-Dateien |
| `xtream` | Externer Xtream-Anbieter (`server`, `username`, `password`, Live/VOD/Serien) – Zugangsdaten nie einchecken |
| `go2rtc` | Eigene Streams über go2rtc (`base_url`, `streams`) |
| `output` | Dateiname, Gruppen, TVG-Attribute |

**GitHub-Secrets** für den Worker, nur die Namen (Einrichtung: [cloudflare-worker/GITHUB_SECRETS_SETUP.md](cloudflare-worker/GITHUB_SECRETS_SETUP.md)):

| Secret | Wozu |
|---|---|
| `XTREAM_USERNAME`, `XTREAM_PASSWORD` | Login am Xtream-Server (ein Benutzer) |
| `XTREAM_CREDENTIALS` | Alternative: mehrere Benutzer |
| `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID` | Ausrollen auf Cloudflare Workers |
| `PLAYLIST_URL`, `WORKER_URL` | optional, eigene Adressen statt der Vorgaben |

Mehr: [docs/entwickeln.md](docs/entwickeln.md).

## Lizenz

GPL-3.0-or-later, siehe [LICENSE](LICENSE).
