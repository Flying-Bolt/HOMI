# HOMI

[![Latest Release](https://img.shields.io/github/v/release/Flying-Bolt/HOMI)](https://github.com/Flying-Bolt/HOMI/releases/latest)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)

**Dein Smart-Home auf dem Handgelenk.** HOMI ist eine Wear-OS-App (plus Smartphone-Begleiter), mit der du Geräte in deinem Zuhause per einfachem HTTP-Aufruf direkt von der Uhr steuerst — Türöffner, Licht, Steckdosen, Rollläden, und mehr. Funktioniert mit allem, was sich über eine URL ansprechen lässt: **Tasmota, Shelly, ioBroker (simple-api)** und ähnliche.

<p align="center">
  <img src="docs/watch-grid.png" alt="HOMI auf der Galaxy Watch" width="320">
</p>

> Dies ist die öffentliche **Download- & Anleitungs-Seite**. Der Quellcode liegt in einem separaten, privaten Repository.

---

## Was HOMI kann

- **Frei konfigurierbare Buttons** auf der Uhr, in Gruppen organisiert (horizontal wischen).
- **Drei Button-Modi:**
  - **Tap** – ein Druck löst eine URL aus (z. B. Tür öffnen, Szene starten).
  - **Schalter** – Ein/Aus mit zwei URLs und optionaler Status-Anzeige (Button färbt sich: voll = Ein, gedimmt = Aus).
  - **Wert** – zeigt einen Live-Wert aus einer URL an (z. B. Batterie-Ladestand, Temperatur), per Regex extrahiert.
- **Konfiguration am Smartphone** (Begleiter-App): Buttons anlegen, Icon/Farbe/URL wählen, testen — und per Knopfdruck an die Uhr senden.
- **Eigenständig auf der Uhr** – läuft auch ohne gekoppeltes Phone weiter, solange WLAN da ist.
- **Lokal & privat** – kein Cloud-Dienst, kein Tracking. HOMI spricht nur die Adressen an, die du selbst einträgst.

---

## Download

Alle Releases findest du unter **[Releases](https://github.com/Flying-Bolt/HOMI/releases/latest)**. Es gibt zwei APKs:

| Datei | Gerät |
|---|---|
| `HOMI-mobile-*.apk` | Android-Smartphone (Begleiter-App zur Konfiguration) |
| `HOMI-wear-*.apk`   | Galaxy Watch / Wear-OS-Gerät |

> ℹ️ Die APKs sind **mit dem Standard-Android-Debug-Key signiert**, damit du sie ohne Play-Store-Schlüssel direkt installieren kannst. Für die private Nutzung im eigenen Netz ist das völlig in Ordnung.

---

## Installation

### 1. Phone-App (Smartphone)

1. Auf dem Phone die [Release-Seite](https://github.com/Flying-Bolt/HOMI/releases/latest) öffnen und die `HOMI-mobile-*.apk` herunterladen.
2. Im Browser/Datei-Manager auf die APK tippen.
3. Android fragt nach **„Installation aus unbekannten Quellen"** – der jeweiligen App (z. B. Chrome, Files) die Erlaubnis erteilen.
4. Installieren → App startet als **HOMI**.

### 2. Watch-App (Galaxy Watch / Wear OS)

Wear OS bietet **keinen direkten APK-Installer auf der Uhr** – die App wird per ADB „sideloaded". Einmalig etwas Aufwand, danach läuft's.

**Vorbereitung auf der Uhr:** Einstellungen → Watch-Info → Software-Info → **Build-Nummer 7× tippen** (schaltet Entwickleroptionen frei) → Entwickleroptionen → **WLAN-Debugging** aktivieren.

**Auf dem PC** (mit installierten [Android Platform-Tools](https://developer.android.com/tools/releases/platform-tools)):

```bash
# 1. PC und Uhr im gleichen WLAN. Auf der Uhr:
#    Entwickleroptionen → WLAN-Debugging → "Gerät mit Pairing-Code koppeln"
#    -> zeigt IP:PORT (Pairing) und einen 6-stelligen Code.
adb pair <IP>:<PAIRING-PORT>        # Code eingeben

# 2. Verbinden (der WLAN-Debugging-Hauptschirm zeigt einen ANDEREN IP:PORT):
adb connect <IP>:<DEBUG-PORT>

# 3. APK aus dem Release installieren:
adb -s <IP>:<DEBUG-PORT> install -r HOMI-wear-*.apk
```

> Pairing-Port und Debug-Port sind **zwei verschiedene Ports**. Bei älteren Wear-Geräten (vor Wear OS 4) entfällt der Pairing-Schritt – dort genügt `adb connect <IP>:<PORT>`.

---

## Erste Schritte

1. **HOMI** auf dem Phone öffnen.
2. Eine **Gruppe** anlegen (z. B. „Türen", „Licht") und darin **Buttons** hinzufügen.
3. Pro Button: Icon, Farbe, **Modus** (Tap / Schalter / Wert) und die **URL(s)** eintragen.
   - Beispiel Tasmota (Schalter): `http://192.168.x.x/cm?cmnd=Power%20On` / `…Power%20Off`
   - Beispiel ioBroker (Status lesen): `http://192.168.x.x:8087/get/<datenpunkt>`
4. Mit **„Testen"** prüfen, ob die URL antwortet.
5. Oben auf das **Senden-Symbol (✈)** tippen → die Konfiguration wandert auf die Uhr.
6. Fertig: Auf der Uhr die Buttons antippen.

> **Tipp:** Bei „Schalter" mit Status-Anzeige immer eine **`/get/`**-URL als Status-Quelle nutzen (nicht `/set/`), und die Erkennungs-Regex an das Antwortformat anpassen (z. B. `"val":true` / `"val":false`).

---

## Hinweise & Einschränkungen

- HOMI steuert **lokale Geräte im eigenen Netz** (HTTP). Externe/HTTPS-URLs gehen ebenfalls, wenn das Zielgerät sie unterstützt.
- Im **Energiesparmodus** schaltet Wear OS das WLAN ab und blockiert Apps daran, es zu reaktivieren – HOMI kann das nicht umgehen und zeigt dann einen Hinweis.
- Keine Telemetrie, keine Cloud, keine Konten. Deine URLs und Tokens bleiben auf deinen Geräten.

---

## Lizenz

[Apache License 2.0](LICENSE) — siehe auch [NOTICE](NOTICE).

<sub>🤖 App & Doku mitentwickelt mit <a href="https://claude.com/claude-code">Claude Code</a> (Opus 4.8).</sub>
