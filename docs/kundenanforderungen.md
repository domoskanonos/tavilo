# Tavilo – Kundenanforderungen für Hörspiele

_Stand: 09.07.2026_

## Projektkontext

- **Produzent:** Dominik
- **Kunde / Auftraggeber:** Alex (vertritt die Marke Tavilo)
- **Marke:** Tavilo – Kinder tippen eine "Schlafkarte" an, die Geschichte startet automatisch
- **Wertversprechen:** "Eltern können für einen Moment durchatmen"
- **Test-Nutzer:** Nico (Dominiks Sohn)

## Zielgruppe & Format

| Parameter | Vorgabe |
|---|---|
| Alter | 3–7 Jahre |
| Länge pro Folge | 10–15 Minuten |
| Tonalität | warm, ruhig, leicht verträumt |
| Erzählstimme | stark, warm, natürlich (via TTS) |
| Sounddesign / FX | **keines** (vorerst, da TTS-limitiert) |
| Cliffhanger | **keine** |
| Aufregung am Ende | **keine** |
| Meditative Elemente | **keine** (ausdrücklicher Kundenwunsch) |

## Struktur jeder Folge

1. **Klangmotiv** (3–5 Sekunden) – akustische Wiedererkennung
2. **Einstiegsritual** – fester Intro-Text, schafft Verbindlichkeit
3. **Kleines Erlebnis der Hauptfigur** – niedrige Spannung, keine Konflikt-Eskalation
4. **Ruhige und sichere Auflösung** – natürlich, sanft, kein Showdown
5. **Ausstiegsritual** – fester Outro-Text, sanftes Ausklingen

**Wichtig:** Kinder sollen sich entspannen und einschlafen, nicht gespannt warten wie es weitergeht.

## Ausgewählte Stimmen

| Rolle | Stimme | Voice ID | Charakter |
|---|---|---|---|
| Weiblich | sanfte_erzaehlerin | `german_podcast_female_calm_01` | Sehr sanft, langsam, warm, natürlich |
| Männlich | liebevoller_vater | `german_audiobook_male_narrator_01` | Tief, warm, väterlich, authentisch |

## Fester Intro-Text

> Schön, dass du hier bist. Heute Abend erzähle ich dir eine Geschichte über **[Thema]**, die dich sanft in den Schlaf wiegt. Kuschel dich in deine Decke und hör einfach zu.

**Hinweis:** `[Thema]` wird durch das jeweilige Thema der Geschichte ersetzt.

## Fester Outro-Text

> Das war unsere Geschichte für heute. Schlaf gut und bis zum nächsten Mal.

## Klangmotiv

- **Dauer:** 3–5 Sekunden
- **Position:** Ganz am Anfang, vor dem Intro
- **Anforderung:** Muss bei jeder Geschichte exakt gleich bleiben (akustische Wiedererkennung)
- **Status:** Noch zu erstellen (Vorschläge: Spieluhr, Glockenspiel, Harfe)

## Sprachliche Richtlinien

- **Keine persönlichen Anreden** wie "Schatz", "mein Schatz" etc.
- **Kein formelles "Willkommen"** – stattdessen "Schön, dass du hier bist"
- **Natürliche, authentische Sprechweise** – nicht künstlich oder wie Radiosprecher
- **Eigenständige Folgen** in derselben Welt (Vertrautheit ohne Reihenfolge)
- **Wiederkehrende Hauptfigur** als Identifikationsfigur
- **Festes Setting** mit Wiedererkennungswert

## Akustische Konsistenz

- **Intro, Outro, Stimme und Klangmotiv müssen bei jeder Geschichte exakt gleich bleiben**
- Kinder reagieren sehr stark auf akustische Wiedererkennung
- Alle Audio-Dateien müssen konsistent in Qualität und Lautstärke sein

## Technische Anforderungen

- **Format:** MP3 (für App-Integration)
- **Qualität:** Hohe Bitrate für klare Audioqualität
- **Workflow:** Git-basierte Versionierung der Audio-Dateien
- **TTS-Engine:** QuisyTTS (Qwen-TTS 1.7B)

## Referenzen

- **Sleep Tight Stories (Kanada):** Vorbild – einfache Schlafgeschichten mit minimalem Produktionsaufwand
- **Tonies Schlummerband:** Negativer Kontrast – setzt stärker auf Schlafmusik, Tavilo geht den Weg der ruhigen Erzählung
- **Traumreise:** Nicos aktuelles Hörspiel – enthält Meditations-Elemente, die Tavilo **nicht** haben soll

## Offene Punkte

- [ ] Aktive oder passive Ansprache der Kinder ("Stell dir vor..." vs. rein passiv erzählt)
- [ ] Sprache: nur Deutsch? (potentiell später weitere Sprachen)
- [ ] Audio-Spec: Loudness-Ziel (z.B. -16 LUFS), Normung über alle Folgen
- [ ] Anzahl Folgen pro erstem Batch
- [ ] Lieferzyklus / Deadline für erste Folge
- [ ] Abnahme-/Feedbackschleife definieren
- [ ] Auslieferungskanal: nur Tavilo-System oder parallel Spotify?
- [ ] Feedbackstruktur pro Folge (wie viele Revisionsrunden?)
- [ ] Testlauf mit Kindern als Abnahmekriterium?
- [ ] Klangmotiv erstellen (3–5 Sekunden)
- [ ] Erste Testgeschichte produzieren
- [ ] Telefonat zur KI-Workflow-Demo (nächste Woche)
