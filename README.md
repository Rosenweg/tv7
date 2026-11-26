<p align="center"><img src="docs/logo.png" width="96" alt="Logo"></p>
<h1 align="center">Rosenweg TV-Playlist</h1>
<p align="center">Eine gemeinsame Senderliste (M3U) fürs Fernsehen im Rosenweg – für jede IPTV-App auf Fernseher, Handy oder PC.</p>
<p align="center"><a href="https://raw.githubusercontent.com/Rosenweg/tv7/main/playlist.m3u">Playlist</a> · <a href="docs/anleitung.md">Anleitung</a> · <a href="docs/entwickeln.md">Für Entwickler</a></p>

## Was drin ist

- Eine fertige Playlist mit Schweizer Sendern (SRF, RTS, RSI und weitere), täglich neu erzeugt.
- Quellen frei kombinierbar: Init7 TV, eigene M3U-Dateien, Xtream-Anbieter, go2rtc-Streams.
- Ein kleiner Xtream-Codes-Server auf Cloudflare Workers – Anmeldung mit Benutzer und Passwort statt URL.
- Senderlogos, Gruppen und EPG-Kennungen werden mitgeliefert.

## Läuft auf

Jede IPTV-App, die M3U oder Xtream Codes kann (z.B. IPTV Smarters Pro, VLC, Kodi, TiviMate).

## Entwickeln

Python-Skript `generate_playlist.py`, gesteuert über `config.yaml`; GitHub Actions erzeugt die
Playlist und rollt den Worker aus. Aufbau, Konfiguration und Secrets-Einrichtung stehen in
[docs/entwickeln.md](docs/entwickeln.md).

## Lizenz

Keine Lizenz hinterlegt – alle Rechte vorbehalten.
